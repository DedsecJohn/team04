<template>
	<view class="mainView">
		<u-row class="info" gutter="10">
			<u-col span="3">
				<image :src="pro_detail.img_url"></image>
			</u-col>
			<u-col span="9">
				<u-cell-group>
					<u-cell-item  :arrow="false" title="商品ID" :value="pro_detail.pro_id" title-width="300"
					:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
					<u-cell-item  :arrow="false" title="商品名称" :value="pro_detail.pro_name" title-width="300"
					:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
					<u-cell-item  :arrow="false" title="商品价格" :value="pro_detail.pro_price" title-width="300"
					:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
					<u-cell-item  :arrow="false" title="商品描述" :value="pro_detail.pro_desc" title-width="300"
					:title-style="titleStyle" :value-style="textStyle2"></u-cell-item>
				</u-cell-group>
			</u-col>
		</u-row>
		<view class="title">商品评论</view>
		<view class="commentArea" v-for="(item,index) in pro_detail.pro_comment">{{item}}</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pro_detail:{},
				titleStyle:{
					"font-size":"50rpx",
					"font-weight":"bold"
				},
				textStyle1:{
					"font-size":"50rpx",
					"text-align":"left"
				},
				textStyle2:{
					"font-size":"45rpx",
					"text-align":"left"
				}
			}
		},
		onLoad:function(option){
			this.pro_detail = getApp().globalData.product
		},
		methods: {
			
		}
	}
</script>

<style scoped lang="scss">
	.mainView{
		padding: 20rpx;
	}
	
	.info{
		margin: 20rpx;
	}	
		
	.title{
		margin:20rpx;
		font-size: 50rpx;
		font-weight: blue($color: #000000);
		font-style: italic;
	}
		
	.commentArea{
		margin:20rpx;
		font-size: 40rpx;
	}
</style>
