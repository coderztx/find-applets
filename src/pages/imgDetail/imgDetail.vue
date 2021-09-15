<template>
  <div class="img-detail">
    <view class="pic">
      <swiper-action @swiperAction="handleSwiper">
        <image
          :src="imgDetail.img"
          mode="aspectFill"
        />
      </swiper-action>
    </view>
    <view class="download">
      <button type="primary" @click="handleDownload">下载图片</button>
    </view>
  </div>
</template>

<script>
import swiperAction from "../../components/swiperAction/swiperAction.vue";
export default {
  components: { swiperAction },
  data: () => ({
    imgDetail: {},
    // 临时下载链接数据
    res: [],
    // 图片列表数据
    list: [],
    // 图片索引值
    index: "",
  }),
  methods: {
    // 下载图片
    async handleDownload() {
      const res = await uni.downloadFile({ url: this.imgDetail.img });
      const { tempFilePath } = res[1];
      await uni.saveImageToPhotosAlbum({
        filePath: tempFilePath,
      });
      uni.showToast({
        title: "下载完成",
      });
    },
    // 滑动事件
    handleSwiper(e) {
      if (e.direction === "left" && this.index < this.list.length - 1) {
        this.index++;
      } else if (e.direction === "right" && this.index > 0) {
        this.index--;
      } else {
        uni.showToast({
          title: '没有更多了~~',
          icon:'none'
        });
      }
      this.imgDetail = this.list[this.index];
    },
  },

  // 页面周期函数--监听页面加载
  onLoad() {
    console.log(getApp().globalData);
    const { list, index } = getApp().globalData;
    this.list = list;
    this.index = index;
    this.imgDetail = list[index];
  },
};
</script>

<style lang="scss" scoped>
.download button {
  border-radius: 0;
}
.pic{
	image{
		width: 100vw;
		height: 100vh;
	}
}
</style>
