<template >
  <div class="frame">
    <div v-if="youtube">
      <i @click="$emit('trailer-close')" class="fa fa-times" aria-hidden="true"></i>
      <iframe
        :src="youtube"
        :width="width||'85%'"
        :height="height||'400px'"
        frameborder="0"
        allowfullscreen
      ></iframe>
    </div>
    <div v-else class="load"></div>
  </div>
</template>

<script>
export default {
  name: "Trailer",
  props: ["youtube"],
};
</script>

<style scoped>
.frame {
  display: flex;
  justify-content: center;
  position: relative;
  z-index: 2;
}
.load {
  background-color: white;
}
i {
  position: absolute;
  right: 10px;
  top: 10px;
}
</style>