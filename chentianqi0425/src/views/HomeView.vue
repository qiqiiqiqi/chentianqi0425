<template>
  <div class="continer" :style="{ height: getheight }">
    <div class="header">
      <div class="header-log">
        <img src="../assets/img/logo.png" />
      </div>
    </div>
    <div class="content">
      <div class="charecter">
        <div class="charactertag">CHARACTER</div>
      </div>
      <div class="charecter-list-box">
        <div class="charecter-list">
          <div v-for="item in list" :key="item.title" class="list-item">
            <div>
              <img class="list-item-img" :src="item.img" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-show="currentPhonestatus == 'or'" class="or-box">
    <div class="img-scape-box">
      <img class="img-scape" src="../assets/img/scape.png" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {},
  data() {
    return {
      list: [],
      currentPhonestatus: "",
      requrl: "http://localhost:8080/" + "character.json",
      windowWidth: document.documentElement.clientWidth, //实时屏幕宽度
      windowHeight: document.documentElement.clientHeight, //实时屏幕高度
    };
  },
  mounted() {
    var _this = this;
    window.onresize = () => {
      return (() => {
        window.fullHeight = document.documentElement.scrollHeight;
        _this.windowHeight = window.fullHeight; // 高
      })();
    };
    this.initPage();
    // 判断是否是手机端
    if (this._isMobile) {
      window.addEventListener(
        "onorientationchange" in window ? "orientationchange" : "resize",
        function () {
          _this.isPhoneDirection();
        }
      );
      // 判断手机端是横屏还是竖屏
      this.isPhoneDirection();
      console.log(this.list);
    }
  },
  methods: {
    initPage() {
      console.log(this.requrl);
      axios.get(this.requrl).then(
        (res) => {
          console.log(res);
          if (res && res.data && res.data.data) {
            let tmpdata = res.data.data;
            tmpdata.forEach(item =>{
              console.log(item);
              item['img'] = require(`@/assets/img/${item.img}`);
            })
            this.list = tmpdata;
            console.log(this.list);
          }
        },
        (err) => {
          console.log("请求失败！");
        }
      );
    },
    isPhoneDirection() {
      // 竖屏
      if (window.orientation === 180 || window.orientation === 0) {
        this.currentPhonestatus = "or";
      }
      // 横屏
      if (window.orientation === 90 || window.orientation === -90) {
        this.currentPhonestatus = "vw";
      }
    },
    // 判断手机端还是PC端
    _isMobile() {
      let flag = navigator.userAgent.match(
        /(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i
      );
      return flag;
    },
  },
  watch: {
    currentPhonestatus(newvalue) {
      console.log(newvalue);
      this.currentPhonestatus = newvalue;
    },
  },
  computed: {
    getheight: function () {
      return `${this.windowHeight}px`;
    },
  },
};
</script>
<style scoped>
.continer {
  width: 100%;
  height: 100%;
}
.charecter {
  height: 50px;
  width: 100%;
  border-bottom: 0.1px solid #605a5a;
  margin-bottom: 50px;
  position: relative;
}
.continer .header {
  width: 100%;
  min-height: 200px;
  background-image: url("../../src/assets/img/bg.png");
  background-repeat: no-repeat;
  position: relative;
}
.charecter-list-box {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  width: 80%;
  margin: 0 auto;
  height: calc(100% - 200px);
  height: auto;
}
.charecter-list {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: flex-start;
  flex-flow: wrap;
}

.list-item {
  height: 200px;
  width: 200px;
  margin: 0px 20px 40px 20px;
}
.list-item-img {
  height: 200px;
  width: 200px;
}
.header-log {
  position: absolute;
  left: 20%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.content {
  /* background: rgb(70, 69, 69); */
  background: linear-gradient(rgb(37, 37, 39), rgb(37, 37, 43));
  height: auto;
  min-height: calc(100% - 200px);
}
.charactertag {
  position: absolute;
  color: #fff;
  background: rgba(224, 224, 224, 0.1);
  top: 100%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50px 50px 50px 50px / 0px 0px 0px 0px;
  padding: 5px 10px;
}
.or-box {
  position: absolute;
  height: 100%;
  width: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 99;
  top: 0px;
  left: 0px;
}
.or-box .img-scape-box {
  width: 100%;
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 999;
  transform: translate(-50%, -50%);
}
.or-box .img-scape {
  height: 80%;
  width: 80%;
  animation: routephone 1s ease-out 1s infinite normal;
}
@keyframes routephone {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(90deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
</style>
<style></style>
