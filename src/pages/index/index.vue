<template>
    <div class="container">
        <!-- <div class="message">{{msg}}</div> -->
        <div class="box">
            <img src="/static/images/lantern.png" mode="widthFix" @click="getSingleRiddle">
        </div>
        <!-- <div class="pop-container" v-if="showRiddle">
        {{riddle}}
        </div>-->
        <div class="pop-container">
            <div class="question">
                <img src="/static/images/lace.jpeg" mode="widthFix">
                <div>{{riddle}}</div>
                <img src="/static/images/lace.jpeg" mode="widthFix">
            </div>
            <div class="answer">
                <!-- <input type="text" confirm-type="search" v-model="answer" placeholder="请输入谜底" @confirm="confirm($event)" v-on:input="watchAnswer"> -->
                <input
                    type="text"
                    confirm-type="done"
                    v-model="answer"
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
            showRiddle: false,
            answer: ""
        };
    },

    mounted() {
        if (toLogin()) {
            console.log("has logged!");
            this.userInfo = login();
            console.log(this.userInfo);
        }
    },

    methods: {
        clickLantern() {
            this.showRiddle = !this.showRiddle;
        },
        async getSingleRiddle() {
            var _this = this;
            const data = await get("/riddle/single", {});
            this.riddle = data;
            this.showRiddle = true;
            console.log(this.riddle);
        },
        confirm() {
            var _this = this;
            var answer = this.answer;
            const data = post("/riddle/verify", { answer: answer });
            console.log(this.answer);
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

/* 头像 */
.userinfo-avatar {
    width: 128rpx;
    height: 128rpx;
    margin: 20rpx;
    /* 在App.vue中统一定义 */
    /* border-radius:50%; */
}
</style>
