<template>
  <div class="first-screen">
	<the-canvas-background class="canvas"></the-canvas-background>
    <h1 v-html="fText"></h1>
  </div>
</template>

<script>
import theCanvasBackground from './ui/the-canvas-background.vue';
export default {
  components: { theCanvasBackground },
  name: "FirstScreen",
  data() {
    return {
      fText: "Hi,</br> Im Vlad,</br> front-end developer",
    };
  },
};
</script>

<style lang="scss" scoped>
.first-screen{
	min-height: 100vh;
	display: flex;
	align-items: center;
}
h1 {
  margin: 0;
  font-weight: 600;
  font-size: 90px;
  line-height: 120%;
}
.canvas{
	position: absolute;
	top: 0;
	left: 0;
	z-index: -1;
}
</style>