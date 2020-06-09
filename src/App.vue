<template>
  <div id="app">
    <DVDScreen :start="start"/>
    <button @click="startDVD">start/stop</button>
  </div>
</template>

<script>
import DVDScreen from "@/components/DVDScreen.vue";
export default {
  name: "app",
  components: {
    DVDScreen
  },
  data() {
    return {
      start: false
    };
  },
  methods: {
    startDVD() {
      this.start = !this.start;
    }
  }
};
</script>