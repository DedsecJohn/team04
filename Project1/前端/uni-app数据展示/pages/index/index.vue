<template>
	<view>
		<view @click="addSwitch()"><button>添加商品/确定</button></view>
		<view v-show="addshow[0]">
			<input class="login-input" type="text" v-model="img_url" placeholder="图片地址">
			<input class="login-input" type="text" v-model="pro_id" placeholder="商品编号">
			<input class="login-input" type="text" v-model="pro_name" placeholder="商品名称">
			<input class="login-input" type="text" v-model="pro_price" placeholder="商品价格">
			<input class="login-input" type="text" v-model="pro_desc" placeholder="商品描述">
			<view>
				<u-button @click="addProduct()" form-type="submit">提交</u-button>
			</view>
		</view>

		<u-card v-for="(item,index) in productList" :key="item.pro_id">
			<view slot="body">
				<view class="u-body-item u-flex u-border-bottom u-col-between u-p-t-0">
					<image :src="item.img_url" mode="aspectFill"></image>
					<view>
						<u-cell-group>
							<u-cell-item  :arrow="false" title="商品名称" :value="item.pro_name" title-width="300"
							:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
							<u-cell-item  :arrow="false" title="商品价格" :value="'¥'+item.pro_price" title-width="300"
							:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
							<u-cell-item  title="商品详情" :arrow="false" value="点击查看商品详细信息" title-width="300"
							:title-style="titleStyle" :value-style="textStyle1" @click="gotoDetail(index)">
								<u-icon slot="right-icon" size="50rpx" name="chat-fill" :label="item.pro_comment.length+'评论'"></u-icon>
							</u-cell-item>
						</u-cell-group>
					</view>
				</view>
			</view>
			
			<view slot="foot">
				<u-tr>
					<u-td>
						<u-button @click="itemsShow(index)">修改/确定</u-button>
					</u-td>
					<u-td>
						<u-button @click="deleteProduct(index)">删除</u-button>
					</u-td>
				</u-tr>
				<view v-show='itemshow[index]'>
					图片地址: <input class="login-input" type="text" v-model="item.img_url" placeholder="图片地址">
					商品编号: <input class="login-input" type="text" v-model="item.pro_id" placeholder="商品编号">
					商品名称: <input class="login-input" type="text" v-model="item.pro_name" placeholder="商品名称">
					商品价格: <input class="login-input" type="text" v-model="item.pro_price" placeholder="商品价格">
					商品描述: <input class="login-input" type="text" v-model="item.pro_desc" placeholder="商品描述">
				</view>
				<view v-show='itemshow[index]'>
					<u-button v-show="false" @click="modifyProduct(index)" form-type="submit">提交</u-button>
				</view>
			</view>
		</u-card>
		
		<!-- <u-row gutter="20">
			<u-col span="6" v-for="(item,index) in productList" :key="item.pro_id">
				<u-row gutter="10">
					<u-col span="3" class="pro_info">
						<image :src="item.img_url"></image>
					</u-col>
					<u-col span="9">
						<u-cell-group>
							<u-cell-item  :arrow="false" title="商品名称" :value="item.pro_name" title-width="150"
							:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
							<u-cell-item  :arrow="false" title="商品价格" :value="item.pro_price" title-width="150"
							:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
							<u-cell-item  title="商品详情" value="点击查看商品详细信息" title-width="150"
							:title-style="titleStyle" :value-style="textStyle1"></u-cell-item>
						</u-cell-group>
					</u-col>
				</u-row>
			</u-col>
			
		</u-row> -->
	</view>
</template>


<script>
	export default {
		data() {
			return {
				// 产品的列表信息
				productList: [],
				itemshow: [],
				addshow: [],
				titleStyle:{
					"font-size":"50rpx",
					"font-weight":"bold"
				},
				textStyle1:{
					"font-size":"50rpx",
					"text-align":"left"
				}
			};
		},
		onLoad: function() {
			this.getlist().then(res => {
				this.productList = res.data.productList
			})

		},
		methods: {
			//获取产品的信息
			getlist() {
				return new Promise((resolve, reject) => {
					// 封装主体：网络请求
					uni.request({
						url: "https://www.fastmock.site/mock/52fc9552714869da30453dbf8633b489/api/mdata",
						method: 'POST',
						success: (res) => {
							resolve(res.data)
						},
						fail: (err) => {
							uni.showToast({
								title: '请求接口失败'
							})
							reject(err)
						}
					})
				})
			},
			gotoDetail(i) {
				getApp().globalData.product = this.productList[i]
				uni.navigateTo({
					url: '../detail/detail'
				})
			},
			deleteProduct(i) {
				return this.productList.splice(i, 1);
			},

			addProduct() {
				return this.productList.splice(0, 0, {
					"img_url": this.img_url,
					"pro_id": this.pro_id,
					"pro_name": this.pro_name,
					"pro_price": this.pro_price,
					"pro_desc": this.pro_desc,
					"pro_comment":[]
				});
			},
			modifyProduct(i) {
				return this.productList.splice(i, 1, {
					"img_url": this.item.img_url,
					"pro_id": this.item.pro_id,
					"pro_name": this.item.pro_name,
					"pro_price": this.item.pro_price,
					"pro_desc": this.item.pro_desc
				});
			},

			addSwitch() {
				let arr = this.addshow[0];
				arr = !arr
				this.addshow.splice(0, 1, arr);
			},
			itemsShow(index) {
				let arr = this.itemshow[index];
				arr = !arr
				this.itemshow.splice(index, 1, arr);
			}
		}
	};
</script>

<style scoped lang="scss">
	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 10px;
	}

	.u-body-item {
		padding:10rpx;
	}

	.u-body-item image {
		width: 400rpx;
		flex: 0 0 400rpx;
		height: 400rpx;
		border-radius: 10rpx;
		margin-left: 20rpx;
	}
	
</style>
