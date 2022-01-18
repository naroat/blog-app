<template>
	<view class="">
		<u-navbar :is-back="false" :title="title" :bgColor="bgColor"></u-navbar>
		<!-- 头部信息 -->
		<!-- <r-header></r-header> -->
		<!-- 轮播图 -->
		<view class="py-20 px-30" style="background: linear-gradient(#3ec1d3, #f3f3f3);">
			<u-swiper
				:list="swiperList"
				keyName="image"
				indicator
				:autoplay="false"
				radius="5"
				circular
				height="180"
			    ></u-swiper>
		</view>
		
		<!-- 搜索 -->
		<view class="mx-30 px-20 py-20 bg-white-color u-border-radius">
			<u-search v-model="keyword" @search="searchArticle" @custom="searchArticle"></u-search>
		</view>
		
		<!-- 文章块 -->
		<view v-for="(item, index) in articleList" :key="index" class="mx-30 bg-white-color py-20 u-border-radius" style="height:250rpx; margin-top: 20rpx;">
			<view class="d-flex j-sb px-20 flex-shrink h-100" style="height: 40rpx; line-height: 25rpx; border-bottom:2rpx solid #f4f4f4; color:#a3a3a3;">
				<text class="d-flex a-baseline" style="font-size: 26rpx;">程序人生</text>
				<view class="d-flex a-baseline">
					<u-icon name="clock" size="10" color="#ff9a00" style="margin-right:10rpx;"></u-icon>
					<text style="font-size: 26rpx;">发布时间：{{$u.timeFormat(item.created_at, 'y-m-d') }}</text>
				</view>
			</view>
			<view class="mx-20" style="margin-top:10rpx;" @click="$ran.goto('pages/article/article', {id: item.id})">
				<view class="font-weight u-line-1" style="">{{item.title}}</view>
				<view class="u-line-2 my-20" style="font-size:32rpx;color:#4f5155;">{{$ran.filterHtml(item.content_html)}}</view>
				<view class="d-flex j-sb" style="min-width: 0;">
					<view class="d-flex w-100">
						<view v-for="(_item, _index) in item.tags" :key="_index" class="d-flex a-baseline" style="margin-right: 20rpx;height:40rpx;">
							<u-icon name="tags-fill" size="10" color="#ff9a00" style="margin-right:10rpx;"></u-icon>
							<text style="font-size: 26rpx; color:#a3a3a3;">{{_item}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 返回顶部 -->
		<u-back-top :scroll-top="scrollTop"></u-back-top>
		<!-- 加载更多 -->
		<u-loadmore :status="status" />
		<!-- 底部bar占位 -->
		<view style="height:100rpx;"></view>
	</view>

</template>

<script>
	import swiperList from "@/ran-common/data/swiper.js"
	export default {
		data() {
			return {
				title: "RanBlogs",
				bgColor: "#3ec1d3",
				swiperList: swiperList,
				scrollTop: 0,
				status: 'loadmore',
				currenttPage: 0, //当前页
				nextPage: 0, //下一页
				totalPage: 0, //总页数
				articleList: [],
				keyword: ''
			}
		},
		onPageScroll(e) {
			this.scrollTop = e.scrollTop;
		},
		onReachBottom() {
			if(this.currenttPage >= this.totalPage) {
				this.status = 'nomore'
				return ;
			}
			this.status = 'loading';
			setTimeout(() => {
				if (this.currenttPage < this.nextPage) {
					//可以翻页
					this.getArticle(true);
					this.status = 'loading'
				} else {
					this.status = 'nomore'
				}
			}, 2000)
		},
		onLoad() {

		},
		methods: {
			//搜索文章
			searchArticle(e) {
				this.keyword = e
				this.getArticle()
			},
			//获取文章
			getArticle(next = false) {
				let ajaxData = {}
				if (next) {
					//下一页
					ajaxData.page = this.nextPage;
				}

				if (ajaxData.keyword != "") {
					ajaxData.title = this.keyword;
				}
				uni.$u.http.get(
					'/api/articles',
					{params: ajaxData}
				).then(res => {
					this.currenttPage = res.list.current_page
					this.nextPage = res.list.last_page
					this.totalPage = res.list.last_page
					if (this.currenttPage > 1) {
						this.articleList = this.articleList.concat(res.list.data)
					} else {
						this.articleList = res.list.data
					}
				}).catch(err => {
				
				})
			}
		},
		mounted: function(){
			this.getArticle();
			console.log(this.articleList)
		}
	}
</script>

<style>

</style>
