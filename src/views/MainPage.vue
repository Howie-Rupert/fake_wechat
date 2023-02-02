<template>
  <!-- <div @click="back_pg">mainpage</div> -->
  <el-container>
    <el-aside class="el_aside" width="300px">
      <a_side />
    </el-aside>
    <el-container>
      <el-header class="el_header"> <drag /> </el-header>

      <el-main>Main</el-main>
    </el-container>
  </el-container>
</template>

<script>
const { ipcRenderer } = window.require("electron");
import drag from "../components/drag";
import a_side from "../components/aside";
export default {
  components: { drag, a_side },
  data() {
    return {};
  },
  mounted() {
    ipcRenderer.send("LoginSuccess");
  },
  methods: {
    back_pg() {
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
.el_header {
  position: relative;
}
.el_aside {
  height: 100%;
  min-height: 100vh;
}
</style>