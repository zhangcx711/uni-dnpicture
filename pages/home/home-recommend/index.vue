<template>
	<scroll-view @scrolltolower="handleToLower" class="recommend_view" scroll-y="true" v-if="recommend.length > 0">
		<view class="recommend_wrap" >
		  <view class="recommend_item" v-for="item in recommend" :key="item.id">
		    <image mode="widthFix" :src="item.thumb"></image>
		  </view>
			<!-- 月份 开始 -->
			<view class="monthes_wrap">
			  <view class="monthes_title">
			    <view class="monthes_title_info">
			      <view class="monthes_info">
			        <text> {{monthes.DD}} / </text>
			        {{monthes.MM}} 月
			      </view>
			      <view class="monthes_text"> {{monthes.title}} </view>
			    </view>
			    <view class="monthes_title_more">更多 > </view>
			  </view>
			  <view class="monthes_content">
			    <view class="monthes_items" v-for="(item,index) in monthes.items" :key="item.id">
						<go-detail :list="monthes.items" :index="index">
							<image :src="item.img" mode="widthFix"></image>
						</go-detail>
			    </view>
			  </view>
				<!-- 热门 -->
				<view class="hot_wrap">
					<view class="hot_title">
						<text>热门</text>
					</view>
					<view class="hot_content">
						<view class="hot_item" v-for="(item,index) in hot" :key="item.id">
							<go-detail :list="hot" :key="index">
								<image :src="item.thumb" mode="widthFix"></image>
							</go-detail>
						</view>
					</view>
				</view>
			</view>
		</view>
	</scroll-view>
	
</template>

<script>
	import moment from 'moment'
	import goDetail from '@/components/goDetail'
	export default {
		components: {
			goDetail
		},
		data() {
			return {
				// 推荐
				recommend:[],
				// 月份
				monthes:[],
				// 热门
				hot:[],
				// 请求参数
				params: {
					limit:30,
					order:'hot',
					skip:0
				},
				// 是否还有下一页
				hasmore:true
			};
		},
		mounted() {
			this.getList(),
			uni.setNavigationBarTitle({
				title:"推荐"
			})
		},
		methods:{
			handleToLower(){
				if(this.hasmore) {
					this.params.skip += this.params.limit
					this.getList()
				}else{
					uni.showToast({
						title:"没有数据了",
						icon:"none"
					})
				}
				
			},
			// 获取接口数据
			getList() {
				this.request({
				  url:"http://157.122.54.189:9088/image/v3/homepage/vertical",
				  data:this.params
				}).then(res => {
					if(res.res.vertical.length === 0) {
						this.hasmore = false;
						return;
					}
					if(this.recommend.length === 0) {
						// 推荐模块
						this.recommend = res.res.homepage[1].items
						// 月份模块
						this.monthes = res.res.homepage[2]
						this.monthes.MM = moment(this.monthes.stime).format("MM")
						this.monthes.DD = moment(this.monthes.stime).format("DD")
						
					}
					this.hot = [...this.hot,...res.res.vertical]
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.recommend_view {
		height: calc(100vh - 36px);
	}
	.recommend_wrap {
	  display: flex;
	  flex-wrap: wrap;
	  .recommend_item {
	    width: 50%;
	    border: 5rpx solid #fff;
	    image {
	      width: 100%;
	      height: 100%;
	    }
	  }
	}
	.monthes_wrap {
		.monthes_title {
			width: 750rpx;
			display: flex;
			justify-content: space-between;
			padding:20rpx;
			.monthes_title_info {
				color: $color;
				font-size: 30rpx;
				font-weight: 600;
				display: flex;
				.monthes_info {
					text {
						font-size: 36rpx;
					}
				}
				.monthes_text {
					font-size: 34rpx;
					color: #666;
					margin-left: 30rpx;
				}
			}
			.monthes_title_more {
				font-size: 24rpx;
				color: $color;
			}
		}
		.monthes_content {
			display: flex;
			flex-wrap: wrap;
			.monthes_items {
				width: 100%;
				border:5px solid #fff
			}
		}
	}
	.hot_wrap {
		.hot_title {
			padding-left: 20rpx;
			text{
				border-left: 20rpx solid $color;
				padding-left: 20rpx;
				font-size: 34rpx;
				font-weight: 600;
			}
		}
		.hot_content {
			display: flex;
			flex-wrap: wrap;
			.hot_item {
				width: 50%;
				border:5px solid #fff
			}
		}
	}
</style>
