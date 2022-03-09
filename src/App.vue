<template>
  <div id="app">
    <fractal-quill />
  </div>
</template>

<script>
import FractalQuill from "./components/FractalQuill.vue";
// eslint-disable-next-line no-unused-vars
import FractalQuillZoid from "./utils/zoid";

export default {
  name: "FuFractalQuill",
  components: { FractalQuill },
};
</script>
