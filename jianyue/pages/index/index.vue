<template>
	<view class="container">
		<view class="article-box">
			<view class="article" v-for="(article, index) in articles" :key="index">
				<!-- 标题 -->
				<text class="article-title" @tap="gotoDetail(article.id)">{{ article.title }}</text>
				<!-- 大于等于三张图片的显示方式 -->
				<view class="" v-if="article.imgs.length >= 3">
					<view class="thumbnail-box">
						<view
							class="thumbnail-item"
							v-for="(item, index1) in article.imgs"
							:key="index1"
							v-if="index1 < 3"
						>
							<image :src="item.imgUrl"></image>
						</view>
					</view>
				</view>
				<!-- 小于三张图片的显示方式 -->
				<view class="" v-else-if="article.imgs.length >= 1">
					<view class="text-img-box">
						<view class="left">
							<text>{{ handleContent(article.content) }}...</text>
						</view>
						<view class="right">
							<image :src="article.imgs[article.imgs.length - 1].imgUrl"></image>
						</view>
					</view>
				</view>
				<!-- 没有图片的显示方式 -->
				<view class="text-box" v-else>
					<text>{{ handleContent(article.content) }}...</text>
				</view>
				<!-- 文章作者等信息 -->
				<view class="article-info">
					<image :src="article.avatar" class="avatar small"></image>
					<text class="info-text">{{ article.nickname }}</text>
					<text class="info-text">{{ handleTime(article.createTime) }}</text>
				</view>
			</view>
		</view>
		<!-- <view class="green-btn">
			<button type="primary" class="btn" >
				<text class="icon-text">+</text>
			</button>
		</view> -->
	</view>
</template>

<script>
export default {
	data() {
		return {
			articles: []
		};
	},
	onLoad: function() {
		this.getArticles();
	},
	onShow: function() {},
	onPullDownRefresh: function() {
		this.getArticles();
	},
	methods: {
		getArticles: function() {
			var _this = this;
			uni.request({
				url: this.apiServer + '/article/list',
				method: 'GET',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				success: res => {
					_this.articles = res.data.data;
				},
				complete: function() {
					uni.stopPullDownRefresh();
				}
			});
		},
		gotoDetail: function(aId) {
			uni.navigateTo({
				url: '../article_detail/article_detail?aId=' + aId
			});
		},
		handleTime: function(date) {
			var d = new Date(date);
			var year = d.getFullYear();
			var month = d.getMonth() + 1;
			var day = d.getDate() < 10 ? '0' + d.getDate() : '' + d.getDate();
			var hour = d.getHours() < 10 ? '0' + d.getHours() : '' + d.getHours();
			var minutes = d.getMinutes() < 10 ? '0' + d.getMinutes() : '' + d.getMinutes();
			var seconds = d.getSeconds() < 10 ? '0' + d.getSeconds() : '' + d.getSeconds();
			return year + '-' + month + '-' + day + ' ' + hour + ':' + minutes + ':' + seconds;
		},
		handleContent: function(content) {
			content = content.replace(/(\n)/g, '');
			content = content.replace(/(\t)/g, '');
			content = content.replace(/(\r)/g, '');
			content = content.replace(/<\/?[^>]*>/g, '');
			content = content.replace(/\s*/g, '');
			return content.substring(0, 20);
		}
	}
};
</script>

<style scoped>
.icon-text {
	color: #ffffff;
	font-size: 35px;
}
.article-box {
	height: auto;
	width: 100%;
}
.article {
	display: flex;
	flex-direction: column;
}
.thumbnail-box {
	margin-top: 10px;
	display: flex;
	height: 130px;
	width: 100%;
	border-bottom: 1px solid #e6e7e9;
}
.thumbnail-item {
	flex: 1 1 33%;
	height: 110px;
	margin-right: 5px;
	margin-left: 5px;
}
.thumbnail-item image {
	width: 100%;
	height: 100%;
}
.left {
	flex: 1 1 60%;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 4;
	overflow: hidden;
}
.right {
	flex: 1 1 40%;
	width: 100%;
	height: 100%;
}
.right image {
	width: 90%;
	height: 200upx;
}
.article-info {
	color: #e9e9e9;
	width: 100%;
	margin-left: 10upx;
}
.avatar {
	width: 75upx;
	height: 75upx;
	border-radius: 50%;
}
.content {
	text-align: center;
}
.logo {
	height: 200upx;
	width: 200upx;
	margin-top: 200upx;
}
.add-title {
	/* float: right; */
	width: 15%;
	position: absolute;
	right: 0px;
	bottom: 0px;
}

.article {
	display: flex;
	flex-direction: column;
	width: 100%;
}
.text-img-box {
	display: flex;
	flex-direction: row;
}
.thumbnail-box {
	display: flex;
	flex-direction: row;
}
.thumbnail-item image {
	width: 220upx;
	height: 220upx;
	margin-left: 25upx;
}
.left {
	flex: 1 1 60%;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 4;
	overflow: hidden;
}
.right {
	flex: 1 1 40%;
	width: 100%;
	height: 100%;
}
.right image {
	width: 90%;
	height: 200upx;
}
.article-info {
	color: #e9e9e9;
	width: 100%;
	margin-left: 10upx;
}
.avatar {
	width: 75upx;
	height: 75upx;
	border-radius: 50%;
}
</style>
