<template>
	<view class="u-page">
		<!-- 页面的Title -->
		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="15">
				<view style="background:linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186));">
					<u-icon label="访客预约登记  新晟半导体" labelColor="#ffffff" name="level" color="#ffffff" size="50"></u-icon>
				</view>
			</u-col>
		</u-row>

		<!-- 微信的昵称 -->
		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="layout">
					<u-icon label="微信昵称" labelColor="#ffffff" name="weixin-fill" color="#ffffff" size="30"></u-icon>
					<view class="field_layout"><u--input :value="fk_nickname" disabled></u--input></view>
				</view>
			</u-col>
		</u-row>

		<!-- 姓名  -->
		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="layout">
					<u-icon label="访客姓名" labelColor="#ffffff" name="list" color="#ffffff" size="30"></u-icon>
					<view class="field_layout">
						<u--input :value="fk_realname" placeholder="请输入您的姓名" ></u--input>
						</view>
				</view>
			</u-col>
		</u-row>

		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="layout">
					<u-icon label="  身份证" labelColor="#ffffff" name="eye" color="#ffffff" size="30"></u-icon>
					<view class="field_layout">
						<u--input type="idcard" :value="fk_authcard" placeholder="请输入你的身份证"></u--input>
					</view>
				</view>
			</u-col>
		</u-row>

		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="layout">
					<u-icon label="  联系电话" labelColor="#ffffff" name="phone" color="#ffffff" size="30"></u-icon>
					<view class="field_layout">
						<u--input :value="fk_phone" placeholder="请输入您的移动电话" maxlength="11"></u--input>
					</view>
				</view>
			</u-col>
		</u-row>

		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="fk_type_layout">
					<u-icon label="  访客时间" labelColor="#ffffff" name="clock" color="#ffffff" size="30"></u-icon>
					<view class="field_layout">
						<u--input :value="fk_time_retval" disabled></u--input>
						<u-button text="选择来访时间" @click="fk_time_trigger" color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))"></u-button>
						<u-calendar :show="fk_time_showing" mode="single" @confirm="fk_time_done" @close="fk_time_showing = false" title="请选择到访时间"></u-calendar>
					</view>
				</view>
			</u-col>
		</u-row>
		
		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="fk_type_layout">
					<u-icon label="  来访身份 [普通访客/施工人员]" labelColor="#ffffff" name="account-fill" color="#ffffff" size="30"></u-icon>
					<view class="field_layout">
							<u--input :value="fk_type_retval" disabled></u--input>
							<u-button text="选择访客身份" @click="fk_type_trigger" color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))"></u-button>
							<u-picker id="fk_type":show="fk_type_showing" :columns="fk_type_list" 
							@confirm="fk_type_confirm" @close="fk_type_showing=false" @cancel="fk_type_showing=false"></u-picker>
					</view>
				</view>
			</u-col>
		</u-row>
		
		
		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="12">
				<view class="fk_type_layout">
					<u-icon label=" 访客部门" labelColor="#ffffff" name="share-square" color="#ffffff" size="30"></u-icon>
					<view class="field_layout">
							<u--input :value="fk_dept_retval" disabled></u--input>
							<u-button text="选择访客部门" @click="fk_dept_trigger" color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))"></u-button>
							<u-picker id="fk_dept":show="fk_dept_showing" :columns="fk_dept_list" 
								@confirm="fk_dept_confirm" @close="fk_dept_showing=false" @cancel="fk_dept_showing=false"></u-picker>
					</view>
				</view>
			</u-col>
		</u-row>
		
		<u-row justify="space-between" gutter="10" style="display: flex;" customStyle="margin-bottom: 10px">
			<u-col textAlign="left" span="6">
				<u-button id="btnSubmit" text="提交" color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))"></u-button>
			</u-col>
			
			<u-col textAlign="left" span="6">
				<u-button id="btnCancel" text="重置" @click="fk_field_reset" color="linear-gradient(to right, rgb(66, 83, 216), rgb(213, 51, 186))"></u-button>
			</u-col>
			
		</u-row>
		
	</view>
</template>

<script>
	
	const d = new Date()
	const year = d.getFullYear()
	let month = d.getMonth()
	month = month < 10 ? `0${month}` : month
	const date = d.getDate()
	const time_reval = year.toString() + '-' + month.toString() + '-' + date.toString()
