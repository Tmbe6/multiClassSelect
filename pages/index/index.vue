<template>
	<view class="content">
		<button class="title" type="primary" @click="showClassSelect">打开分类菜单</button>
		<!-- 分类侧边弹窗 -->
		<view class="cu-modal drawer-modal justify-start" :class="showSelectClass==true ?'show':''" @click="classSelect">
			<view class="cu-dialog basis-lg" @click.stop="" :style="[{top:StatusBar+'px',height:'calc(100vh - ' + StatusBar + 'px)'}]">
				<!-- 分类选择组件 -->
				<class-select @classSelect="classSelect" ref="classSelect" :selectList="selectList" :level="3"></class-select>
			</view>
		</view>
	</view>
</template>

<script>
	import classSelect from '../../components/classSelect/classSelect.vue';
	export default {
		components:{classSelect},
		data() {
			return {
				showSelectClass: false,
				StatusBar: this.StatusBar, //手机顶部站位
				selectList: [
					{
						cnname: '一建',
						open: false,
						child:[
							{
								cnname: '二级分类名称',
								open: false,
								child: [
									{
										cnname: '一组',
										open: false,
										child: [
											{
												cnname: '刘志远',
												checked: false,
											},
											{
												cnname: '张黔香',
												checked: false,
											},
											{
												cnname: '刘亚君',
												checked: false,
											},
											{
												cnname: '寇响',
												checked: false,
											},
											{
												cnname: '刘欢',
												checked: false,
											},
											{
												cnname: '黄俊',
												checked: false,
											},
											{
												cnname: '陈双双',
												checked: false,
											},
											{
												cnname: '管庆飞',
												checked: false,
											},
										],
									},
									{
										cnname: '二组',
										open: false,
										child: [],
									},
									{
										cnname: '三组',
										open: false,
										child: [],
									},
									{
										cnname: '四组',
										open: false,
										child: [],
									},
								],
							},
						],
					},
				],
			}
		},
		onLoad() {

		},
		methods: {
			showClassSelect(){
				this.showSelectClass = true;
			},
			classSelect(){
				this.showSelectClass = false;
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
	}
	.basis-lg {
		flex-basis: 100%;
	}
</style>
