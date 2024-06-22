<template>
	<view class="content">
		<!-- <uni-table class="cus-table"
			stripe
			emptyText="暂无更多数据">
			<view class="btn-action">
				<button @click="handleAdd"
					type="primary"
					inline>新增数据</button>
			</view>
			<uni-tr>
				<uni-th>序号</uni-th>
				<uni-th>封面</uni-th>
				<uni-th>标题</uni-th>
				<uni-th>内容</uni-th>
				<uni-th>分类</uni-th>
			</uni-tr>
			<uni-tr v-for="(item,index) in articlesList"
				:key="item._id">
				<uni-td>{{index+1}}</uni-td>
				<uni-td @click="handlePreview(item.thumb)">
					<image :src="item.thumb"
						mode="aspectFit"></image>
				</uni-td>
				<uni-td>{{item.title}}</uni-td>
				<uni-td>{{item.content}}</uni-td>
				<uni-td>{{item.catagory}}</uni-td>
			</uni-tr>
		</uni-table> -->

		<unicloud-db ref="udb"
			:page-size="3"
			v-slot:default="{data, loading, error, options}"
			collection="gf-article">
			<view>
				<view v-if="error"
					class="error">{{error.message}}</view>
				<view v-else-if="loading">正在加载...</view>
				<uni-table v-else
					class="cus-table"
					stripe
					emptyText="暂无更多数据">
					<view class="btn-action">
						<button @click="handleAdd"
							type="primary"
							inline>新增数据</button>
					</view>
					<uni-tr>
						<uni-th>序号</uni-th>
						<uni-th>封面</uni-th>
						<uni-th>标题</uni-th>
						<uni-th>内容</uni-th>
						<uni-th>分类</uni-th>
					</uni-tr>
					<uni-tr v-for="(item,index) in data"
						:key="item._id">
						<uni-td>{{index+1}}</uni-td>
						<uni-td @click="handlePreview(item.thumb)">
							<image :src="item.thumb"
								mode="aspectFit"></image>
						</uni-td>
						<uni-td>{{item.title}}</uni-td>
						<uni-td>{{item.content}}</uni-td>
						<uni-td>{{item.catagory}}</uni-td>
					</uni-tr>
				</uni-table>
			</view>
		</unicloud-db>
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue';
	import {
		onReachBottom,
		onPullDownRefresh
	} from "@dcloudio/uni-app"
	const articlesList = ref({})
	const db = uniCloud.database()
	const udb = ref(null)
	const getArticleLists = async () => {
		const res = await db.collection('gf-article').get();
		console.log(res, 'lists');
		articlesList.value = res.result.data
	}
	getArticleLists()

	const handleAdd = async () => {
		//新增一条数据
		const data = {
			title: "风景图片33",
			content: "风景图片33风景图片33风景图片33风景图片33风景图片33风景图片33风景图片33",
			thumb: "http://img0.baidu.com/it/u=1381580334,1246036276&fm=253&app=138&f=JPEG?w=1200&h=800"
		}
		await db.collection('gf-article').add(data);
		getArticleLists()
	}
	const handlePreview = (data) => {
		uni.previewImage({
			urls: [data],
		})
	}
	onReachBottom(() => {
		console.log('8989')
		udb.value.loadMore()
	})
	onPullDownRefresh(() => {
		console.log("123");
		udb.value.loadMore({
				clear: true
			},
			() => {
				uni.stopPullDownRefresh()
			})
	})
</script>

<style lang="scss"
	scoped>
	.content {
		.cus-table {
			width: 100%;
		}
	}

	.btn-action {
		margin: 20rpx;
	}
</style>