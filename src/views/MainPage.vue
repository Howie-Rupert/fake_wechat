<template>
  <!-- <div @click="back_pg">mainpage</div> -->
  <el-container>
    <el-header> <drag /> </el-header>
    <el-container>
      <el-aside width="200px"> 
        <a_side /> 
        </el-aside>
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

<style>
</style>