<template>
  <view class="album-detail">
    <!-- 大图 开始 -->
    <view class="large-pic">
      <image :src="albumObj.lcover" mode="widthFix" />
      <view class="name">{{ albumObj.name }}</view>
    </view>
    <!-- 大图 结束 -->

    <!-- 用户信息 开始 -->
    <view class="user-info">
      <view class="avatar">
        <image :src="albumObj.user.avatar" mode="" />
      </view>
      <view class="user-name">{{ albumObj.user.name }}</view>
    </view>
    <!-- 用户信息 结束 -->

    <!-- 描述 开始 -->
    <view class="desc-area">
      {{ albumObj.desc }}
    </view>

    <!-- 描述 结束 -->
  </view>
</template>

<script>
export default {
  components: {},
  data: () => ({
    id: "",
    params: {
      limit: 30,
      skip: 0,
      order: "new",
      first: 1,
    },
    albumObj: {},
  }),
  computed: {},
  methods: {
    // 请求数据
    getList() {
      this.request({
        url: `http://service.picasso.adesk.com/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params,
      }).then((result) => {
        console.log(result);
        this.albumObj = result.res.album;
      });
    },
  },

  // 页面周期函数--监听页面加载
  onLoad(options) {
    this.id = options.id;
    this.getList();
  },
};
</script>

<style lang="scss" scoped>
.album-detail {
  .large-pic {
    position: relative;
    .name {
      position: absolute;
      bottom: 0;
      color: #ddd;
      font-size: 32rpx;
      padding: 15rpx;
    }
  }

  .user-info {
    padding: 25rpx;
    display: flex;
    .avatar {
      image {
        width: 90rpx;
        height: 90rpx;
        border-radius: 50%;
      }
    }

    .user-name {
      margin: auto 20rpx;
    }
  }

  .desc-area {
    padding: 5rpx 30rpx;
    line-height: 1.7;
    font-size: 30rpx;
  }
}
</style>
