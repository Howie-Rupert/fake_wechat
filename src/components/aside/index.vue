<template>
  <div class="aside">
    <div class="content">
      <div class="messages" v-for="item in messagelist">
        <div class="messages_item">
          <el-avatar :size="45" :src="item.usericon"></el-avatar>
          <div class="texts">
            <div>{{ item.nickname }}</div>
            <div>{{ item.message }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      messagelist: [],
      userlist: [],
      userid: "",
    };
  },
  mounted() {
    this.userid = window.localStorage.getItem("userid");
    console.log(window.localStorage.getItem("userid"));
    // this.getuserlist();

    this.getmessagelist();
  },
  methods: {
    // getuserlist() {
    //   axios({
    //     url: "http://www.test.com:8083/friendlist.php",
    //     params: {
    //       userid: this.userid,
    //     },
    //     method: "get",
    //   }).then((res) => {
    //     console.log("friendlist", res);
    //     res.data.data.forEach((item) => {
    //       var sq = item.sq_userid;
    //       var bsq = item.bsq_userid;
    //       if (this.userid != sq) {
    //         axios({
    //           url: "http://www.test.com:8083/search.php",
    //           headers: { "Content-Type": "application/x-www-form-urlencoded" },
    //           method: "post",
    //           data: {
    //             userid: sq,
    //           },
    //         }).then((res_i) => {
    //           this.userlist.push(res_i.data.data[0]);
    //         });
    //       } else if (this.userid != bsq) {
    //         axios({
    //           url: "http://www.test.com:8083/search.php",
    //           headers: { "Content-Type": "application/x-www-form-urlencoded" },
    //           method: "post",
    //           data: {
    //             userid: bsq,
    //           },
    //         }).then((res_i) => {
    //           this.userlist.push(res_i.data.data[0]);
    //         });
    //       }
    //     });
    //     console.log(this.userlist);
    //   });
    // },
    getmessagelist() {
      var userid = window.localStorage.getItem("userid");
      axios({
        url: "http://www.test.com:8083/message.php",
        method: "post",
        headers: { "Content-Type": "application/x-www-form-urlencoded" },
        data: {
          userid,
        },
      }).then((res) => {
        console.log("消息列表", res);
        res.data.data.forEach((item) => {
          if (item.from_userid != userid) {
            axios({
              url: "http://www.test.com:8083/search.php",
              headers: { "Content-Type": "application/x-www-form-urlencoded" },
              method: "post",
              data: {
                userid:item.from_userid
              },
            }).then((res_i) => {
                item.usericon = res_i.data.data[0].usericon
                item.nickname = res_i.data.data[0].nickname
            });
          }else if(item.to_userid != userid){
            axios({
              url: "http://www.test.com:8083/search.php",
              headers: { "Content-Type": "application/x-www-form-urlencoded" },
              method: "post",
              data: {
                userid:item.to_userid
              },
            }).then((res_i) => {
                item.usericon = res_i.data.data[0].usericon
                item.nickname = res_i.data.data[0].nickname
            });
          }
        });
        this.messagelist = res.data.data
        console.log(this.messagelist)
      });
    },
  },
};
</script>

<style scoped>
.aside {
  width: calc(100% - 1px);
  height: calc(100vh - 60px);
  border-right: 1px solid #eee;
  overflow: hidden;
}
.content {
  height: auto;
  display: flex;
  flex-direction: column;
}
.messages_item {
  width: 100%;
  display: flex;
  flex-direction: row;
  margin: 10px;
}
.texts {
  width: 60%;
  margin-left: 10px;
  padding-top: 2.6px;
}
.texts > * {
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  text-align: left;
}
</style>