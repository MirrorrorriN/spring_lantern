<script>
/* 这部分相当于原生小程序的 app.js */
export default {
  created () {
    console.log('miniapp created!!!')
  }
}
</script>

<style>
page {
  height: 100%;
}

/* 这部分相当于原生小程序的 app.wxss */
.userinfo-avatar {
border-radius:50%;
}

.container {
  height: 100%;
  background-repeat:no-repeat;
  background-size: 100% 100%;
  /* background-size:cover; */
  /* background-image:url(http://thyrsi.com/t6/664/1548750094x2890202498.jpg); */
  /* background-image:url(http://thyrsi.com/t6/664/1548750264x2890202498.jpg); */
  background-image: url(http://thyrsi.com/t6/664/1548759691x2890203049.png);
}
</style>