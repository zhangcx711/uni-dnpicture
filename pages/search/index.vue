<template>
	<view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
			};
		},
		onLoad() {
			uni.request({
				url:`http://157.122.54.189:9088/image/v1/vertical/category`
			}).then(res => {
				console.log(res)
			})
			this.request({
				url:`http://157.122.54.189:9088/image/v1/vertical/category`
			}).then(res => {
				console.log(res)
			})
		}
	}
</script>

<style lang="scss">

</style>
