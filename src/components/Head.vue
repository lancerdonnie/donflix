<template>
  <div class="hea">
    <Header></Header>
    <template v-if="isThere()">
      <transition name="trans">
        <div
          v-if="original[0].show"
          class="showcase"
          :style="{ backgroundImage: 'url(' + original[0].myVid + ')' }"
        >
          <Details :detail="original[0]"></Details>
        </div>
      </transition>
      <transition name="trans">
        <div
          v-if="original[1].show"
          class="showcase"
          :style="{ backgroundImage: 'url(' + original[1].myVid + ')' }"
        >
          <Details :detail="original[1]"></Details>
        </div>
      </transition>
      <transition name="trans">
        <div
          v-if="original[2].show"
          class="showcase"
          :style="{ backgroundImage: 'url(' + original[2].myVid + ')' }"
        >
          <Details :detail="original[2]"></Details>
        </div>
      </transition>
      <div class="sliders">
        <div @click="setShow(0)" class="slider" :class="{ opac: original[0].show }"></div>
        <div @click="setShow(1)" class="slider" :class="{ opac: original[1].show }"></div>
        <div @click="setShow(2)" class="slider" :class="{ opac: original[2].show }"></div>
      </div>
    </template>
  </div>
</template>

<script>
import Header from "./Header.vue";
import Details from "./Details";
import { reactive, onBeforeUnmount, computed } from "vue";
import { useStore } from "vuex";

export default {
  name: "Head",
  components: { Header, Details },
  setup() {
    const store = useStore();
    const state = reactive({ skipCount: 1 });
    const original = computed(() => store.state.nowPlaying);

    store.dispatch("nowPlaying");

    const skip = () => {
      setShow(state.skipCount);
      state.skipCount === 2 ? (state.skipCount = 0) : state.skipCount++;
    };

    const interval = setInterval(skip, 7000);

    const setShow = (e) => store.commit("SET_SHOW", e);

    const isThere = () => {
      const x = store.state.nowPlaying;
      return x ? true : false;
    };

    onBeforeUnmount(() => clearInterval(interval));

    return { state, setShow, isThere, original };
  },
};
</script>

<style scoped>
.hea {
  position: relative;
  height: 100vh;
  width: 100%;
  overflow: hidden;
}
.showcase {
  position: absolute;
  height: 100%;
  width: 100%;
  background-size: cover;
  z-index: 0;
  background-color: black;
}
.showcase::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  background: rgba(0, 0, 0, 0.5);
  box-shadow: inset 80px -100px 100px #000, inset -80px 0px 100px #000;
}
span {
  font-size: 2.5rem;
  color: white;
}
.sliders {
  display: flex;
  justify-content: space-between;
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 190px;
  z-index: 2;
}
.slider {
  cursor: pointer;
  height: 3px;
  width: 55px;
  z-index: 10;
  background-color: white;
  opacity: 0.2;
}
.opac {
  opacity: 1;
}

.trans-enter {
  transform: translateX(100vw);
}
.trans-enter-to {
  transition: all 1s ease;
  transform: translateX(0);
}
.trans-leave {
  transform: translateX(0);
}
.trans-leave-to {
  transform: translateX(-100vw);
  transition: all 1s ease;
}
</style>
