<template>
  <view class="category-detail">
    <!-- 分段器 开始 -->
    <view class="tab">
      <uni-segmented-control
        :current="current"
        :values="items.map((v) => v.title)"
        @clickItem="onClickItem"
        styleType="text"
        activeColor="#d93a88"
      ></uni-segmented-control>
    </view>

    <!-- 分段器 结束 -->

    <!-- 图片内容 开始 -->
    <scroll-view class="content" @scrolltolower="handleToLower" scroll-y>
      <!-- 最新 -->
      <view v-if="current === 0">
        <view class="pics-item" v-for="(item, index) in picList" :key="item.id">
          <go-detail :list="picList" :index="index">
            <image :src="item.thumb" mode="aspectFill" />
          </go-detail>
        </view>
      </view>
      <!-- 热门 -->
      <view v-if="current === 1">
        <view class="pics-item" v-for="(item, index) in picList" :key="item.id">
          <go-detail :list="picList" :index="index">
            <image :src="item.thumb" mode="aspectFill" />
          </go-detail>
        </view>
      </view>
    </scroll-view>
    <!-- 图片内容 开始 -->
  </view>
</template>

<script>
// 引入扩展组件 分段器
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import goDetail from "../../components/goDetail/goDetail.vue";
export default {
  components: { uniSegmentedControl, goDetail },
  data: () => ({
    id: "",
    params: {
      limit: 30,
      skip: 0,
      order: "new",
    },
    items: [
      { title: "最新", order: "new" },
      { title: "热门", order: "hot" },
    ],
    // 当前 items
    current: 0,
    picList: [],
  }),
  computed: {},
  methods: {
    // 请求数据
    getList() {
      this.request({
        url: `http://service.picasso.adesk.com/v1/vertical/category/${this.id}/vertical`,
        data: this.params,
      }).then((result) => {
        console.log(result);
        // 图片列表
        this.picList = [...this.picList, ...result.res.vertical];
      });
    },
    // 点击tab栏切换函数
    tabChange(index) {
      this.params.order = this.items[index].order;
      this.params.skip = 0;
      this.picList = [];
      this.getList();
    },
    // 分段器点击事件
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
      // 点击最新
      if (e.currentIndex === 0) {
        this.tabChange(0);
      }
      // 点击热门
      if (e.currentIndex === 1) {
        this.tabChange(1);
      }
    },
    // 处理滚动上拉触底事件
    handleToLower() {
      this.params.skip += this.params.limit;
      this.getList();
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
.category-detail {
  .tab {
  }

  .content {
    height: 100vh;
    .pics-item {
      width: 33.33%;
      float: left;
      border: 3rpx solid #fff;
      image {
        border-radius: 20rpx;
      }
    }
  }
}
</style>
