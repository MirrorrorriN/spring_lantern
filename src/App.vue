<script>
/* 这部分相当于原生小程序的 app.js */

export default {
    created() {
        console.log("miniapp created!!!");
    },
    mounted() {
        const backgroundAudioManager = wx.getBackgroundAudioManager();
        backgroundAudioManager.title = "步步高";
        backgroundAudioManager.epname = "步步高";
        backgroundAudioManager.singer = "中央民族乐团";
        backgroundAudioManager.coverImgUrl =
            // "http://y.gtimg.cn/music/photo_new/T002R300x300M000003rsKF44GyaSk.jpg?max_age=2592000";
            "http://y.gtimg.cn/music/photo_new/T002R800x800M000004DC0NR23DKVD.jpg?max_age=2592000";
        // 设置了 src 之后会自动播放
        backgroundAudioManager.src =
            // 可仿照官方链接拼接
            // "http://ws.stream.qqmusic.qq.com/M500001VfvsJ21xFqb.mp3?guid=ffffffff82def4af4b12b3cd9337d5e7&uin=346897220&vkey=6292F51E1E384E061FF02C31F716658E5C81F5594D561F2E88B854E81CAAB7806D5E4F103E55D33C16F3FAC506D1AB172DE8600B37E43FAD&fromtag=46"; 
            "http://isure.stream.qqmusic.qq.com/C400001rjId84BMaEK.m4a?guid=9409697232&vkey=B189EC590816937E068AB49DEDF532FC2249330809C3E2082D6E32DBC656EBF747EB8C2958A0F865005BACD33470B2C58056974FC79700A6&uin=0&fromtag=66";
    }
};
</script>

<style>
page {
    height: 100%;
}

/* 这部分相当于原生小程序的 app.wxss */
.userinfo-avatar {
    border-radius: 50%;
}

.container {
    height: 100%;
    font-size: 14px;
    font-family: "webfont" !important;
    font-size: 16px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
    color: #333;
    background-repeat: no-repeat;
    background-size: 100% 100%;
    /* background-size:cover; */
    /* background-image:url(http://thyrsi.com/t6/664/1548750094x2890202498.jpg); */
    /* background-image:url(http://thyrsi.com/t6/664/1548750264x2890202498.jpg); */
    background-image: url(http://thyrsi.com/t6/664/1548759691x2890203049.png);
}
</style>