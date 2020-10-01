<template>
	<view>
	  <uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" style-type="text" active-color="#d4237a"></uni-segmented-control>
	  <view class="content">
	    <view v-if="current === 0">
	      <home-recommend></home-recommend>
	    </view>
	    <view v-if="current === 1">
	      <home-category></home-category>
	    </view>
	    <view v-if="current === 2">
	      <home-new></home-new>
	    </view>
	    <view v-if="current === 3">
	      <home-album></home-album>
	    </view>
	  </view>
	</view>
</template>

<script>
	import homeAlbum from "./home-album"
	import homeCategory from "./home-category"
	import homeNew from "./home-new"
	import homeRecommend from "./home-recommend"
	import {uniSegmentedControl} from '@dcloudio/uni-ui'
	export default {
		data() {
			return {
				items: ['推荐','分类','最新','专辑'],
				current: 0
			}
		},
		components: {
		  homeAlbum,
		  homeCategory,
		  homeNew,
		  homeRecommend,
		  uniSegmentedControl
		},
		onLoad() {



		},
		methods: {
			onClickItem(e) {
			  if (this.current !== e.currentIndex) {
			    this.current = e.currentIndex;
			  }
			}
		}
	}
</script>

<style lang="scss">

</style>
