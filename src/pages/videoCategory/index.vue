<template>
  <view class="category_tab">
    <view class="category_tab_title">
      <view class="title_inner">
        <uni-segmented-control :current="current"
                               :values="items.map(v=>v.title)"
                               @clickItem="onClickItem"
                               style-type="text"
                               active-color="#1A73E8">
        </uni-segmented-control>
      </view>
    </view>
    <scroll-view scroll-y
                 enable-flex
                 @scrolltolower="handleTolower"
                 class="category_tab_content">
      <view class="cate_item"
            v-for="(item,index) in vertical"
            :key="item.id">
        <go-detail :list="vertical"
                   :index="index">
          <image :src="item.img"
                 mode="widthFix">
          </image>
        </go-detail>
      </view>
    </scroll-view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import goDetail from "@/components/goDetail";
export default {
  components: {
    uniSegmentedControl,
    goDetail
  },
  data () {
    return {
      items: [
        { title: '最新', order: 'new' },
        { title: '热门', order: 'hot' }
      ],
      current: 0,
      params: {
        limit: 30,
        skip: 0,
        order: 'new'
      },
      id: 0,
      // 页面显示数据数组
      vertical: [],
      // 判断是否还有分页数据
      hasMore: true
    }
  },
  onLoad (options) {
    this.id = options.id
    console.log(this.id)
    this.getList()
  },
  methods: {
    onClickItem (e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      } else {
        // 重复点击相同标题
        return;
      }
      this.params.order = this.items[e.currentIndex].order
      this.params.skip = 0;
      this.vertical = [];
      this.getList()
    },
    async getList () {
      const { res } = await this.request({
        url: `http://157.122.54.189:9088/videoimg/v1/videowp/category/${this.id}`,
        data: this.params
      })
      console.log(res.videowp);
      this.vertical = [...this.vertical, ...res.videowp]
      // if (res.vertical.length === 0) {
      //   this.showToast()
      //   return
      // }
      // 
    },
    handleTolower () {
      // 触底之后发送请求加载分页数据
      if (this.hasMore) {
        this.getList(this.id);
      } else {
        uni.showToast({
          title:'没有更多数据',
          icon:'none'
        });
        return;
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.category_tab {
  .category_tab_title {
    position: relative;
    .title_inner {
      width: 60%;
      margin: 0 auto;
    }
    .iconsearch {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5%;
    }
  }
}
.category_tab_content {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);
  .cate_item {
    width: 33.33%;
    border: 5rpx solid #fff;
    image {
    }
  }
}
</style>