<template>
	<scroll-view scroll-y enable-flex class="video_main" @scrolltolower="handleScrolltolower">
		<view class="video_item" v-for="item in videowp" :key="item.id" @click="handleGoVideo(item)">
			<image :src="item.img" mode="widthFix"></image>
		</view>
	</scroll-view>
</template>

<script>
	export default {
		props:{
			urlobj:Object
		},
		data() {
			return {
				videowp:[],
				hasMore:true
			};
		},
		watch:{
			urlobj() {
				this.videowp=[]
				this.getList()
			}
		},
		mounted() {
			this.getList()
		},
		methods:{
			getList() {
				this.request({
					url: this.urlobj.url,
					data: this.urlobj.params,
				}).then(res => {
					if(res.res.videowp.length === 0) {
						this.hsaMore = false
						uni.showToast({
							title:"没有更多数据了",
							icon:"none"
						})
						return
					}
					this.videowp = [...this.videowp,...res.res.videowp]
				})
			},
			handleScrolltolower() {
			if(this.hasMore) {
					this.urlobj.params.skip += this.urlobj.params.limit
					this.getList()
				}else{
					uni.showToast({
						title:"没有更多数据了", 
						icon:"none"
					})
				}
			},
			handleGoVideo(item) {
				getApp().globalData.video = item;
				uni.navigateTo({
					url:"/pages/videoPlay/index"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.video_main {
		display: flex;
		flex-wrap: wrap;
		height: calc(100vh -36px);
		.video_item {
			border: 5rpx solid #fff;
			width: 33.33%;
		}
	}
</style>