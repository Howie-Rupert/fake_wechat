<template>
  <div class="demo-image__preview">
    <el-image-viewer v-if="showViewer" :url-list="url" />
  </div>
</template>

<script>
import { ipcRenderer } from "electron";
export default {
  data() {
    return {
      url: [],
      showViewer: true,
    };
  },

  components: {
    "el-image-viewer": () =>
      import("element-ui/packages/image/src/image-viewer"),
  },
  mounted() {
    var data = localStorage.getItem("data");
    this.url = [data];
  },
  methods: {},
};
</script>

<style scoped>
/deep/.el-image-viewer__close {
  display: none !important;
}
</style>