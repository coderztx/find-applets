// 检测滑动事件,对外提供滑动方向的数据
<template>
  <view @touchstart="handleTouchStart" @touchend="handleTouchend">
    <slot></slot>
  </view>
</template>

<script>
export default {
  data: () => ({
    startTime: 0,
    startX: 0,
    startY: 0,
  }),
  methods: {
    handleTouchStart(event) {
      this.startTime = Date.now();
      this.startX = event.changedTouches[0].clientX;
      this.startY = event.changedTouches[0].clientY;
    },
    handleTouchend(event) {
      const endTime = Date.now();
      const endX = event.changedTouches[0].clientX;
      const endY = event.changedTouches[0].clientY;

      // 判断按下的时长
      if (endTime - this.startTime > 2000) {
        return;
      }

      // 滑动方向
      let direction = "";

      // 先判断用户的滑动距离 是否合法  合法则:判断滑动方向
      if (Math.abs(endX - this.startX) > 70 && Math.abs(endY - this.startY) < 50) {
        // 滑动方向
        direction = endX - this.startX > 0 ? "right" : "left";
      } else {
        return;
      }

      // console.log(direction);

      // 发送给父组件
      this.$emit("swiperAction", { direction });
    },
  },
};
</script>

<style lang="scss"></style>
