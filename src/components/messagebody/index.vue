<template>
  <div class="messagebody">
    <div class="bodyname">
      {{ username }}
    </div>
    <div class="his_message">
      <img ref="yourImgDom" src="" alt="" />
    </div>

    <div class="send_message">
      <div class="action-bar">
        <!-- 表情 -->
        <div class="action-item">
          <div v-if="emoji.visible" class="emoji-box">
            <img
              v-for="(emojiItem, emojiKey, index) in emoji.map"
              class="emoji-item"
              :key="index"
              :src="emoji.url + emojiItem"
            />
          </div>
          <i
            class="iconfont icon-smile"
            title="表情"
            @click="showEmoji = !showEmoji"
          ></i>
        </div>
        <!-- 图片 -->
        <div class="action-item">
          <ImgUpload for="img-input" @input="filelist"></ImgUpload>
        </div>
        <!-- 视频 -->
        <div class="action-item">
          <VideoUpload for="video-input" @input="filelist"></VideoUpload>
        </div>
        <!-- 文件 -->
        <div class="action-item">
          <label for="file-input">
            <FileUpload for="video-input" @input="filelist"></FileUpload>
          </label>
        </div>
        <div class="action-item">
          <label for="file-input">
            <i @click="screen" class="iconfont icon-paizhao" title="表情"></i>
          </label>
        </div>
      </div>
      <Emoji class="emojibox" @emoji_click="emoji" v-show="showEmoji" />
      <el-input
        type="textarea"
        ref="inputArea"
        class="textar"
        @blur="handleInputBlur"
        :rows="5"
        v-model="textarea"
        maxlength="200"
        refs="drag-input"
      >
      </el-input>
    </div>
  </div>
</template>

<script>
import Emoji from "../../components/emoji";
import ImgUpload from "../../components/Imageupload";
import VideoUpload from "../../components/Videoupload";
import FileUpload from "../../components/FileUpload";
import { ipcRenderer, desktopCapturer } from "electron";

export default {
  components: { Emoji, ImgUpload, VideoUpload, FileUpload },
  data() {
    return {
      username: "USERNAME",
      showEmoji: false,
      textarea: "",
      cursorIndex: "",
    };
  },
  mounted() {},
  methods: {
    emoji(index) {
      console.log("父组件", index);
      var s1 = this.textarea;
      var s2 = this.textarea;
      var len = this.textarea.length;
      this.textarea = "";
      this.textarea =
        s1.substring(0, this.cursorIndex) +
        index +
        s2.substring(this.cursorIndex, len);
      this.showEmoji = false;
    },
    filelist(index) {
      console.log("父元素", index);
    },
    handleInputBlur(e) {
      this.cursorIndex = e.srcElement.selectionStart;
    },
    resize(newRect) {
      this.width = newRect.width;
      this.height = newRect.height;
      this.top = newRect.top;
      this.left = newRect.left;
    },
    activateEv(index) {
      console.log("activateEv" + index);
      this.$refs["drag-input"].focus();
    },
    screen() {
      var yourImgDom = this.$refs.yourImgDom;
      console.log(yourImgDom);
      desktopCapturer
        .getSources({
          types: ["screen"],
          thumbnailSize: { width: 1920, height: 1080 }, //实际屏幕尺寸可以通过主进程的screen模块获得
        })
        .then((imgs) => {
          yourImgDom.src = imgs[0].thumbnail.toDataURL();
          ipcRenderer.invoke("showWindow"); //屏幕拍照完成后，再显示这个窗口，不然它也会被拍进去
        });
    },
  },
};
</script>

<style scoped>
.messagebody {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.bodyname {
  width: calc(100% -20px);
  height: 40px;
  text-align: left;
  padding-left: 20px;
  line-height: 40px;
  /* background-color: #eee; */
  border-bottom: 1px solid #d6d6d6;
}
.his_message {
  height: calc(100% - 200px);
  width: 100%;
  overflow: auto;
}
.send_message {
  position: relative;
  width: 100%;
  height: 200px;
  background-color: #fff;
  border-top: 1px solid #d6d6d6;
}
/deep/ .el-textarea__inner {
  width: 100%;
  height: 100%;
  resize: none;
  border: none !important;
}
.textar {
  width: 100% !important;
  height: 100% !important;
  overflow: auto;
}
.textar::-webkit-scrollbar {
  width: 10px;
}
.textar::-webkit-scrollbar-thumb {
  border-radius: 5px;
  background: #bab9b9;
}
.textar::-webkit-scrollbar-button {
  width: 10px;
  border-radius: 50%;
  background: black;
  display: none;
}
.drag_top {
  position: absolute;
  width: 100%;
  top: 0;
  left: 0;
  border-top: 1px solid red;
  cursor: n-resize;
}
.action-bar {
  display: flex;
  flex-direction: row;
  padding: 0 10px;
}

.action-bar .action-item {
  text-align: left;
  padding: 10px 0;
  position: relative;
}

.action-bar .action-item .iconfont {
  font-size: 22px;
  margin: 0 10px;
  z-index: 3;
  color: #606266;
  cursor: pointer;
}

.action-bar .action-item .iconfont:focus {
  outline: none;
}

.action-bar .action-item .iconfont:hover {
  color: #d02129;
}

.emoji-box {
  width: 210px;
  position: absolute;
  top: -111px;
  left: -11px;
  z-index: 2007;
  background: #fff;
  border: 1px solid #ebeef5;
  padding: 12px;
  text-align: justify;
  font-size: 14px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  word-break: break-all;
  border-radius: 4px;
}

.emoji-item {
  width: 38px;
  height: 38px;
  margin: 0 2px;
}

.input-box {
  padding: 0 10px;
  flex: 1;
}

.input-content {
  border: none;
  resize: none;
  display: block;
  padding: 5px 15px;
  box-sizing: border-box;
  width: 100%;
  color: #606266;
  outline: none;
  background: #ffffff;
  word-break: break-all;
}

.send-box {
  padding: 5px 10px;
  text-align: right;
}

.send-button {
  width: 70px;
  height: 30px;
  font-size: 15px;
  border: 1px solid #d02129;
  background-color: #ffffff;
  color: #d02129;
  border-radius: 5px;
}

.action-popup {
  width: 850px;
  height: 100%;
  position: absolute;
  top: 0;
  left: -281px;
  background: rgba(51, 51, 51, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-popup-main {
  width: 150px;
  height: 120px;
  background: #ffffff;
  z-index: 100;
  border-radius: 10px;
  overflow: hidden;
}

.action-popup-main .action-item {
  text-align: center;
  line-height: 40px;
  font-size: 15px;
  color: #262628;
  border-bottom: 1px solid #efefef;
  cursor: pointer;
}

.image-preview {
  max-width: 750px;
  max-height: 500px;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  margin: auto;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 9998;
}

.image-preview img {
  max-width: 750px;
  max-height: 500px;
}

.image-preview .close {
  font-size: 50px;
  line-height: 24px;
  cursor: pointer;
  color: #ffffff;
  position: absolute;
  top: 10px;
  right: 5px;
  z-index: 1002;
}

.emojibox {
  position: absolute;
  top: -320px;
}
</style>