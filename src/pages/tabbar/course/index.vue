<!--
 * @Author: 朽木白
 * @Date: 2022-08-22 17:06:51
 * @LastEditors: 1547702880@@qq.com
 * @LastEditTime: 2023-02-20 17:25:59
 * @Description: 课程列表
-->
<template>
  <view class="container course">
    <view class="course_list">
      <view v-for="item in list" :key="item.id" class="course_list_item">
        <navigator class="course_list_item_a" :url="'/pages/course/detail/index?id=' + item.id">
          <view class="item_cover">
            <image :src="item.cover" />
          </view>
          <view class="item_content">
            <h3 class="content_title">{{ item.title }}</h3>
            <view class="content_price">
              <view class="price">¥{{ item.price }}</view>
              <view class="buy_num">{{ item.buyCount }}人已购买</view>
            </view>
          </view>
        </navigator>
      </view>
      <!-- 加载更多 -->
      <view class="load_more">
        <uni-load-more :status="status" />
      </view>
    </view>

    <v-back-top></v-back-top>
  </view>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import { onLoad, onReachBottom, onPageScroll } from '@dcloudio/uni-app'
import courseService from '@/api/course'
import UniLoadMore from '@dcloudio/uni-ui/lib/uni-load-more/uni-load-more.vue'
import VBackTop from '@/components/v-back-top/v-back-top.vue'
const list = ref([]) as any
const params = reactive({
  page: 1,
  limit: 10,
})
const status = ref('more')

onLoad((options) => {
  getCourseList()
})

onReachBottom(() => {
  if (status.value !== 'noMore') {
    status.value = 'loading'
    params.page++
    getCourseList()
  }
})
onPageScroll((res) => {
  uni.$emit('onPageScroll', res)
})

async function getCourseList() {
  try {
    const res: any = await courseService.courseList({
      ...params,
    })
    const items = res.data.items
    // 数组解构拼接
    list.value = [...list.value, ...items]
    if (items.length < 10) return (status.value = 'noMore')
    if (items.length >= 10) status.value = 'more'
  } catch (e) {
    console.log('e', e)
  }
}
</script>

<style scoped lang="scss">
@import '@/static/styles/_global.scss';
@import '@/static/styles/_uni-defult.scss';
.course {
  min-height: 100vh;
}
.header {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 999;
  background: #1f2228;
}

.course_list {
  background: #000;
}
.course_list_item {
  padding: 15px 15px;
  background: #1f2228;
  margin-bottom: 12px;
  &_a {
    display: flex;
    .item_cover {
      width: 113px;
      height: 64px;
      image {
        width: 100%;
        height: 100%;
      }
    }
    .item_content {
      flex: 1;
      padding-left: 12px;
      padding-top: 8px;

      .content_title {
        font-size: 12px;
        color: #a1a7b2;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        word-break: break-all;
        height: 34px;
      }
      .content_price {
        display: flex;
        align-items: center;
        .price {
          color: #e53935;
          font-size: 16px;
        }
        .buy_num {
          width: 100%;
          font-size: 12px;
          color: #a1a7b2;
          margin-left: 12px;
        }
      }
    }
  }
}
</style>
