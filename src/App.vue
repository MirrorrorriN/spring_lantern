<script>
/* 这部分相当于原生小程序的 app.js */

export default {
    created() {
        console.log("miniapp created!!!");
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
    background-image: url(https://mirrorrorrin.com/static/img/anagrams/lantern_riddle_bg.jpeg);
}
</style>