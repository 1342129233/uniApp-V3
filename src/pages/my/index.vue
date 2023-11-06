<script setup lang="ts">
import { useGuessList } from '@/composables'
import { useMemberStore } from '@/stores'
// 获取屏幕边界到安全区域距离
const { safeAreaInsets } = uni.getSystemInfoSync()

// 获取会员信息
const memberStore = useMemberStore()

const { guessRef, onScrolltolower } = useGuessList()

// 订单选项
const orderTypes = [
    { type: 1, text: '待付款', icon: 'icon-currency' },
    { type: 2, text: '待发货', icon: 'icon-gift' },
    { type: 3, text: '待收货', icon: 'icon-check' },
    { type: 4, text: '待评价', icon: 'icon-comment' },
]
</script>

<template>
    <scroll-view enable-back-to-top @scrolltolower="onScrolltolower" class="viewport" scroll-y>
        <!-- 个人资料 -->
        <view class="profile" :style="{ paddingTop: safeAreaInsets!.top + 'px' }">
            <!-- 情况1: 已登录 -->
            <view class="overview" v-if="memberStore.profile">
                <navigator url="/pagesMember/profile/profile" hover-class="none">
                    <image class="avatar" :src="memberStore.profile.avatar" mode="aspectFill"></image>
                </navigator>
                <view class="meta">
                    <view class="nickname">
                        {{ memberStore.profile.nickname || memberStore.profile.account }}
                    </view>
                    <navigator class="extra" url="/pagesMember/profile/profile" hover-class="none">
                        <text class="update">更新头像昵称</text>
                    </navigator>
                </view>
            </view>
            <!-- 情况2: 未登录 -->
            <view class="overview" v-else>
                <navigator url="/pages/login/login" hover-class="none">
                    <image class="avatar gray" mode="aspectFill"
                        src="https://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-06/db628d42-88a7-46e7-abb8-659448c33081.png">
                    </image>
                </navigator>
                <view class="meta">
                    <navigator url="/pages/login/login" hover-class="none" class="nickname">
                        未登录
                    </navigator>
                    <view class="extra">
                        <text class="tips">点击登录账号</text>
                    </view>
                </view>
            </view>
            <navigator class="settings" url="/pagesMember/settings/settings" hover-class="none">
                设置
            </navigator>
        </view>
        <!-- 我的订单 -->
        <view class="orders">
            <view class="title">
                我的订单
                <navigator class="navigator" url="/pagesOrder/list/list?type=0" hover-class="none">
                    查看全部订单<text class="icon-right"></text>
                </navigator>
            </view>
            <view class="section">
                <!-- 订单 -->
                <navigator v-for="item in orderTypes" :key="item.type" :class="item.icon"
                    :url="`/pagesOrder/list/list?type=${item.type}`" class="navigator" hover-class="none">
                    {{ item.text }}
                </navigator>
                <!-- 客服 -->
                <!-- #ifdef MP-WEIXIN -->
                <button class="contact icon-handset" open-type="contact">售后</button>
                <!-- #endif -->
            </view>
        </view>
        <!-- 猜你喜欢 -->
        <view class="guess">
            <XtxGuess ref="guessRef" />
        </view>
    </scroll-view>
</template>

<style lang="scss">
page {
    height: 100%;
    overflow: hidden;
    background-color: #f7f7f8;
}
</style>
