<template>
	<view class="video_play">
		<image :src="videoObj.img" mode=""></image>
		<view class="video_tool">
			<view @click="handleMuted" :class="['iconfont',muted?'iconjingyin':'iconshengyin'] "></view>
			<view class="iconfont iconzhuanfa">
				<button open-type="share" ></button>
			</view>
		</view>
		<view class="video_wrap">
			<video :muted="muted" :src="videoObj.video" controls objectFit="fill"></video>
		</view>
		<view class="video_download">
			<view class="download_btn" @click="handleDownload">下载高清</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				videoObj:{},
				muted:false
			};
		},
		onLoad() {
			this.videoObj = getApp().globalData.video
		},
		methods:{
			handleMuted(){
				this.muted = !this.muted
			},
			async handleDownload() {
				await uni.showLoading({
					title:"下载中"
				})
				const {tempFilePath} = (await uni.downloadFile({
					url:this.videoObj.video
				}))[1]
				await uni.saveVideoToPhotosAlbum({
					filePath:tempFilePath
				})
				uni.hideLoading()
				await uni.showToast({
					title:"下载成功"
				})
			}
		}
	}
</script>

<style lang="scss">
	.video_play{
		position: relative;
		image{
			position: absolute;
			height: 100vh;
			width: 100vw;
			z-index: -1;
			filter: blur(20px);
		}
		.video_tool{
			display: flex;
			justify-content: flex-end;
			.iconfont {
				width: 80rpx;
				height: 80rpx;
				border-radius: 50%;
				color: white;
				background-color: rgba(0,0,0,.2);
				font-size: 50rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				margin-right: 20rpx;
			}
			.iconzhuanfa{
				position: relative;
				button{
					position: absolute;
					width: 100%;
					height: 100%;
					opacity: 0;
				}
			}
		}
		.video_wrap{
			display: flex;
			justify-content: center;
			video {
				width: 360rpx;
				height: 600rpx;
			}
		}
		.video_download{
			display: flex;
			justify-content: center;
			margin-top: 30rpx;
			.download_btn{
				width: 360rpx;
				height: 80rpx;
				border-radius: 40rpx;
				border: 3rpx solid #fff;
				display: flex;
				justify-content: center;
				align-items: center;
				color: white;
				background-color: rgba(0,0,0,.2);
			}
		}
	}
</style>
