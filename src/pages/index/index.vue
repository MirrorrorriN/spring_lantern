<template>
    <div class="container">
        <!-- <div class="message">{{msg}}</div> -->
        <div class="box">
            <img src="/static/images/lantern.png" mode="widthFix">
        </div>
        <!-- <div class="pop-container" v-if="showRiddle">
        {{riddle}}
        </div>-->
        <div class="userinfo-bar">
            <div>
                <img class="userinfo-avatar" :src="userInfo.avatarUrl">
                <!-- <img class="userinfo-avatar" src="/static/images/unlog_avatar.jpg" v-else> -->
            </div>
            <div>
                <span>{{userInfo.nickName}}</span>
            </div>
            <div style="color:#f03232;">猜中{{correctCount}}条</div>
        </div>
        <div class="riddle-bar" v-if="correct!=0||!showRiddle">
             <div v-if="correct===0&&!showRiddle" @click="getSingleRiddle">点击抽取</div>
            <div v-else-if="correct===1||correct===2" @click="getSingleRiddle">再抽一条</div>
        </div>
        <div class="pop-container" v-if="showRiddle">
            <div class="question">
                <!-- <img src="/static/images/lace.jpeg" mode="widthFix"> -->
                <div>{{riddle}}</div>
                <!-- <img src="/static/images/lace.jpeg" mode="widthFix"> -->
                <div>{{hint}}</div>
            </div>
            <div class="answer">
                <!-- <input type="text" confirm-type="search" v-model="answer" placeholder="请输入谜底" @confirm="confirm($event)" v-on:input="watchAnswer"> -->
                <input
                    v-if="correct==0"
                    type="text"
                    confirm-type="done"
                    v-model="userAnswer"
                    placeholder="猜猜看"
                    @confirm="confirm()"
                    class="answer-input"
                >
                <!-- <button class="confirm-btn" @click="handleClick">太简单了！提交~</button> -->
                <div class="scroll" v-else-if="correct==1">
                    <div>答对了!</div>
                    <div>{{explanation}}</div>
                </div>
                <div class="scroll" v-else>不对哦~再试试</div>
            </div>
        </div>
    </div>
</template>

<script>
import { get, getStorageOpenid, post } from "../../utils/wx-request";
import { toLogin, login } from "../../utils";
export default {
    data() {
        return {
            msg: "Hello",
            riddle: "日出北京城",
            hint: "",
            showRiddle: false,
            answer: "",
            userAnswer: "",
            userInfo: wx.getStorageSync("userInfo"),
            correctCount: 0,
            openid: wx.getStorageSync("openid"),
            correct: 0,
            explanation:""
        };
    },

    onload(){
        wx.loadFontFace({
          family: 'webfont',
          source: 'url("//at.alicdn.com/t/webfont_mjy7t30ao5.eot")',
        //   source: 'url("//down.izihun.com/font/font/488/font/font.ttf?e=1548768793&token=VD7rjRLQpyBmrGT1RTx31HKU9_CMCYnZXpSBHHUq:RlOsQ0jz-duHqVn-z3TR62JJdr8=&attname=%E5%AD%97%E9%AD%8224%E5%8F%B7-%E9%95%87%E9%AD%82%E6%89%8B%E4%B9%A6.ttf&v=1548764383")'
          success: function (res) {
              console.log(res.status) //  loaded
          },
          fail: function (res) {
              console.log(res.status) //  error
          },
          complete: function (res) {
              console.log(res.status);
          }
      });
    },

    mounted() {
        if (toLogin()) {
            console.log("has logged!");
            login();
        }
        var _this = this;
        get("/user/correctCount", { openid: this.openid }).then(res => {
            this.correctCount = res.data;
        });
        this.userInfo = wx.getStorageSync("userInfo");
        this.openid = wx.getStorageSync("openid");
    },

    methods: {
        clickLantern() {
            this.showRiddle = !this.showRiddle;
        },
        async getSingleRiddle() {
            var _this = this;
            const data = await get("/riddle/single", {});
            this.riddle = data.data.question;
            this.hint = data.data.hint;
            this.showRiddle = true;
            this.correct = 0;
            this.userAnswer = "";
            // 待删除在确认时再赋值
            this.explanation=data.data.explanation;
            console.log(this.riddle);
        },
        async confirm() {
            var _this = this;
            var userAnswer = this.userAnswer;
            const res = await get("/riddle/verify", { answer: userAnswer });
            if (res != null && res.data == true) {
                this.correct = 1;
                this.correctCount+=1;
            } else {
                this.correct = 2;
            }
            this.userAnswer = "";
            console.log("correct:" + res.data);
        }
    }
};
</script>

<style scoped>
div .box {
    text-align: center;
}

div.box img {
    width: 70%;
    margin-top: 2px;
}

.message {
    color: black;
    padding: 10px;
    text-align: center;
}

.pop-container {
    text-align: center;
    margin: 0 28px;
    min-height: 20%;
    border-style:double;
    border-color: #f03232;
    border-radius: 0.5em;
    border-width: 7px;
    padding:5px;
}

.pop-container > div {
    text-align: center;
}

.pop-container img {
    width: 50%;
}

input {
    font-size: 10pt;
}

.confirm-btn {
    flex: 1;
    margin: 3px;
    text-align: center;
    width: 70%;
    border: none;
    background-color: #f03232;
    color: #eee;
}

div .userinfo-bar {
    position: absolute;
    top: 20%;
    right: 20px;
    font-size: 14px;
}

div .riddle-bar {
    position: absolute;
    top: 20%;
    left:46%;
    right: 46%;
    padding: 6px;
    border:#f03232;
    border-radius: 0.5em;
    background-color: #f03232;
    color:burlywood;
}

div .question{
    color:#f03232;
}

/* 头像 */
.userinfo-avatar {
    width: 64rpx;
    height: 64rpx;
    /* 在App.vue中统一定义 */
    /* border-radius:50%; */
}

/* 字体 */
.web-font{
    font-family:"webfont" !important;
    font-size:16px;font-style:normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
    color:#333;
}

.answer-input{
    border:1px,solid,#f03232;
}

.scroll{
    margin: 5px 0 0 0;
}

</style>
