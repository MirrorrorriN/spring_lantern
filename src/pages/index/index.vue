<template>
    <div class="container">
        <!-- <div class="message">{{msg}}</div> -->
        <div class="box">
            <img src="/static/images/lantern.png" mode="widthFix" @click="getSingleRiddle">
        </div>
        <!-- <div class="pop-container" v-if="showRiddle">
        {{riddle}}
        </div>-->
        <div class="userinfo-bar">
            <div>
                <img class="userinfo-avatar" :src="userInfo.avatarUrl">
                <!-- <img class="userinfo-avatar" src="/static/images/unlog_avatar.jpg" v-else> -->
                <span>{{userInfo.nickName}}</span>
            </div>
            <div>已答{{correctCount}}题</div>
        </div>
        <div class="pop-container">
            <div class="question">
                <!-- <img src="/static/images/lace.jpeg" mode="widthFix"> -->
                <div>{{riddle}}</div>
                <!-- <img src="/static/images/lace.jpeg" mode="widthFix"> -->
                <div>{{hint}}</div>
            </div>
            <div class="answer">
                <!-- <input type="text" confirm-type="search" v-model="answer" placeholder="请输入谜底" @confirm="confirm($event)" v-on:input="watchAnswer"> -->
                <input
                    type="text"
                    confirm-type="done"
                    v-model="userAnswer"
                    placeholder="请输入谜底"
                    @confirm="confirm()"
                >
                <!-- <button class="confirm-btn" @click="handleClick">太简单了！提交~</button> -->
            </div>
        </div>
    </div>
</template>

<script>
import { get, getStorageOpenid } from "../../utils/wx-request";
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
            openid: wx.getStorageSync("openid")
        };
    },

    mounted() {
        if (toLogin()) {
            console.log("has logged!");
            login();
        }
        var _this = this;
        get("/user/correctCount", { openid: this.openid }).then(res => {
            console.log("correctCount" + res.data);
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
            console.log(this.riddle);
        },
        confirm() {
            var _this = this;
            var userAnswer = this.userAnswer;
            const data = post("/riddle/verify", { answer: userAnswer });
            console.log(this.userAnswer);
        }
    }
};
</script>

<style scoped>
div .box {
    text-align: center;
}

div.box img {
    width: 77%;
    margin-top: 30px;
}

.message {
    color: black;
    padding: 10px;
    text-align: center;
}

.pop-container {
    text-align: center;
    padding: 0;
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
    background-color: red;
    color: #eee;
}

div .userinfo-bar {
    position: absolute;
    top: 20%;
    right: 20px;
    font-size: 14px;
}

/* 头像 */
.userinfo-avatar {
    width: 32rpx;
    height: 32rpx;
    /* 在App.vue中统一定义 */
    /* border-radius:50%; */
}
</style>
