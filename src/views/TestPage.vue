<template>
  <div>
    <div class="anim3d">
      <div class="container">
        <div class="gallery">
          <div class="frame" v-for="i in 10" :key="i">{{ i * 100 }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  mounted() {

    let zSpacing = -1000,
      lastPos = zSpacing / 5,
      $frames = document.querySelectorAll(".frame"),
      zVals = Array.from({ length: $frames.length }).map(
        (_, i) => i * zSpacing + zSpacing
      );

    window.onscroll = () => {
      let top = document.documentElement.scrollTop,
        delta = lastPos - top;

      lastPos = top;

      $frames.forEach((_, idx) => {
        zVals[idx] += delta * -5;
        let frame = $frames[idx],
          transform = `translateZ(${zVals[idx]}px)`,
          opacity = zVals[idx] < Math.abs(zSpacing) / 1.8 ? 1 : 0;

        frame.setAttribute(
          "style",
          `transform: ${transform}; opacity: ${opacity}`
        );
      });
    };
    window.scrollTo(0, 1);
  },
};
</script>

<style lang="scss">
:root {
  --index: calc(1vw + 1vh);
  --side-x: 26;
  --side-y: 36;
  --gutter: 30px;
  --transition: 0.75s cubic-bezier(0.075, 0.5, 0, 1);
//   --transition: 0.75s cubic-bezier( 0, 0.96, 0.45, 0.61 );
  --depth: 4000px;
}
</style>

<style lang="scss" scoped>
.anim3d {
  background-color: #000;
  color: #fff;
  font-size: calc(var(--index) * 0.8);
  height: var(--depth);
}
.container {
  width: 100%;
  height: 100%;
  position: fixed;
  perspective: 1500px;
}
.gallery {
  height: 100%;
  transform-style: preserve-3d;
}
.frame {
  width: 100%;
  height: 100%;
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: var(--transition), opacity 0.75s ease;
  will-change: transform;
  transform-style: preserve-3d;
}
</style>