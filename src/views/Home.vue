<template>
  <!-- <img src="@/assets/images/1.jpg" alt=""> -->
  <div class="anim3d">
    <div class="container" ref="main">
      <section class="gallery" ref="innerWrp">
        <div
          v-for="(frame, idx) in frames"
          :key="frame.src + '' + idx || frame.content + '' + idx"
          class="frame"
          ref="frame"
          :class="{
            frame_bg: frame.type === 'image' || frame.type === 'video',
          }"
        >
          <div
            class="frame__content"
            :class="frame.position ? `text-${frame.position}` : ''"
          >
            <template v-if="frame.type === 'text'">
              <h3>{{ frame.content }}</h3>
              <p v-if="frame.description">{{ frame.description }}</p>
            </template>

            <img
              v-if="frame.type === 'image'"
              :src="frame.src"
              :class="['frame-media', `frame-media_${frame.position || ''}`]"
            />

            <video
              v-if="frame.type === 'video'"
              :src="frame.src"
              :class="['frame-media', `frame-media_${frame.position || ''}`]"
              autoplay
              loop
              muted
            ></video>
          </div>
        </div>
      </section>
    </div>
    <img class="soundbutton paused" src="@/assets/images/sound.gif" alt="Alt" />
    <audio class="audio" src="@/assets/media/ambient.mp3" loop></audio>
  </div>
</template>
<script>
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);
export default {
  name: "home-page",
  data() {
    return {
      frames: [
        { type: "text", content: "Beautiful World" },
        {
          type: "image",
          src: require("@/assets/images/1.jpg"),
          position: "left",
        },
        {
          type: "video",
          src: require("@/assets/media/video_optimized.mp4"),
          position: "right",
        },
        // {},
        {
          type: "text",
          content: "Lorem ipsum",
          description:
            "Lorem ipsum dolor sit amet, consectetur elit adipisicing. Nemo fugit, rerum dolorem assumenda consequatur dicta error iure laboriosam temporibus.",
          position: "right",
        },
        {
          type: "image",
          src: require("@/assets/images/2.jpg"),
          position: "left",
        },
        // {},
        {
          type: "image",
          src: require("@/assets/images/3.jpg"),
          position: "right",
        },
        // {},
        {
          type: "text",
          content: "Ask the Mountains",
          description:
            "Lorem ipsum dolor sit amet, consectetur elit adipisicing. Nemo fugit, rerum dolorem assumenda consequatur dicta error iure laboriosam temporibus.",
          position: "left",
        },
        {
          type: "image",
          src: require("@/assets/images/4.jpg"),
          position: "right",
        },
        {
          type: "video",
          src: require("@/assets/media/video_optimized.mp4"),
          position: "left",
        },
        // {},
        {
          type: "image",
          src: require("@/assets/images/5.jpg"),
          position: "right",
        },
        {
          type: "video",
          src: require("@/assets/media/video_optimized.mp4"),
          position: "center",
        },
        // {},
        // {},
        {
          type: "text",
          content: "last frame",
          description:
            "Lorem ipsum dolor sit amet, consectetur elit adipisicing. Nemo fugit, rerum dolorem assumenda consequatur dicta error iure laboriosam temporibus.",
          position: "center",
        },
      ],
      zSpacing: -1000,
      zVals: [],
    };
  },

  computed: {
    animateHeight() {
      return this.frames.length;
    },
  },

  mounted() {
    // const frames = document.querySelectorAll(".frame");
    this.zVals = Array.from({ length: frames.length }).map(
      (_, i) => i * this.zSpacing + this.zSpacing
    );

    const mainWrp = this.$refs.main,
      innerWrp = this.$refs.innerWrp;

    const scrollZ = Math.abs(this.zVals[this.zVals.length - 1]) + 500;

    gsap.to(innerWrp, {
      translateZ: scrollZ,
      scrollTrigger: {
        scrub: 2,
        pin: ".container",
        trigger: mainWrp,
        start: "top center",
        end: `${scrollZ}`,
        // onUpdate: () => {
        //   frames.forEach((_, idx) => {
        //     let frame = frames[idx],
        //       opacity = this.zVals[idx] < this.zSpacing / -1.7 ? 1 : 0;
        //     frame.setAttribute(
        //       "style",
        //       `opacity: ${opacity}`
        //     );
        //   });
        // },
      },
    });

    this.startPostitonInit()
    
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
  methods: {
    startPostitonInit() {
      if ( !this.$refs.frame ) return
      this.$refs.frame.forEach((_, idx) => {
        let frame = frames[idx],
          transformZ = `translateZ(${this.zVals[idx]}px)`,
          opacity = this.zVals[idx] < this.zSpacing / -1.7 ? 1 : 0;
        frame.setAttribute(
          "style",
          `transform: ${transformZ}; opacity: ${opacity}`
        );
      });
    },
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
  // --depth: 4000px;
  --transition: 0.75s cubic-bezier(0.075, 0.5, 0, 1);
}

/* Скрываем Scrollbar */
// body {
//   scrollbar-width: none; /* Firefox */
// }
// body::-webkit-scrollbar {
//   display: none; /* Safari and Chrome */
// }

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
.anim3d {
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
  // position: fixed;
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
  right: calc(var(--side-small) / 2 * var(--index) + var(--gutter)) !important;
}
.frame-media_right {
  left: calc(var(--side-small) / 2 * var(--index) + var(--gutter)) !important;
}
.frame_bg {
  background-color: rgb(0 0 0 / 0.87);
}
video.frame-media {
  width: calc(var(--index) * var(--side-big));
  height: calc(var(--index) * var(--side-small));
}
video.frame-media_right {
  left: calc(var(--side-big) / 2 * var(--index) + var(--gutter)) !important;
}
video.frame-media_left {
  right: calc(var(--side-big) / 2 * var(--index) + var(--gutter)) !important;
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
