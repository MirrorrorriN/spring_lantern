<template>
    <div class="container">
        <!-- <div class="message">{{msg}}</div> -->
        <div class="bgm-btn">
            <img
                class="note-pic"
                src="https://mirrorrorrin.com/static/img/anagrams/note.png"
                @click="revertBgm()"
            >
        </div>
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
            <!-- <div>
                <span>{{userInfo.nickName}}</span>
            </div>-->
            <div class="user-bar">猜中{{correctCount}}条</div>
            <div class="user-bar" style="margin-top:5px; text-align:right;">{{score}}积分</div>
        </div>
        <div class="riddle-bar">
            <div v-if="correct===0&&!showRiddle" @click="getSingleRiddle">点击抽取</div>
            <div v-else-if="correct===1||correct===3" @click="getSingleRiddle">再抽一条</div>
            <div v-else @click="getSingleRiddle">跳过本条</div>
        </div>
        <div v-if="showRiddle">
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
                        v-if="correct==0"
                        type="text"
                        confirm-type="done"
                        v-model="userAnswer"
                        placeholder="猜猜看"
                        @confirm="confirm()"
                        class="answer-input"
                        style="margin:2px 0;font-size:14px"
                    >
                    <!-- <button class="confirm-btn" @click="handleClick">太简单了！提交~</button> -->
                    <div class="scroll" v-else-if="correct==1">
                        <div>答对了!</div>
                        <div>{{explanation}}</div>
                    </div>
                    <div class="scroll" v-else-if="correct==2">不对哦~</div>
                    <div v-if="correct==2" @click="reDisplayRiddle" style="padding:5px;">
                        <span class="refresh">再猜一次</span>
                    </div>
                    <div v-if="correct==0||correct==2" style="margin-top:5px;text-align:right;"> 
                        <span class="query-answer" @click="queryRiddleAnswer">查看答案（10积分）</span>
                    </div>
                    <div v-if="correct==3">
                        <div>谜底：{{answer}}</div>
                        <div>{{explanation}}</div>
                    </div>
                </div>
            </div>
            <div>
                <span><button class="share-btn" open-type="share">分享给父老乡亲^_^</button></span>
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
            riddleId: 0,
            hint: "",
            showRiddle: false,
            answer: "",
            userAnswer: "",
            userInfo: wx.getStorageSync("userInfo"),
            correctCount: 0,
            openid: wx.getStorageSync("openid"),
            correct: 0,
            explanation: "",
            score: 0,
            bgmState: true
        };
    },

    onload() {
        wx.loadFontFace({
            family: "webfont",
            source: 'url("//at.alicdn.com/t/webfont_mjy7t30ao5.eot")',
            //   source: 'url("//down.izihun.com/font/font/488/font/font.ttf?e=1548768793&token=VD7rjRLQpyBmrGT1RTx31HKU9_CMCYnZXpSBHHUq:RlOsQ0jz-duHqVn-z3TR62JJdr8=&attname=%E5%AD%97%E9%AD%8224%E5%8F%B7-%E9%95%87%E9%AD%82%E6%89%8B%E4%B9%A6.ttf&v=1548764383")'
            success: function(res) {
                console.log(res.status); //  loaded
            },
            fail: function(res) {
                console.log(res.status); //  error
            },
            complete: function(res) {
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
            this.correctCount = res.data.count;
            this.score = res.data.score;
        });
        this.userInfo = wx.getStorageSync("userInfo");
        this.openid = wx.getStorageSync("openid");

        const backgroundAudioManager = wx.getBackgroundAudioManager();
        backgroundAudioManager.title = "平凡天使";
        // backgroundAudioManager.epname = "步步高";
        backgroundAudioManager.singer = "邓紫棋";
        // backgroundAudioManager.coverImgUrl =
            // "http://y.gtimg.cn/music/photo_new/T002R300x300M000003rsKF44GyaSk.jpg?max_age=2592000";
            // "http://y.gtimg.cn/music/photo_new/T002R800x800M000004DC0NR23DKVD.jpg?max_age=2592000";
        // 设置了 src 之后会自动播放
        backgroundAudioManager.src =
            // 可仿照官方链接拼接
            "https://mirrorrorrin.com/static/audi/平凡天使.mp3"
            // "http://ws.stream.qqmusic.qq.com/M500001VfvsJ21xFqb.mp3?guid=ffffffff82def4af4b12b3cd9337d5e7&uin=346897220&vkey=6292F51E1E384E061FF02C31F716658E5C81F5594D561F2E88B854E81CAAB7806D5E4F103E55D33C16F3FAC506D1AB172DE8600B37E43FAD&fromtag=46";
            // "http://isure.stream.qqmusic.qq.com/C400001rjId84BMaEK.m4a?guid=9409697232&vkey=B189EC590816937E068AB49DEDF532FC2249330809C3E2082D6E32DBC656EBF747EB8C2958A0F865005BACD33470B2C58056974FC79700A6&uin=0&fromtag=66";
        // "http://ws.stream.qqmusic.qq.com/C400001rjId84BMaEK.m4a?guid=9409697232&vkey=B189EC590816937E068AB49DEDF532FC2249330809C3E2082D6E32DBC656EBF747EB8C2958A0F865005BACD33470B2C58056974FC79700A6&uin=0&fromtag=66";
    },

    methods: {
        clickLantern() {
            this.showRiddle = !this.showRiddle;
        },
        async getSingleRiddle() {
            var _this = this;
            const openid = wx.getStorageSync("openid");
            const data = await get("/riddle/single", { openid: openid });
            this.riddle = data.data.question;
            this.hint = data.data.hint;
            this.riddleId = data.data.id;
            this.answer = data.data.answer;
            this.showRiddle = true;
            this.correct = 0;
            this.userAnswer = "";
            // 待删除在确认时再赋值
            this.explanation = data.data.explanation;
            console.log(this.riddle);
        },
        async confirm() {
            var _this = this;
            var userAnswer = this.userAnswer;
            var riddleId = this.riddleId;
            const openid = wx.getStorageSync("openid");
            const res = await get("/riddle/verify", {
                answer: userAnswer,
                riddleId: riddleId,
                openid: openid
            });
            if (res != null && res.data == true) {
                this.correct = 1;
                this.correctCount += 1;
                this.score += 5;
            } else {
                this.correct = 2;
            }
            this.userAnswer = "";
            console.log("correct:" + res.data);
        },
        reDisplayRiddle() {
            this.correct = 0;
            this.userAnswer = "";
        },
        queryRiddleAnswer() {
            if (this.score < 10) {
                wx.showToast({
                    title: "积分不够用啦 \r\n 继续加油哦",
                    icon: "none",
                    duration: 2000 //持续的时间
                });
            } else {
                var riddleId = this.riddleId;
                var openid = this.openid;
                get("/riddle/queryAnswer", {
                    riddleId: riddleId,
                    openid: openid
                }).then(res => {
                    if (res.errno === 0) {
                        this.score -= 10;
                        this.correct = 3;
                    } else {
                        var errmsg = res.errmsg;
                        wx.showToast({
                            title: errmsg,
                            icon: "none",
                            duration: 2000 //持续的时间
                        });
                    }
                });
            }
        },
        revertBgm() {
            const backgroundAudioManager = wx.getBackgroundAudioManager();
            if (this.bgmState) {
                backgroundAudioManager.pause();
            } else {
                backgroundAudioManager.play();
            }
            this.bgmState = !this.bgmState;
        }
    }
};
</script>

