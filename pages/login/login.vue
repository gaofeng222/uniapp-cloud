<template>
	<view>
		<div class="user-info"
			v-if="userInfo">

			<image class="user-info__avatar"
				:src="userInfo.avatarUrl"
				mode="widthFix"></image>

			<!-- <p class="user-info__nickname">
				{{userInfo.nickName}}
			</p> -->
			<input type="nickname"
				v-model="userInfo.nickName" />
		</div>
		<button class="submit-btn"
			open-type="chooseAvatar"
			@chooseavatar="onChooseAvatar">获取用户头像</button>

		<!-- <button class="submit-btn"
			open-type="getUserInfo"
			@getuserinfo="getUserInfo">获取用户信息</button> -->

		<!-- #ifndef H5 -->
		<button open-type="getPhoneNumber"
			@click="handleClick"
			@getphonenumber="getphonenumber">一键登录</button>
		<!-- #endif -->
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'
	const userInfo = ref({
		avatarUrl: 'https://cube.elemecdn.com/9/c2/f0ee8a3c7c9638a54940382568c9dpng.png',
		nickName: ''
	})
	const getUserInfo = (e) => {
		console.log(e, 'phone');
		userInfo.value = e.detail.userInfo
	}
	const onChooseAvatar = e => {
		console.log(e, 'avatar');
		const {
			avatarUrl
		} = e.detail

		userInfo.value.avatarUrl = avatarUrl
	}
	const handleClick = e => {
		uni.login({
			provider: 'univerify',
			univerifyStyle: {
				fullScreen: true
			}
		})
	}
	const getphonenumber = e => {
		console.log(e, 'number');
	}
</script>

<style lang="scss"
	scoped>
	.user-info {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		&__avatar {
			width: 180rpx;
			height: 180rpx;
			border-radius: 50%;
			margin: 30rpx;
		}

		&__nickname {
			font-size: 32rpx;
			color: $uni-text-color-grey;
		}
	}

	.submit-btn {
		width: 280rpx;
		margin: 20rpx auto;
	}
</style>