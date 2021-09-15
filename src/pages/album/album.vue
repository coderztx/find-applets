<template>
  <view class="album">
    <!-- 轮播图 开始 -->
    <view class="swiper-wrap">
      <swiper class="swiper" indicator-dots autoplay circular interval="3000">
        <swiper-item v-for="(item, index) in swiperPicList" :key="index">
          <image :src="item.src" />
        </swiper-item>
      </swiper>
    </view>
    <!-- 轮播图 结束 -->
    <!-- 专辑列表 开始 -->
    <view class="album-wrap">
      <navigator class="album-item" v-for="item in albumList" :key="item.id" :url="`/pages/albumDetail/albumDetail?id=${item.id}`">
        <image :src="item.cover" mode="aspectFill"/>
        <view class="content">
          <view class="title">{{ item.name }}</view>
          <view class="desc">{{ item.desc }}</view>
        </view>
      </navigator>
    </view>
    <!-- 专辑列表 结束 -->
  </view>
</template>

<script>
export default {
  components: {},
  data: () => ({
    // 轮播图图片列表
    swiperPicList: [
      {
        src: "../../static/pictures/7.jpg",
      },
      {
        src: "../../static/pictures/2.jpg",
      },
      {
        src: "../../static/pictures/3.jpg",
      },
      {
        src: "../../static/pictures/4.jpg",
      },
      {
        src: "../../static/pictures/5.jpg",
      },
      {
        src: "../../static/pictures/6.jpg",
      },
      {
        src: "../../static/pictures/1.jpg",
      },
    ],
    params: {
      limit: 30,
      skip: 0,
      order: "new",
    },
    // 专辑列表
    albumList: [],
    hasMore: true,
  }),
  methods: {
    // 请求数据
    getList() {
      this.request({
        url: "http://service.picasso.adesk.com/v1/wallpaper/album",
        data: this.params,
      }).then((result) => {
        console.log(result);
        if (result.res.album.length === 0) {
          this.hasMore = false;
          return;
        }
        // 赋值专辑列表数组
        this.albumList = [...this.albumList, ...result.res.album];
      });
    },
  },

  // 页面周期函数--监听页面加载
  onLoad() {
    this.getList();
  },

  // 页面处理函数--监听用户上拉触底
  onReachBottom() {
    if (this.hasMore) {
      this.params.skip += this.params.limit;
      this.getList();
    } else {
      uni.showToast({
        title: "没有更多了",
        icon: "none",
      });
    }
  },
};
</script>

<style lang="scss" scoped>
// 轮播图 开始
.swiper-wrap {
  .swiper {
    // 屏幕的宽度为750rpx
    height: 421rpx;
  }
}
// 轮播图 结束

// 专辑列表开始
.album-wrap {
  padding: 15rpx;
  .album-item {
    display: flex;
    border-bottom: 3rpx solid #ccc;
    padding: 15rpx 0;
    image {
      width: 33.33%;
      height: 100px;
      border-radius: 20rpx;
    }

    .content {
      width: 66.66%;
      padding: 0 30rpx;
      .title {
        font-weight: 600;
        font-size: 30rpx;
      }

      .desc {
        font-size: 26rpx;
        margin-top: 15rpx;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }
  }
}
// 专辑列表结束
</style>