<style scoped>
div .box {
    text-align: center;
}

div.box img {
    height:360px;
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
    border-style: double;
    border-color: #f03232;
    border-radius: 0.5em;
    border-width: 7px;
    padding: 5px;
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

.user-bar {
    color: #eee;
    background-color: #f03232;
    border-radius: 0.1em;
    font-weight: lighter;
    margin-right: 5px;
    padding: 2px 5px;
}

div .bgm-btn {
    position: absolute;
    top: 5%;
    right: 30px;
    font-size: 14px;
}

.note-pic {
    width: 64rpx;
    height: 64rpx;
    border: 1;
    border-color: #f03232;
}

div .userinfo-bar {
    position: absolute;
    top: 20%;
    right: 20px;
    font-size: 14px;
}

div .riddle-bar {
    position: absolute;
    top: 130px;
    left: 46%;
    right: 46%;
    padding: 6px;
    border: #f03232;
    border-radius: 0.5em;
    background-color: #f03232;
    color: burlywood;
}

div .question {
    color: #f03232;
}

/* 头像 */
.userinfo-avatar {
    width: 64rpx;
    height: 64rpx;
    /* 在App.vue中统一定义 */
    /* border-radius:50%; */
}

/* 字体 */
.web-font {
    font-family: "webfont" !important;
    font-size: 16px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
    color: #333;
}

.answer-input {
    border: 1px, solid, #f03232;
}

.scroll {
    margin: 5px 0 0 0;
}

.refresh {
    color: #eee;
    background-color: #f03232;
    border-radius: 0.5em;
    font-weight: lighter;
    margin-right: 5px;
    padding: 2px 5px;
    margin: 5px;
}

.query-answer {
    color: #eee;
    background-color: #f03232;
    border-width: 1px;
    border-radius: 0.5em;
    /* font-weight: lighter; */
    margin-right: 5px;
    padding: 2px 5px;
    margin: 5px;
    margin-top: 5px;
}

.share-btn {
    position: relative;
    width:60%;
    margin-top: 10px;
    margin-left: auto;
    margin-right: auto;
    padding-left: 14px;
    padding-right: 14px;
    box-sizing: border-box;
    border: none;
    font-size: 16px;
    text-align: center;
    text-decoration: none;
    overflow: hidden;
    color:burlywood;
    background-color: #f03232;
}
</style>