export default {
	data() {
		return {
			wxUserInfo: '您好，用户',
			wx_avatar: '',
			fk_nickname: 'VJStorm',
			fk_realname: 'Admin',
			fk_authcard:'310105192209183254',
			fk_phone: '110',
			
			
			fk_time_showing: false,
			fk_time_retval: '',
			
			fk_type_showing: false,
			fk_type_list: [['普通访客', '施工人员']],
			fk_type_retval: '普通访客',
			
			fk_dept_showing: false,
			fk_dept_list: [['厂务', '人事', '财务', '工程', '研发' , '其它']],
			fk_dept_retval: '厂务',
			
			wx_jscode: '', //获取微信的登录Code
			myAppID: 'wxe19ab92e3196a501',
			mySecret: '6a7929be17e2478372d9982dcc0235a0',
			openid: '',
			unionid: '',
			sesstion_key: ','
		};
	},
	created() {
		// this.wxLoginToast();
	},
	onLoad() {
		this.fk_init()
	},
	methods: {
		empty_function() {},
		
		fk_time_trigger() {
			this.fk_time_showing = true
		},
		
		// 访客时间确认后触发
		fk_time_done(e) {
			this.fk_time_retval = e
			this.fk_time_showing = false
		},
		
		//通过触发Input的focus时间，激活访客类型选择器 UView Picker
		fk_type_trigger(e) {
				this.fk_type_showing = true
		},
		
		fk_type_confirm(e) {
				// console.log(e)
				this.fk_type_retval = e.value[0]
				this.fk_type_showing = false
		},
		
		fk_dept_trigger() {
				this.fk_dept_showing = true
		},
		
		fk_dept_confirm(e) {
			 this.fk_dept_retval = e.value[0]
			 this.fk_dept_showing = false
		},
		
		fk_init() {
			// Get current datatime
			const year = new Date().getFullYear()
			const month = new Date().getMonth() + 1
			const day = new Date().getDate()
			this.fk_time_retval = year + '-' + month + '-' + day
			
			uni.login({
				success: res => {
					this.wx_jscode = res.code;
					console.log(res);
					uni.request({
						url: 'https://api.weixin.qq.com/sns/jscode2session',
						method: 'GET',
						data: {
							appid: this.myAppID,
							secret: this.mySecret,
							js_code: this.wx_jscode,
							grant_type: 'authorization_code'
						},
						success: cts => {
							this.openid = cts.data.openid;
							this.unionid = cts.data.unionid;
							this.sesstion_key = cts.data.sesstion_key;
							console.log(cts);
						},
						fail: cts => {
							console.error(cts);
						}
					});
				}
			});
		},
		
		fk_field_reset() {
			this.fk_nickname = ''
			this.fk_realname = ''
			this.fk_authcard =''
			this.fk_phone = ''
			this.fk_time_retval= ''
			this.fk_type_retval= ''
			this.fk_dept_retval= ''
		},
		
		wxGetUserProfile() {
			wx.getUserProfile({
				desc: '获取你的昵称，头像，性别',
				lang: 'zh_CN',
				success: res => {
					console.debug(res);
					// console.debug(res.userInfo.nickname)
					this.nickname = '微信昵称:    ' + res.userInfo.nickName;
					this.wx_avatar = res.userInfo.avatarUrl;
				},
				fail: res => {
					console.error(res);
				}
			});
		},
		wxLoginToast() {
			uni.showModal({
				title: '授权',
				content: '微信授权',
				showCancel: false,
				success: res => {
					this.wxGetUserProfile();
				}
			});
		}
	}
};
</script>

<style lang="scss">
.wrap {
	padding: 12px;
}

.layout {
	height: 80px;
	border-radius: 5px;
	margin-left: 20px;
	margin-right: 20px;
	background: #99a9bf;
}

.field_layout {
	border-radius: 5px;
	margin-left: 30px;
	margin-right: 30px;
	background: #f4f4f5;
}

.fk_type_layout {
	height: 120px;
	border-radius: 5px;
	margin-left: 20px;
	margin-right: 20px;
	background: #99a9bf;
}

.bg-purple {
	background: #ced7e1;
}

.bg-purple-dark {
	background: #99a9bf;
}
</style>
