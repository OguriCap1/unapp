<template>
	<view>
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<!-- 循环渲染轮播图的 item 项 -->
			<swiper-item v-for="(item, i) in swiperList" :key="i">
				<view class="swiper-item">
					<!-- 动态绑定图片的 src 属性 -->
					<image :src="item.image_src"></image>
				</view>
			</swiper-item>
		</swiper>
		<!-- 分类导航区域 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item, i) in navList" :key="i">
				<image :src="item.image_src" class="nav-img"></image>
			</view>
		</view>
		<!-- 楼层区域 -->
		  <view class="floor-list" v-for="item,index in floorList" :key="index">
		   <!-- 楼层 item 项 -->
		      <view class="floor-item" >
		      <!-- 楼层标题 -->
		       <image :src="item.floor_title.image_src" class="floor-title"></image>
		      </view>
		      <!-- 楼层图片区域 -->
		      <view class="floor-img-box"  >
		      <!-- 左侧大图片的盒子 -->
		      <view  class="left-img-box">
		          <image  :src="item.product_list[0].image_src" :style="{width:item.product_list[0].image_width+'rpx'}" mode="widthFix"></image>
		      </view>
		      <!-- 右侧 4 个小图片的盒子 -->
		      <view class="right-img-box">
		          <view v-if="index2 != 0" class="right-img-item" v-for="item2,index2 in item.product_list" :key="index2" >
		          <image :src="item2.image_src" :style="{width:item2.image_width+'rpx'}" mode="widthFix" ></image>
		          </view>
		      </view>
		      </view>
		  </view>
		 </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 1. 轮播图的数据列表，默认为空数组
				swiperList: [],
				// 1. 分类导航的数据列表
				navList: [],
				// 1. 楼层的数据列表
				floorList: []
			};
		},
		onLoad() {
			// 2. 在小程序页面刚加载的时候，调用获取轮播图数据的方法
			this.getSwiperList()
			// 2. 在 onLoad 中调用获取数据的方法
			this.getNavList()
			// 2. 在 onLoad 中调用获取楼层数据的方法
			this.getFloorList()
		},
		methods: {
			// 3. 获取轮播图数据的方法
			async getSwiperList() {
				// 3.1 发起请求
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/swiperdata')
				// 3.2 请求失败
				if (res.meta.status !== 200) {
					return uni.showToast({
						title: '数据请求失败！',
						duration: 1500,
						icon: 'none',
					})
				}
				// 3.3 请求成功，为 data 中的数据赋值
				this.swiperList = res.message
			},
			// 3. 在 methods 中定义获取数据的方法
			async getNavList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/catitems')
				if (res.meta.status !== 200) return uni.$showMsg()
				this.navList = res.message
			},
			// 3. 定义获取楼层列表数据的方法
			async getFloorList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/floordata')
				if (res.meta.status !== 200) return uni.$showMsg()
				this.floorList = res.message
			},
		},
	}
</script>

<style lang="scss">
	swiper {
		height: 330rpx;

		.swiper-item,
		image {
			width: 100%;
			height: 100%;
		}
	}

	.nav-list {
		display: flex;
		justify-content: space-around;
		margin: 15px 0;

		.nav-img {
			width: 128rpx;
			height: 140rpx;
		}
	}

	.floor-title {
		height: 60rpx;
		width: 100%;
		display: flex;
	}

	.right-img-box {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.floor-img-box {
		display: flex;
		padding-left: 10rpx;
	}

	.right-img-box {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.floor-img-box {
		display: flex;
		padding-left: 10rpx;
	}

	.right-img-box {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.floor-img-box {
		display: flex;
		padding-left: 10rpx;
	}
</style>
