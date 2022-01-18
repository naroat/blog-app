<template>
	<view>
		<u-navbar @leftClick="$ran.gotoPre" :is-back="false" :title="title" :bgColor="bgColor"></u-navbar>
		<view class="" style="">
			<view class="d-flex flex-column bg-white-color px-30 my-30 py-20">
				<view class="font-weight" style="font-size: 40rpx; min-height: 80rpx;">{{articleData.title}}</view>
				<view class="d-flex flex-column my-20 u-border-radius py-20 px-20" style="background-color:#f3f3f3;">
					<view class="d-flex a-baseline " style="color: #a3a3a3; font-size: 24rpx;">
						<u-icon name="clock" color="#a3a3a3" size="10"></u-icon>
						<text class="mx-10">发布时间:</text>
						<text>{{$u.timeFormat(articleData.created_at, 'y-m-d') }}</text>
					</view>
					
					<view class="d-flex j-sb" style="min-width: 0;margin-top: 10rpx;">
						<view class="d-flex w-100">
							<view v-for="(_item, _index) in articleData.tags" :key="_index" class="d-flex a-baseline" style="margin-right: 20rpx;height:40rpx;">
								<u-icon name="tags-fill" size="10" color="#ff9a00" style="margin-right:10rpx;"></u-icon>
								<text style="font-size: 26rpx; color:#a3a3a3;">{{_item}}</text>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="parse-html bg-white-color px-30 py-10" style="padding-bottom: 20rpx;" v-html="articleData.content_html"></view>
			<!-- <rich-text :nodes="articleData.content_html"></rich-text> -->
			<!-- <u-parse class="bg-white-color px-30" :content="articleData.content_html"></u-parse> -->
			<!-- <mp-html class="parse-html bg-white-color" style="padding:0 30rpx 0 30rpx ;" :content="articleData.content_html" /> -->
		</view>
		
		<view class="" style="text-align: center; color:#a3a3a3;height: 100rpx;line-height: 100rpx;font-size: 26rpx;">
			© 2021-2029 www.ranblogs.com
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: "RanBlogs",
				bgColor: "#3ec1d3",
				id: '',
				articleData: ''
			}
		},
		onLoad(option) {
			console.log(option)
			this.id = option.id
		},
		methods: {
			//获取文章
			getArticleOne() {
				let ajaxData = {}
				uni.$u.http.get(
					'/api/articles/' + this.id,
					{params: ajaxData}
				).then(res => {
					this.articleData = res.data
					// var htmlString = this.articleData.content_html.replace(/\\/g, "").replace(/<img/g, "<img style=\"max-width:100%;\"");
					// this.articleData.content_html = HTMLParser(htmlString);
				}).catch(err => {
				
				})
			}
		},
		mounted() {
			this.getArticleOne()
		}
	}
</script>

<style lang="scss">

</style>
