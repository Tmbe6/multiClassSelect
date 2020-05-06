<template>
	<view class="select" v-if="isShow">
		<view class="cu-bar bg-white">
			<view class="action" @click.stop="clickBack">
				<text class="cuIcon-back text-gray"></text>选择分类
			</view>
			<view class="content text-bold"></view>
			<view class="text-blue action bar-left" @click.stop="submit">完成</view>
		</view>
		<view class="select-list" v-for="(item,index) in selectList" :key="index">
			<view class="cu-form-group" @click="openSelect(index)">
				<view class="select-list-le">
					<image src="/static/classificat.png"></image>
					<text>{{item.cnname}}</text>
				</view>
				<view class="title">
					<text :class="[item.open == true ? 'cuIcon-unfold' : 'cuIcon-back']"></text>
				</view>
			</view>
			<!-- 二级分类 -->
			<view class="select-list-content" v-if="item.open && level== 2">
				<uni-transition :duration="500" :mode-class="['fade','slide-left']" :show="true">
					<view class="select-list-txt">
						<view class="select-list-item" v-for="(item1,index1) in item.child" :key="index1" @click.stop="selcetItemTwo(index,index1)">
							<text>{{item1.cnname}}</text>
							<text class="cuIcon-check text-blue" style="font-weight: bold;" v-if="item1.checked == true"></text>
						</view>
					</view>
				</uni-transition>
			</view>
			<!-- 三级分类 -->
			<view class="select-list-content" v-if="item.open  && level== 3">
				<uni-transition :duration="500" :mode-class="['fade','slide-left']" :show="true">
					<view class="select-list" v-for="(item1,index1) in item.child" :key="index1" @click.stop="openSelect1(index,index1)">
						<view class="cu-form-group">
							<view class="select-list-le" style="padding-left: 20rpx;">
								<text>{{item1.cnname}}</text>
							</view>
							<view class="title">
								<text :class="[item1.open == true ? 'cuIcon-unfold' : 'cuIcon-back']"></text>
							</view>
						</view>
						<view class="select-list-content" v-if="item1.open">
							<uni-transition :duration="500" :mode-class="['fade','slide-left']" :show="true">
								<view class="select-list-txt">
									<view class="select-list-item" v-for="(item2,index2) in item1.child" :key="index2" @click.stop="selcetItemThree(index,index1,index2)">
										<text>{{item2.cnname}}</text>
										<text class="cuIcon-check text-blue" style="font-weight: bold;" v-if="item2.checked == true"></text>
									</view>
								</view>
							</uni-transition>
						</view>
					</view>
				</uni-transition>
			</view>
			<!-- 四级分类 -->
			<view class="select-list-content" v-if="item.open  && level== 4">
				<uni-transition :duration="500" :mode-class="['fade','slide-left']" :show="true">
					<view class="select-list" v-for="(item1,index1) in item.child" :key="index1" @click.stop="openSelect1(index,index1)">
						<view class="cu-form-group">
							<view class="select-list-le" style="padding-left: 20rpx;">
								<text>{{item1.cnname}}</text>
							</view>
							<view class="title">
								<text :class="[item1.open == true ? 'cuIcon-unfold' : 'cuIcon-back']"></text>
							</view>
						</view>
						<view class="select-list-content" v-if="item1.open">
							<uni-transition :duration="500" :mode-class="['fade','slide-left']" :show="true">
								<view class="select-list" v-for="(item2,index2) in item1.child" :key="index2" @click.stop="openSelect2(index,index1,index2)">
									<view class="cu-form-group">
										<view class="select-list-le" style="padding-left: 40rpx;">
											<text>{{item2.cnname}}</text>
										</view>
										<view class="title">
											<text :class="[item2.open == true ? 'cuIcon-unfold' : 'cuIcon-back']"></text>
										</view>
									</view>
									<view class="select-list-content" v-if="item2.open">
										<uni-transition :duration="500" :mode-class="['fade','slide-left']" :show="true">
											<view class="select-list-txt">
												<view class="select-list-item" v-for="(item3,index3) in item2.child" :key="index3" @click.stop="selcetItemFour(index,index1,index2,index3)">
													<text>{{item3.cnname}}</text>
													<text class="cuIcon-check text-blue" style="font-weight: bold;" v-if="item3.checked == true"></text>
												</view>
											</view>
										</uni-transition>
									</view>
								</view>
							</uni-transition>
						</view>
					</view>
				</uni-transition>
			</view>
		</view>
	</view>
</template>


