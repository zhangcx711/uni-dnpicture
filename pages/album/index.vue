<template>
	<view>
		<!-- 背景图 -->
		<view class="album_wrap">
			<view class="album_img">
				<image :src="album.cover" mode=""></image>
			</view>
			<view class="album_info">
				<view class="album_title">
					<text>{{album.name}}</text>
				</view>
				<view class="album_attention">
					<text>关注专辑</text>
				</view>
			</view>
		</view>
		<!-- 描述 -->
		<view class="album_word">
			<view class="album_userinfo">
				<image :src="album.user.avatar" mode="widthFix"></image>
				<text>{{album.user.name}}</text>
			</view>
			<view class="album_desc">
				<text>{{album.desc}}</text>
			</view>
		</view>
		<!-- 列表 -->
		<view class="album_list">
		  <view
		    class="album_item"
		    v-for="(item,index) in wallpaper"
		    :key="item.id"
		  >
				<go-detail :list="wallpaper" :index="index">
					<image
						mode="aspectFill"
						:src="item.thumb+item.rule.replace('$<Height>',360)"
					></image>
		   </go-detail>
		  </view>
		</view>
	</view>
</template>

<script>
	import goDetail from "../../components/goDetail.vue"
	export default {
		components:{
			goDetail
		},
		data() {
			return {
				params: {
					limit:30,
					order:"new",
					skip:0,
					first:1
				},
				id:"",
				album:[],
				wallpaper:[],
				hasMore:true
			};
		},
		onLoad(option) {
			console.log(option)
			this.id = option.id
			this.getList()
		},
		onReachBottom() {
			if(this.hasMore) {
				this.params.first = 0
				this.params.skip = this.params.limit
				this.getList()
			}else {
        uni.showToast({
          title: "没有数据了",
          icon: "none"
        });
			}
		},
		methods:{
			getList() {
				this.request({
					url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
					data: this.params,
				}).then(res => {
					if (Object.keys(this.album).length === 0) {
					  this.album = res.res.album;
					}
					
					if (res.res.wallpaper.length === 0) {
					  this.hasMore = false;
					  uni.showToast({
					    title: "没有更多数据了",
					    icon: "none"
					  });
					  return;
					}
					this.wallpaper = [...this.wallpaper,...res.res.wallpaper]
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.album_wrap {
		position: relative;
		.album_img {
			
		}
		.album_info{
			position: absolute;
			width: 100%;
			height: 80rpx;
			bottom:0;
			left: 0;
			display: flex;
			justify-content: space-between;
			align-items: center;
			color: #fff;
			padding: 0 15rpx;
			.album_title {
				font-size: 40rpx;
			}
			.album_attention {
				background-color: $color;
				width: 152rpx;
				height: 50rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				border-radius: 10rpx;
			}
		}
	}
	.album_word {
		padding: 10rpx;
		.album_userinfo {
			display: flex;
			image{
				width: 50rpx;
			}
			text {
				font-weight: 500;
			}
		}
		.album_desc {
			color: #000;
			margin-left: 15rpx;
		}
	}
	.album_list {
	  display: flex;
	  flex-wrap: wrap;
	  .album_item {
	    width: 33.33%;
	    border: 3rpx solid #fff;
	    image {
	      height: 160rpx;
	    }
	  }
	}
</style>
