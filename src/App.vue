<template>
  <div id="app">
    <div @click="getPrizeId" class="btn">开奖</div>
    <Turntable :prizeId="prizeId" :prize="prize" @overed="overed" :option="option" />
  </div>
</template>

<script>
import Turntable from "./components/Turntable.vue";

export default {
  name: "App",
  components: {
    Turntable,
  },
  data() {
    return {
      prizeId: 0,
      prize: [
        {
          background: "red",
          prizeImg:
            "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793819024,2902167935&fm=26&gp=0.jpg",
          prizeName: "苹果手机0",
        },
        {
          background: "#332244",
          prizeImg:
            "https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1906469856,4113625838&fm=26&gp=0.jpg",
          prizeName: "草莓1",
        },
        {
          background: "yellow",
          prizeImg:
            "https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2820341014,3310204024&fm=26&gp=0.jpg",
          prizeName: "电脑2",
        },
        {
          background: "blue",
          prizeImg:
            "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2213777692,4084044944&fm=26&gp=0.jpg",
          prizeName: "笔记本3",
        },
        {
          background: "green",
          prizeImg:
            "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1594615811,447045871&fm=26&gp=0.jpg",
          prizeName: "谢谢惠顾4",
        },
        {
          background: "gray",
          prizeImg:
            "https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=2793819024,2902167935&fm=26&gp=0.jpg",
          prizeName: "苹果手机5",
        },
        {
          background: "yellow",
          prizeImg:
            "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1594615811,447045871&fm=26&gp=0.jpg",
          prizeName: "谢谢惠顾6",
        },
      ],
      option: {
        times: 10, //旋转圈数
        duration: 5, //旋转时间
        imgHeight: 40,
        imgWidth: 40,
        imgOffset: 150, //距离圆心距离
      },
    };
  },
  methods: {
    getPrizeId() {
      this.prizeId = Math.floor(Math.random() * this.prize.length);
    },
    overed() {
      alert("中奖ID" + this.prizeId);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.btn {
  width: 200px;
  background: rgb(86, 172, 252);
  height: 40px;
  line-height: 40px;
  border-radius: 10px;
  color: #fff;
  cursor: pointer;
}
</style>
