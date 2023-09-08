<template>
  <div>
    <canvas ref="cometCanvas"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      canvas: null,
      ctx: null,
      config: {
        star: {
          color: 'rgba(255, 255, 255, .5)',
          width: 3,
          randomWidth: true
        },
        line: {
          color: 'rgba(255, 255, 255, .5)',
          width: 0.2
        },
        position: {
          x: 0,
          y: 0
        },
        width: window.innerWidth,
        height: window.innerHeight,
        velocity: 0.1,
        length: window.innerWidth / 6,
        distance: 120,
        radius: window.innerWidth / 5,
        stars: []
      }
    };
  },
  mounted() {
    this.canvas = this.$refs.cometCanvas;
    this.ctx = this.canvas.getContext('2d');
    this.initStars();
    this.bindEvents();
  },
  beforeDestroy() {
    this.unbindEvents();
  },
  methods: {
    initStars() {
      this.canvas.width = this.config.width;
      this.canvas.height = this.config.height;
      this.ctx.fillStyle = this.config.star.color;
      this.ctx.strokeStyle = this.config.line.color;
      this.ctx.lineWidth = this.config.line.width;
      this.createStars();
      this.animateStars();
    },
    createStars() {
      for (let i = 0; i < this.config.length; i++) {
        const star = this.createStar();
        star.create();
        this.config.stars.push(star);
      }
    },
    createStar() {
      return {
        x: Math.random() * this.canvas.width,
        y: Math.random() * this.canvas.height,
        vx: (this.config.velocity - (Math.random() * 0.5)),
        vy: (this.config.velocity - (Math.random() * 0.5)),
        radius: this.config.star.randomWidth ? (Math.random() * this.config.star.width) : this.config.star.width,
        create: () => {
          this.ctx.beginPath();
          this.ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
          this.ctx.fill();
        }
      };
    },
    animateStars() {
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

      for (const star of this.config.stars) {
        star.create();
        star.x += star.vx;
        star.y += star.vy;
      }

      requestAnimationFrame(this.animateStars);
    },
    bindEvents() {
      window.addEventListener('mousemove', this.handleMouseMove);
      window.addEventListener('resize', this.handleResize);
    },
    unbindEvents() {
      window.removeEventListener('mousemove', this.handleMouseMove);
      window.removeEventListener('resize', this.handleResize);
    },
    handleMouseMove(e) {
      this.config.position.x = e.clientX - this.canvas.offsetLeft;
      this.config.position.y = e.clientY - this.canvas.offsetTop;
    },
    handleResize() {
      this.canvas.width = window.innerWidth;
      this.canvas.height = window.innerHeight;
    }
  }
};
</script>

<style scoped>
canvas {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
