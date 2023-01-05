<template>
    <div @click="back_pg">mainpage</div>
</template>

<script>
const { ipcRenderer } = window.require("electron");
export default {
    mounted(){
        ipcRenderer.send('LoginSuccess');
    },
    methods:{
        back_pg(){
            this.$router.push('/')
        }
    }
}
</script>

<style>

</style>