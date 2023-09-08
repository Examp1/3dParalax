<template>
  <!-- <img src="@/assets/images/1.jpg" alt=""> -->
  <div class="container">
    <section class="gallery">
      <div class="frame">
        <div class="frame__content">
          <h2>Beautiful World</h2>
        </div>
      </div>

      <div class="frame">
        <div class="frame__content">
          <img
            class="frame-media frame-media_left"
            src="@/assets/images/1.jpg"
          />
        </div>
      </div>

      <div class="frame frame_bg">
        <div class="frame__content">
          <video
            class="frame-media frame-media_right"
            src="@/assets/media/video_optimized.mp4"
            autoplay
            loop
            muted
          ></video>
        </div>
      </div>

      <div class="frame"></div>

      <div class="frame">
        <div class="frame__content text-right">
          <h3>Pure planet</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur elit adipisicing. Nemo
            fugit, rerum dolorem assumenda consequatur dicta error iure
            laboriosam temporibus.
          </p>
        </div>
      </div>

      <div class="frame frame_bg">
        <div class="frame__content">
          <img
            class="frame-media frame-media_left"
            src="@/assets/images/2.jpg"
          >
        </div>
      </div>

      <div class="frame"></div>

      <div class="frame frame_bg">
        <div class="frame__content">
          <img
            class="frame-media frame-media_right"
            src="@/assets/images/3.jpg"
          >
        </div>
      </div>

      <div class="frame"></div>

      <div class="frame">
        <div class="frame__content text-left">
          <h3>Ask the Mountains</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur elit. Rerum dolorem
            assumenda consequatur dicta error iure laboriosam temporibus omnis
            quae eaque aliquam esse unde accusamus dolores non soluta.
          </p>
        </div>
      </div>

      <div class="frame frame_bg">
        <div class="frame__content">
          <img
            class="frame-media frame-media_right"
            src="@/assets/images/4.jpg"
          >
        </div>
      </div>

      <div class="frame">
        <div class="frame__content">
          <video
            class="frame-media frame-media_left"
            src="@/assets/media/video_optimized.mp4"
            autoplay
            loop
            muted
          ></video>
        </div>
      </div>

      <div class="frame"></div>
      <div class="frame"></div>

      <div class="frame frame_bg">
        <div class="frame__content">
          <img
            class="frame-media frame-media_right"
            src="@/assets/images/5.jpg"
          >
        </div>
      </div>

      <div class="frame frame_bg">
        <div class="frame__content">
          <video
            class="frame-media"
            src="@/assets/media/video_optimized.mp4"
            autoplay
            loop
            muted
          ></video>
        </div>
      </div>

      <div class="frame"></div>
      <div class="frame"></div>

      <div class="frame">
        <div class="frame__content">last</div>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  name: "home-page",
  mounted() {
    let zSpacing = -1000,
      lastPos = zSpacing / 5,
      $frames = document.querySelectorAll(".frame"),
      zVals = []; 
  

    window.onscroll = () =>  { 
      let top = document.documentElement.scrollTop, 
        delta = lastPos - top;

      lastPos = top;

     
      $frames.forEach((el, i) => {
       
        zVals.push(i * zSpacing + zSpacing);
        zVals[i] += delta * -5.5;
        let frame = $frames[i],
          transform = `translateZ(${zVals[i]}px)`,
          opacity = zVals[i] < Math.abs(zSpacing) / 1.8 ? 1 : 0;
        frame.setAttribute(
          "style",
          `transform: ${transform}; opacity: ${opacity}`
        );
      });
    };

    window.scrollTo(0, 1);

    // Audio

    let soundButton = document.querySelector(".soundbutton"),
      audio = document.querySelector(".audio");

    soundButton.addEventListener("click", () => {
      soundButton.classList.toggle("paused");
      audio.paused ? audio.play() : audio.pause();
    });

    window.onfocus = function () {
      soundButton.classList.contains("paused") ? audio.pause() : audio.play();
    };

    window.onblur = function () {
      audio.pause();
    };
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
:root {
  --index: calc(1vw + 1vh);
  --gutter: 30px;
  --side-small: 26;
  --side-big: 36;
  --depth: 4000px;
  --transition: 0.75s cubic-bezier(0.075, 0.5, 0, 1);
}

/* Скрываем Scrollbar */
body {
  scrollbar-width: none; /* Firefox */
}
body::-webkit-scrollbar {
  display: none; /* Safari and Chrome */
}

@font-face {
  font-family: raleway_c;
  src: url(@/assets/fonts/raleway-v22-cyrillic-300.woff2);
  font-weight: 300;
}
@font-face {
  font-family: raleway_c;
  src: url(@/assets/fonts/raleway-v22-cyrillic-100.woff2);
  font-weight: 100;
}
body {
  background-color: #000;
  color: #fff;
  font-size: calc(var(--index) * 0.8);
  font-family: raleway_c, sans-serif;
  line-height: 1.75;
  height: var(--depth);
  font-weight: 300;
}
.container {
  width: 100%;
  height: 100%;
  position: fixed;
  perspective: 1500px;
}
.gallery {
  transform-style: preserve-3d;
  height: 100%;
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
h1,
h2,
h3,
h4 {
  font-weight: 100;
  text-transform: uppercase;
  width: min-content;
  line-height: 1;
}
.frame h2 {
  text-align: center;
  font-size: calc(var(--index) * 3.3);
}
.frame-media {
  position: relative;
  width: calc(var(--index) * var(--side-small));
  height: calc(var(--index) * var(--side-big));
  background-position: center;
  background-size: cover;
}
.frame-media_left {
  right: calc(var(--side-small) / 2 * var(--index) + var(--gutter));
}
.frame-media_right {
  left: calc(var(--side-small) / 2 * var(--index) + var(--gutter));
}
.frame_bg {
  background-color: rgb(0 0 0 / 0.87);
}
video.frame-media {
  width: calc(var(--index) * var(--side-big));
  height: calc(var(--index) * var(--side-small));
}
video.frame-media_right {
  left: calc(var(--side-big) / 2 * var(--index) + var(--gutter));
}
video.frame-media_left {
  right: calc(var(--side-big) / 2 * var(--index) + var(--gutter));
}
.text-right > * {
  position: relative;
  left: 18vw;
}
.text-left > * {
  position: relative;
  right: 18vw;
}
.frame h3 {
  font-size: calc(var(--index) * 3);
}
.frame p {
  max-width: 30vw;
  margin-top: 3vh;
}
.soundbutton {
  position: fixed;
  bottom: 5vh;
  right: 5vw;
  cursor: pointer;
  width: 24px;
  transition: 0.25s ease;
}
.soundbutton.paused {
  opacity: 0.25;
}
</style>
