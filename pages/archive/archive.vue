<template>
	<view>
		<u-navbar :is-back="false" :title="title" :bgColor="bgColor"></u-navbar>
		<view class="" v-for="(item, index) in archiveList" :key="index">
			<view class="font-weight my-30 px-30 bg-white-color" style="font-size: 60rpx;height: 80rpx;">
				{{item.year}}
			</view>
			<view class="mx-30 my-30">
				<u-steps current="-1" dot direction="column" inactiveColor="#3ec1d3" style="margin-right: 10rpx;">
					<u-steps-item
					v-for="(_item, _i) in item.list"
					class="u-line-1 my-20"
						:title="_item.title"
						:desc="_item.created_at_format"
					>
					<u-icon class="px-10 py-10" name="clock-fill" slot="icon" color="#3ec1d3" style="background-color: #f3f3f3;"></u-icon>
					</u-steps-item>
				</u-steps>
			</view>
		</view>
		<!-- 底部bar占位 -->
		<view style="height:100rpx;"></view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				title: "归档 - RanBlogs",
				bgColor: "#3ec1d3",
				archiveList: []
			}
		},
		methods: {
			//获取归档
			getArchive() {
				let ajaxData = {}
				uni.$u.http.get(
					'/api/article/archives',
					{params: ajaxData}
				).then(res => {
					this.archiveList = res.list
				}).catch(err => {
				
				})
			}
		},
		mounted() {
			this.getArchive()
		}
	}
</script>

<style>

</style>
