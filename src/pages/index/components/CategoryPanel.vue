<script setup lang="ts">
import { onMounted } from 'vue';
import type { CategoryItem } from '@/types/home';

// 定义 props 接收数据
const props = withDefaults(defineProps<{
	list: CategoryItem[]
}>(), {
	list: () => []
})

</script>

<!-- 首页分类 -->
<template>
	<view class="category">
		<navigator class="category-item" hover-class="none" url="/pages/index/index" v-for="item in list" :key="item.id">
			<image class="icon" :src="item.icon"></image>
			<text class="text">{{ item.name }}</text>
		</navigator>
	</view>
</template>

<style lang="scss">
@import "../styles/category.scss";
</style>
