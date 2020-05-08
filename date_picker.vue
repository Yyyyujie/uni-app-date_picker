<template>
		<view class="mask">
			<view class="handleBox">
				<view @tap="operation(1)">取消</view>
				<view @tap="operation(2)">确定</view>
			</view>
			<picker-view   :value="showDate | dateToIndex" @change="bindChange">
			    <picker-view-column>
			        <view class="item" v-for="(item,index) in years" :key="index">{{item}}年</view>
			    </picker-view-column>
			    <picker-view-column>
			        <view class="item" v-for="(item,index) in months" :key="index">{{item}}月</view>
			    </picker-view-column>
			    <picker-view-column>
			        <view class="item" v-for="(item,index) in days" :key="index">{{item}}日</view>
			    </picker-view-column>
			</picker-view>
		</view>
</template>

<script>
	let _this;
	export default{
		props:{
			showDate:{
				type:String,
				default:''
			}
		},
		data(){
			const date = new Date()
			    const years = []
			    const year = date.getFullYear();
			    const months = []
			    const month = date.getMonth() + 1
			    const days = []
			    const day = date.getDate()
					let starteYear = year - 60 ;
					let endDate = year - 16 ;
			    for (let i = starteYear; i <= endDate; i++) {
			        years.push(i)
			    }
			
			    for (let i = 1; i <= 12; i++) {
			        months.push(i)
			    }
			
			    for (let i = 1; i <= 31; i++) {
			        days.push(i)
			    }
			return {
						years,
						year,
						months,
						month,
						days,
						day
			}
		},
		beforeCreate: function () {
		     _this = this;
		 },
		filters:{
			dateToIndex(str){
				let indexArr= [0,0,0];   //value要下标
				if(str){
					let dateArr = str.split("-");
					indexArr[0] = _this.years.indexOf(Number(dateArr[0]));
					indexArr[1] = _this.months.indexOf(Number(dateArr[1]));
					indexArr[2] = _this.days.indexOf(Number(dateArr[2]));
				}
				return indexArr;
				
			}
		},
		methods:{
			bindChange (e) {
			    const val = e.detail.value
			    this.year = this.years[val[0]]
			    this.month = this.months[val[1]]>10?this.months[val[1]]:'0'+this.months[val[1]]
			    this.day = this.days[val[2]]>10?this.days[val[2]]:'0'+this.days[val[2]]
			},
			operation(type){
				if(type==1){
					//取消选择关闭弹框
					this.$emit("dateEvent",false)
				}else{
					//选择时间
					this.$emit("dateEvent",{selectedDate:this.year+'-'+this.month+'-'+this.day})
				}
			}
		}
	}
</script>

<style scoped>
	.mask{
		position: fixed;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		background-color: rgba(0,0,0,0.4);
		z-index: 99999;
	}
	.handleBox{
		width: 100%;
		height: 80rpx;
		background-color: #fff;
		position: fixed;
		top: calc(100% - 600rpx - 80rpx);
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 30upx;
		box-sizing: border-box;
		font-size: 30upx;
		border-bottom: 1upx solid #f5f5f5;
	}
	.handleBox>view:last-child{
		color: #FC0F4A;
	}
	picker-view {
	    width: 100%;
	    height: 600rpx;
			z-index: 9999;
			top: calc(100% - 600rpx);
			background-color: #fff;
			position: fixed;
	}
	
	.item {
	    line-height: 68upx;
	    text-align: center;
			color: #000;
	}
</style>
