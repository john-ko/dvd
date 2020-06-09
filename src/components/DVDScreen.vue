<template>
  <div class="dvd-screen" :style="getGameStyles">
    <DVD :top="posY" :left="posX" :backgroundColor="color"/>
  </div>
</template>

<script>
import DVD from "@/components/DVD.vue";
export default {
  name: "DVDScreen",
  props: {
    height: {
      type: Number,
      default: 600
    },
    width: {
      type: Number,
      default: 800
    },
    dvdHeight: {
      type: Number,
      default: 87
    },
    dvdWidth: {
      type: Number,
      default: 159
    },
    colors: {
      type: Array,
      default: () => {
        return ["red", "orange", "yellow", "green", "blue", "purple"];
      }
    },
    start: {
      type: Boolean,
      default: false
    },
    startX: {
      type: Number,
      default: 0
    },
    startY: {
      type: Number,
      default: 0
    }
  },
  watch: {
    start: function() {
      if (this.start) {
        this.interval = setInterval(() => {
          this.advanceOneFrame();
        }, 10);
      } else {
        clearInterval(this.interval);
      }
    }
  },
  components: {
    DVD
  },
  data() {
    return {
      posX: 0,
      posY: 0,
      deltaX: 1,
      deltaY: 1,
      color: "",
      interval: null
    };
  },
  created() {
    // setup
    this.changeColor();
    this.posX = this.startX;
    this.posY = this.startY;
  },
  mounted() {
    // game starts
  },
  computed: {
    getGameStyles() {
      return {
        height: `${this.height}px`,
        width: `${this.width}px`
      };
    }
  },
  methods: {
    advanceOneFrame() {
      this.handleDirectionChange();
      this.posX += this.deltaX;
      this.posY += this.deltaY;
    },
    handleDirectionChange() {
      let toggled = false;
      // checks X position
      if (
        this.isPositionOutOfBounds(
          this.posX,
          this.deltaX,
          this.dvdWidth,
          this.width
        )
      ) {
        this.toggleDeltaX();
        toggled = true;
      }
      // checks Y position
      if (
        this.isPositionOutOfBounds(
          this.posY,
          this.deltaY,
          this.dvdHeight,
          this.height
        )
      ) {
        this.toggleDeltaY();
        toggled = true;
      }
      // if either one or both toggled
      // change color
      if (toggled) {
        this.changeColor();
      }
    },
    isPositionOutOfBounds(pos, delta, length, bounds) {
      const position = pos + delta;
      // if position is lower than 0
      if (position < 0) {
        return true;
      }
      // else check if out of bounds
      return position + length > bounds;
    },
    toggleDeltaX() {
      this.deltaX *= -1;
    },
    toggleDeltaY() {
      this.deltaY *= -1;
    },
    changeColor() {
      // picks a new color
      let color = this.getRandomColor();
      while (this.color === color) {
        color = this.getRandomColor();
      }
      this.color = color;
    },
    getRandomColor() {
      return this.colors[Math.floor(Math.random() * this.colors.length)];
    }
  }
};
</script>

<style>
.dvd-screen {
  position: relative;
  margin: 0 auto;
  background-color: #212121;
}
</style>