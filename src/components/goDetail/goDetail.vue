<template>
  <view class="go-detail" @click="handleClick">
    <slot></slot>
  </view>
</template>

<script>
export default {
  props: {
    list: Array,
    index: Number,
  },
  data: () => ({}),
  methods: {
    handleClick() {
      getApp().globalData.list = this.list;
      getApp().globalData.index = this.index;

      uni.navigateTo({
        url: "/pages/imgDetail/imgDetail",
      });
    },
  },
};
</script>

<style lang="scss"></style>
