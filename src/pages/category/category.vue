<template>
  <view class="category">
    <navigator
      class="category-item"
      v-for="item in categoryList"
      :key="item.id"
      :url="`/pages/categoryDetail/categoryDetail?id=${item.id}`"
    >
      <view class="mask">
        <view class="text">{{ item.name }}</view>
      </view>
      <image :src="item.cover" mode="aspectFill" />
    </navigator>
  </view>
</template>

<script>
export default {
  components: {},
  data: () => ({ categoryList: [] }),
  methods: {
    // 请求数据
    getList() {
      this.request({
        url: "http://service.picasso.adesk.com/v1/vertical/category",
      }).then((result) => {
        console.log(result);
        this.categoryList = result.res.category;
      });
    },
  },

  // 页面周期函数--监听页面加载
  onLoad() {
    this.getList();
  },
  // 页面周期函数--监听页面初次渲染完成
};
</script>

<style lang="scss" scoped>
.category {
  display: flex;
  flex-wrap: wrap;
  .category-item {
    width: 33.33%;
    border: 3rpx solid #fff;
    height: 330rpx;
    overflow: hidden;
    position: relative;
    border-radius: 20rpx;

    .mask {
      width: 100%;
      background: rgba(0, 0, 0, 0.3);
      position: absolute;
      bottom: 0;
      height: 50rpx;
      line-height: 50rpx;
      z-index: 1;
      .text {
        margin-left: 20rpx;
        color: #eee;
        font-size: 26rpx;
      }
    }
  }
}
</style>
