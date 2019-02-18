<template>
    <div class="container" @click="clickHandle">
        <div class="bgm-btn">
            <img class="note-pic" src="http://thyrsi.com/t6/672/1550506004x2890174315.png" @click=revertBgm()>
        </div>
        <div class="author-wrapper">
            <div>
                <img class="userinfo-avatar" src="/static/images/unlog_avatar.jpg">
                <button
                    class="author-btn"
                    open-type="getUserInfo"
                    @getuserinfo="bindGetUserInfo"
                    @click="getUserInfo"
                >授权</button>
            </div>
            <!-- <button open-type="getUserInfo" @getuserinfo="getUserInfo">登录</button> -->
        </div>
    </div>
</template>
<script>
import { get, getStorageOpenid } from "../../utils/wx-request";
export default {
    data() {
        return {
            openid: ""
        };
    },
    methods: {
        bindGetUserInfo(e) {
            if (e.mp.detail.rawData) {
                //用户按了允许授权按钮
                console.log("用户按了允许授权按钮");
                const userInfo = e.mp.detail.userInfo;
                console.log("111", userInfo);
                wx.setStorageSync("userInfo", userInfo);
                const openid = wx.getStorageSync("openid");
                // 使用navigateTo只能调转到非tabBar的页面
                wx.navigateTo({
                    url: "/pages/index/main"
                });
                // 如果在tabBar中要注册跳转的页面，可使用wx.switchTab,wx.navigateBack()（注意区别）
                // wx.navigateBack({
                    
                // });
                console.log("after bind userInfo");
            } else {
                //用户按了拒绝按钮
                console.log("用户按了拒绝按钮");
            }
        },
        getUserInfo() {
            console.log("click事件首先触发");
            // 判断小程序的API，回调，参数，组件等是否在当前版本可用。  为false 提醒用户升级微信版本
            // console.log(wx.canIUse('button.open-type.getUserInfo'))
            if (wx.canIUse("button.open-type.getUserInfo")) {
                // 用户版本可用
            } else {
                console.log("请升级微信版本");
            }
            wx.login({
                success: function(res) {
                    if (res.code) {
                        //发起网络请求
                        console.log("code:" + res.code);
                        // 原生框架写法
                        // wx.request({
                        //     url: "https://localhost:9007/spring/user/getOpenId",
                        //     data: {
                        //         code: res.code
                        //     }
                        // });
                        get("/user/getOpenId", { code: res.code }).then(res => {
                            wx.setStorageSync("openid", res.data);
                            console.log("wxResp:", res.data);
                        });
                    } else {
                        console.log("获取用户登录态失败！" + res.errMsg);
                    }
                }
            });
        }
    }
};
</script>
<style>
/* 头像 */
.userinfo-avatar {
    width: 256rpx;
    height: 256rpx;
    margin: 20rpx;
    /* 在App.vue中统一定义 */
    /* border-radius:50%; */
}

.author-btn {
    position: relative;
    display: block;
    width: 50%;
    margin-left: auto;
    margin-right: auto;
    padding-left: 14px;
    padding-right: 14px;
    box-sizing: border-box;
    font-size: 18px;
    text-align: center;
    text-decoration: none;
    line-height: 2.55555556;
    border-radius: 5px;
    -webkit-tap-highlight-color: transparent;
    overflow: hidden;
    color: #fff;
    background-color: #e42424;
}
.author-wrapper {
    width: 100%;
    height: 80%;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>