<script>
	export default {
		props: {
			// 分类数据
			selectList: {
				type: Array,
				default(){
					return []
				}
			},
			// 分类级数
			level: {
				type: Number,
				default(){
					return 2
				}
			}
		},
		data() {
			return {
				isShow: true,
				selectName: ''
			};
		},
		created() {
			
		},
		computed:{
			
		},
		methods:{
			// 完成
			submit(){
				this.$emit('classSelect',this.selectName);
			},
			// 返回
			clickBack(){
				this.submit();
			},
			// 打开菜单
			openSelect(index){
				this.isShow = false;
				this.selectList[index].open = !this.selectList[index].open;
				this.selectList.forEach((item,id) => {
					if(id != index){
						item.open = false;
					}
				})
				this.isShow = true;
			},
			// 二级菜单
			openSelect1(index,index1){
				// console.log(index,index1)
				this.isShow = false;
				this.selectList[index].child[index1].open = !this.selectList[index].child[index1].open;
				this.selectList[index].child.forEach((item,id) => {
					if(id != index1){
						item.open = false;
					}
				})
				this.isShow = true;
			},
			// 三级菜单
			openSelect2(index,index1,index2){
				this.isShow = false;
				this.selectList[index].child[index1].child[index2].open = !this.selectList[index].child[index1].child[index2].open;
				this.selectList[index].child[index1].child.forEach((item,id) => {
					if(id != index2){
						item.open = false;
					}
				})
				this.isShow = true;
			},
			// 选择二级菜单
			selcetItemTwo(index,index1){
				this.isShow = false;
				this.selectList[index].child[index1].checked = !this.selectList[index].child[index1].checked;
				this.selectList[index].child.forEach((item,id) => {
					if(id != index1){
						item.checked = false;
					}
				})
				if(this.selectList[index].child[index1].checked == true){
					// this.$emit('selectName',this.selectList[index].child[index1].cnname);
					this.selectName = this.selectList[index].child[index1].cnname;
				}
				// console.log(this.selectList[index].child[index1].cnname);
				this.isShow = true;
			},
			// 选择三级菜单
			selcetItemThree(index,index1,index2){
				this.isShow = false;
				this.selectList[index].child[index1].child[index2].checked = !this.selectList[index].child[index1].child[index2].checked;
				this.selectList[index].child[index1].child.forEach((item,id) => {
					if(id != index2){
						item.checked = false;
					}
				})
				if(this.selectList[index].child[index1].child[index2].checked == true){
					// this.$emit('selectName',this.selectList[index].child[index1].child[index2].cnname);
					this.selectName = this.selectList[index].child[index1].child[index2].cnname;
				}
				// console.log(this.selectList[index].child[index1].child[index2].cnname);
				this.isShow = true;
			},
			// 选择四级菜单
			selcetItemFour(index,index1,index2,index3){
				this.isShow = false;
				this.selectList[index].child[index1].child[index2].child[index3].checked = !this.selectList[index].child[index1].child[index2].child[index3].checked;
				this.selectList[index].child[index1].child[index2].child.forEach((item,id) => {
					if(id != index3){
						item.checked = false;
					}
				})
				if(this.selectList[index].child[index1].child[index2].child[index3].checked == true){
					this.selectName = this.selectList[index].child[index1].child[index2].child[index3].cnname;
					// this.$emit('selectName',this.selectList[index].child[index1].child[index2].child[index3].cnname);
				}
				// console.log(this.selectList[index].child[index1].child[index2].child[index3].cnname);
				this.isShow = true;
			},
		}
		
	}
</script>

<style lang="scss" scoped>
	.select{
		width: 100%;
		height: 100vh;
		.bar-left{
			font-size: 30rpx;
			font-weight: bold;
		}
		.cu-form-group{
			width: 750rpx;
			border-bottom: 1rpx solid #EEEEEE;
			.select-list-le{
				display: flex;
				flex-direction: row;
				align-items: center;
				image{
					width: 30rpx;
					height: 30rpx;
				}
				text{
					margin-left: 15rpx;
				}
			}
			.title{
				padding-right: 0;
			}
		}
		.select-list{
			margin-top: 10rpx;
			.select-list-content{
				width: 100%;
				min-height: 200rpx;
				padding-bottom: 20rpx;
				background-color: #fff;
				display: flex;
				.select-list-txt{
					display: flex;
					flex-direction: row;
					flex-wrap: wrap;
					.select-list-item{
						width: 315rpx;
						height: 68rpx;
						font-size: 24rpx;
						background-color: #f1f1f1;
						display: flex;
						justify-content: space-between;
						padding-left: 15rpx;
						padding-right: 15rpx;
						line-height: 68rpx;
						text-align: center;
						margin-left: 40rpx;
						margin-top: 15rpx;
						text{
							padding-left: 10rpx;
						}
					}
				}
			}
		}
	}
</style>
