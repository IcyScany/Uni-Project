<template>
	<view class="content">
		<input class="input" type="text" v-model="content" placeholder="留言">
		<button type="primary" @click="publish()" size="mini">发布</button>
	</view>
	<view v-for="item in list" :key="item._id">
		<view>
			<view class="box">{{item.content}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content: '',
				list: []
			}
		},
		onLoad() {
			uniCloud.callFunction({
				name: "fun1",
				data: {
					api: "getMessages",
				}
			}).then(res => {
				this.list = res.result.data
			})
		},
		methods: {
			publish() {
				uniCloud.callFunction({
					name: 'fun1',
					data: {
						api: 'publish',
						content: this.content
					}
				}).then(res => {
					console.log(res)
					// this.list.push({
					// 	_id: res.result.id,
					// 	content: this.content,
					// })
					uni.showToast({
						title: '留言成功，待审核...',
						icon: 'success'
					})
					this.content = ""
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		align-items: center;
		margin: 40rpx;
	}

	.input {
		border-bottom: 1px solid black;
		padding: 6rpx;
		flex: 1;
	}

	.box {
		margin: 40rpx;
		padding: 20rpx;
		border-bottom: 1px solid #ccc;
	}
</style>
