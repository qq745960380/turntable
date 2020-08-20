<template>
  <div id="turntable" :style="{...getCircleCss}">
    <div v-for="(item,index) in prize" :key="index" class="sector" :style="getSectorCss(index)">
      <div :style="{...getPrizeTextCss}">{{item.prizeName}}</div>
      <img :style="{...getPrizeImgCss}" :src="item.prizeImg" alt />
    </div>
  </div>
</template>

<script>
export default {
  name: "turntable",
  props: {
    prizeId: {
      type: Number,
      require: true,
    },
    prize: {
      type: Array,
      require: true,
    },
    width: {
      type: Number,
      default: 500,
    },
    times: {
      type: Number,
      default: 10,
    },
    option: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      rotateAngle: (this.prizeId * 360) / this.prize.length, //初始旋转角度
    };
  },
  mounted() {
    // 获取大转盘节点，当旋转结束时的回调
    const element = document.getElementById("turntable");
    element.addEventListener("transitionend", this.handleTransitionend, false);
  },
  watch: {
    prizeId: function (newVal) {
      const current = this.rotateAngle % 360; //判断当前角度，先转到第一个奖品后，再开始后面旋转
      const num = this.prize.length; //奖品数量
      const rotateAngle = 360 / num; //每个扇形旋转角度
      const otherAngel = 360 - newVal * rotateAngle; //额外需要旋转的角度
      const { times = 10 } = this.option;
      if (current === 0) {
        this.rotateAngle += times * 360 + otherAngel;
      } else {
        this.rotateAngle += times * 360 + (360 - current) + otherAngel;
      }
    },
  },
  computed: {
    getCircleCss() {
      let circleCss = {};
      const { duration = 5 } = this.option;
      circleCss.width = this.width + "px";
      circleCss.height = this.width + "px";
      circleCss["transform"] = `rotate(${this.rotateAngle}deg)`;
      circleCss["transition-duration"] = duration+"s";
      circleCss["transition-timing-function"] = "ease";
      return circleCss;
    },
    getSectorCss() {
      return function (index) {
        let sectorCss = {};
        const radius = this.width / 2; //圆的半径
        const num = this.prize.length; //奖品数量
        const angle = (2 * Math.PI) / num; //每个扇形的弧角度
        const rotateAngle = 360 / num; //每个扇形旋转角度
        const sectorHeight = Math.tan(angle / 2) * radius; //每个扇形的宽度
        sectorCss["left"] = -(sectorHeight - radius) + "px"; //
        sectorCss["border-top-color"] = this.prize[index].background
          ? this.prize[index].background
          : ""; //每个扇形的背景颜色
        sectorCss["transform"] = `rotate(${rotateAngle * index}deg)`;
        sectorCss["border-width"] = `${radius}px ${
          radius * Math.tan(angle / 2)
        }px `;
        return sectorCss;
      };
    },
    getPrizeImgCss() {
      let { imgHeight, imgWidth, imgOffset } = this.option;
      imgWidth = imgWidth ? imgWidth : this.width / 10; //如果未传值，宽高则设置为大转盘的十分之一
      imgHeight = imgHeight ? imgHeight : this.width / 10; //如果未传值，宽高则设置为大转盘的十分之一
      imgOffset = imgOffset ? -(imgOffset + imgWidth) : (-this.width / 7) * 3;
      const left = imgWidth ? -imgWidth / 2 : -this.width / 20;
      let prizeImgCss = {
        position: "absolute",
        width: imgWidth + "px",
        height: imgHeight + "px",
        top: imgOffset + "px",
        left: left + "px",
      };
      return prizeImgCss;
    },
    getPrizeTextCss() {
      let { textFontSize, textOffset, textWidth, textColor } = this.option;
      const num = this.prize.length; //奖品数量
      const radius = this.width / 2; //圆的半径

      const angle = (2 * Math.PI) / num; //每个扇形的弧角度
      textFontSize = textFontSize ? textFontSize : 16;
      textOffset = textOffset ? textOffset : (this.width / 7) * 2;
      textWidth = textWidth ? textWidth : Math.tan(angle / 2) * radius;
      textColor = textColor ? textColor : "#000";
      let prizeTextCss = {
        position: "absolute",
        "font-size": textFontSize + "px",
        top: -textOffset + "px",
        width: textWidth + "px",
        color: textColor,
        left: -textWidth / 2 + "px",
      };
      return prizeTextCss;
    },
  },
  methods: {
    // 当旋转结束时的回调
    handleTransitionend() {
      this.$emit("overed");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#turntable {
  border-radius: 50%;
  background: white;
  position: relative;
  overflow: hidden;
}
.sector {
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
  border-top-color: gray;
  border-right-color: transparent;
  border-bottom-color: transparent;
  border-left-color: transparent;
}
.btnbtn{
  height:70px;width:70px;z-index:999;
  position: absolute;
  left: 50%;
  top: 50%;
  margin: -35px 0 0 -35px;
}
</style>
