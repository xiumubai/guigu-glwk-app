import { defineComponent } from 'vue';
<template>
  <view class="preferences_list">
    <view v-for="(item, index) in list" :key="index" class="preferences_list_item">
      <navigator
        class="list_item_card"
        :url="
          type === 'course' ? `/pages/course/detail/index?id=${item.id}` : `/pages/teacher/detail/index?id=${item.id}`
        "
      >
        <view class="list_item_card_img" :style="{ height: type === 'course' ? '95px' : 'auto' }">
          <image
            alt="封面"
            class="card-img"
            :mode="type === 'course' ? 'fill' : 'widthFix'"
            :src="type === 'course' ? item.cover : item.avatar"
          />
        </view>
        <view class="list_item_card_content">
          <h3 class="item_content_name">
            {{ type === 'course' ? item.title : item.intro }}
          </h3>
          <view class="item_content__labal">
            <uni-icons v-if="type === 'course'" type="fire" size="18" color="#fa3f4e"></uni-icons>
            <view :class="['study_num', type === 'teacher' ? 'teacher_name' : '']">
              <text v-if="type === 'course'">{{ item.lessonNum }}人已学习</text>
              <text v-else>{{ item.name }}</text>
            </view>
          </view>
          <view v-if="type === 'course'" class="item-content_footer">
            <view class="footer_price">¥{{ item.price }}</view>
            <view class="footer_buy_num">{{ item.buyCount }}人购买</view>
          </view>
        </view>
      </navigator>
    </view>
  </view>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'VCardList',
  props: {
    list: {
      type: Array,
      default: () => {
        return []
      },
    },
    type: {
      type: String,
      default: 'course',
    },
  },
  setup() {},
})
</script>

<style lang="scss" scoped>
.preferences_list {
  .card-img {
    height: 100%;
  }
  &_item {
    padding-bottom: 8px;
    display: inline-block;
    width: 50%;
    box-sizing: border-box;
    &:nth-child(2n-1) {
      padding-right: 4px;
    }
    &:nth-child(2n) {
      padding-left: 4px;
    }
    .list_item_card {
      display: block;
      width: 100%;
      &_img {
        width: 100%;
        height: 95px;
        background-image: url('https://8.idqqimg.com/edu/mobilev2/m-core/3d1dd248376a6da4a15e0000184f44c6.png');
        background-repeat: no-repeat;
        background-size: contain;
        border-radius: 8px 8px 0 0;
        image {
          height: 100%;
          width: 100%;
          border-radius: 8px 8px 0 0;
        }
      }
      &_content {
        background: #1f2228;
        border-radius: 0 0 8px 8px;
        .item_content_name {
          height: 40px;
          padding: 4px 4px;
          color: #a1a7b2;
          font-size: 14px;
          white-space: normal;
          overflow: hidden;
          text-overflow: ellipsis;
          display: -webkit-box;
          -webkit-line-clamp: 2;
          -webkit-box-orient: vertical;
        }
        .item_content__labal {
          display: flex;
          align-items: center;
          padding: 0 4px;
          .study_num {
            font-size: 12px;
            color: #a1a7b2;
            margin-left: 4px;
          }
          .teacher_name {
            color: #68cb9b;
            margin-left: 0;
            font-size: 14px;
            margin-bottom: 4px;
          }
        }
        .item-content_footer {
          display: flex;
          align-items: center;
          padding: 4px;
          .footer_price {
            font-size: 16px;
            color: #fa3f4e;
            margin-right: 4px;
          }
          .footer_buy_num {
            font-size: 12px;
            color: #a1a7b2;
          }
        }
      }
    }
  }
}
</style>
