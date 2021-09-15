<template>
  <view class="home" v-if="recommendList.length">
    <!-- 月份开始 -->
    <view class="months-wrap">
      <view class="months-title">
        <text>{{ monthsObj.DD }}/</text>
        {{ monthsObj.MM }}月
        <view class="text">{{ monthsObj.title }}</view>
      </view>
      <view class="pics">
        <view class="pics-item" v-for="(item,index) in monthsObj.items" :key="item.id">
          <go-detail :list="monthsObj.items" :index="index">
            <image
              :src="item.thumb + item.rule.replace('$<Height>', 360)"
              mode="aspectFill"
            />
          </go-detail>
        </view>
      </view>
    </view>
    <!-- 月份结束-->

    <!-- 热门开始 -->
    <view class="hot-wrap">
      <view class="title">
        <text>热门</text>
      </view>
      <view class="pics">
        <view
          class="pics-item"
          v-for="(item, index) in recommendList"
          :key="item.id"
        >
          <go-detail :list="recommendList" :index="index">
            <image :src="item.thumb" mode="aspectFill" />
          </go-detail>
        </view>
      </view>
    </view>
    <!-- 热门结束 -->
  </view>
</template>

<script>
import goDetail from "../../components/goDetail/goDetail.vue";
import moment from "moment";
export default {
  components: { goDetail },
  data: () => ({
    // 发起请求携带的参数
    params: {
      // 热门对象的个数
      limit: 30,
      // 跳过对象的个数
      skip: 0,
      order: "hot",
    },
    // 月份模块对象
    monthsObj: {},
    // 推荐模块列表
    recommendList: [],
    // 判断是否还有更多可加载的图片
    hasMore: true,
  }),
  methods: {
    // 请求数据
    getList() {
      this.request({
        url: "http://service.picasso.adesk.com/v3/homepage/vertical",
        data: this.params,
      }).then((result) => {
        console.log(result);
        // 如果获取不到新图片对象,则执行
        if (result.res.vertical.length === 0) {
          this.hasMore = false;
          return;
        }
        // 第一次请求时才对下面变量赋值
        if (this.recommendList.length === 0) {
          // 赋值月份对象
          this.monthsObj = result.res.homepage[2];
          // 使用 moment 库格式化时间戳
          this.monthsObj.MM = moment(this.monthsObj.stime).format("MM");
          this.monthsObj.DD = moment(this.monthsObj.stime).format("DD");
        }
        // 赋值推荐列表
        this.recommendList = [...this.recommendList, ...result.res.vertical];
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
// 月份 开始
.home {
  .months-wrap {
    .months-title {
      color: $color;
      font-weight: 600;
      display: flex;
      padding: 15rpx;
      text {
        font-size: 32rpx;
      }

      .text {
        color: #666;
        padding-left: 20rpx;
        font-size: 30rpx;
      }
    }

    .pics {
      display: flex;
      flex-wrap: wrap;
      .pics-item {
        width: 33.33%;
        border: 2rpx solid #fff;
        image {
          border-radius: 20rpx;
        }
      }
    }
  }
}
// 月份结束

// 热门开始
.hot-wrap {
  .title {
    padding: 15rpx;
    text {
      font-weight: 600;
      border-left: 10rpx solid $color;
      padding-left: 15rpx;
      color: #666;
      font-size: 30rpx;
    }
  }

  .pics {
    display: flex;
    flex-wrap: wrap;
    .pics-item {
      border: 3rpx solid #fff;
      width: 33.33%;
      image {
        border-radius: 20rpx;
      }
    }
  }
}
// 热门结束
</style>
