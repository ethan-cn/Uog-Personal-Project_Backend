<template>
	<!-- <div class="login-container"> -->
	<div class="login-background">
		<!-- <img src="../../assets/imgs/background-user.jpg" width="100%" height="100%" alt=""> -->
	</div>

	<div id="login-form">
		<!-- 欢迎登录标头 -->
		<!-- <div
			style="position:absolute;top:5px;left:100px;font-weight:bolder;color:#606266"
		>
			Welcome Musical-recommendation Ecosystem
		</div> -->
		<!-- <div>Log in </div> -->
		<el-form
			:model="ruleForm"
			status-icon
			:rules="rules"
			ref="ruleForm"
			label-width="100px"
			class="demo-ruleForm"
		>
			<el-form-item
				class="font-color-form-label"
				label="User name:"
				prop="username"
			>
				<el-input
					type="name"
					v-model="ruleForm.username"
					autocomplete="off"
					placeholder="Please enter the user id: "
					clearable
				></el-input>
			</el-form-item>
			<el-form-item
				class="font-color-form-label"
				label="Password"
				prop="password"
			>
				<el-input
					type="password"
					v-model="ruleForm.password"
					autocomplete="off"
					placeholder="Please enter the password: "
					clearable
				></el-input>
			</el-form-item>
			<el-form-item
				style="padding-top:25px;margin-bottom:0;margin-right:0;margin-left:-100px;"
			>
				<el-button type="primary" @click="loginSubmitForm('ruleForm')">
					Log in
				</el-button>
				<el-button
					style="margin-left:20px;"
					@click="registerSubmitForm"
				>
					Sign in
				</el-button>
			</el-form-item>
		</el-form>
	</div>
	<el-dialog
		class="register-dialog"
		title="Sign in"
		modal="false"
		v-model="registerDialogVisibleEdit"
	>
		<el-form :model="form">
			<el-form-item label="Name" :label-width="formLabelWidth">
				<el-input
					clearable
					v-model="form.name"
					autocomplete="off"
					placeholder="Please enter the name:"
				></el-input>
			</el-form-item>
			<el-form-item
				class="gender-item"
				label="Gender"
				:label-width="formLabelWidth"
			>
				<el-select
					v-model="form.gender"
					placeholder="Please choose your gender:"
					style="width:600px;"
				>
					<el-option label="Male" value="Male"></el-option>
					<el-option label="Female" value="Female"></el-option>
				</el-select>
			</el-form-item>
			<el-form-item label="Age" :label-width="formLabelWidth">
				<el-input
					clearable
					v-model="form.age"
					autocomplete="off"
					placeholder="Please enter the age:"
				></el-input>
			</el-form-item>
			<el-form-item label="Your city" :label-width="formLabelWidth">
				<el-cascader
					v-model="value"
					:options="areaOptions"
					:props="props"
					@change="handleChange"
					style="width:600px;"
					placeholder="Where are you from?"
				></el-cascader>
			</el-form-item>
			<el-form-item label="Bio" :label-width="formLabelWidth">
				<el-input
					clearable
					v-model="form.des"
					autocomplete="off"
					placeholder="Tell us about yourself:"
				></el-input>
			</el-form-item>
			<el-form-item label="Password" :label-width="formLabelWidth">
				<el-input
					clearable
					v-model="form.password"
					autocomplete="off"
					placeholder="Please enter the password:"
				></el-input>
			</el-form-item>
		</el-form>
		<template #footer>
			<span class="dialog-footer">
				<el-button @click="registerDialogVisibleEdit = false">
					Cancel
				</el-button>
				<el-button type="primary" @click="register">
					Submit
				</el-button>
			</span>
		</template>
	</el-dialog>
	<!-- </div> -->
</template>

<script>
import { getLogin, _register } from "../../network/request.js"
import { ref } from "vue"
// import {
// 	validateUsername,
// 	validatePassword,
// 	validatePhone,
// 	validateRealName,
// 	validateIdNumber,
// } from "../../common/utils.js"
export default {
	name: "login",
	data() {
		// Verify data form，见Element-Plus
		const validateUsername = (rule, value, callback) => {
			if (value === "") {
				callback(new Error("Please enter the username"))
			} else {
				if (this.ruleForm.username !== "") {
					this.$refs.ruleForm.validateField("username")
				}
				callback()
			}
		}
		const validatePass = (rule, value, callback) => {
			if (value === "") {
				callback(new Error("Please enter the password"))
			} else {
				callback()
			}
		}
		return {
			// User info form
			form: {
				// *User form
				// user uid
				uid: ref("uid"),
				// username
				name: ref(""),
				gender: ref(""),
				age: ref(""),
				area: ref(""),
				registerTime: ref(""),
				des: ref(""),
				password: ref(""),
				// User form*
			},
			// Pop-up message _sign in
			registerDialogVisibleEdit: false,
			// Width of box
			formLabelWidth: "120px",
			// Form data
			ruleForm: {
				username: "",
				password: "",
			},
			// Rules of verify form，见Element-Plus
			rules: {
				username: [{ validator: validateUsername, trigger: "blur" }],
				password: [{ validator: validatePass, trigger: "blur" }],
			},
			// Properties of the locale selector
			props: { expandTrigger: "hover" },
			// Choose area value
			value: [],
			// Choose the area
			areaOptions: [
				{
					value: "北京市",
					label: "北京市",
					children: [
						{
							value: "北京城区",
							label: "北京城区",
						},
					],
				},
				{
					value: "天津市",
					label: "天津市",
					children: [
						{
							value: "天津城区",
							label: "天津城区",
						},
					],
				},
				{
					value: "上海市",
					label: "上海市",
					children: [
						{
							value: "上海城区",
							label: "上海城区",
						},
					],
				},
				{
					value: "重庆市",
					label: "重庆市",
					children: [
						{
							value: "重庆城区",
							label: "重庆城区",
						},
					],
				},
				{
					value: "河南省",
					label: "河南省",
					children: [
						{
							value: "郑州市",
							label: "郑州市",
						},
						{
							value: "周口市",
							label: "周口市",
						},
						{
							value: "洛阳市",
							label: "洛阳市",
						},
						{
							value: "开封市",
							label: "开封市",
						},
						{
							value: "新乡市",
							label: "新乡市",
						},
						{
							value: "南阳市",
							label: "南阳市",
						},
						{
							value: "信阳市",
							label: "信阳市",
						},
						{
							value: "安阳市",
							label: "安阳市",
						},
						{
							value: "漯河市",
							label: "漯河市",
						},
						{
							value: "平顶山市",
							label: "平顶山市",
						},
						{
							value: "鹤壁市",
							label: "鹤壁市",
						},
						{
							value: "焦作市",
							label: "焦作市",
						},
						{
							value: "许昌市",
							label: "许昌市",
						},
						{
							value: "三门峡市",
							label: "三门峡市",
						},
						{
							value: "商丘市",
							label: "商丘市",
						},
						{
							value: "济源市",
							label: "济源市",
						},
					],
				},
				{
					value: "湖北省",
					label: "湖北省",
					children: [
						{
							value: "武汉市",
							label: "武汉市",
						},
						{
							value: "黄石市",
							label: "黄石市",
						},
						{
							value: "孝感市",
							label: "孝感市",
						},
						{
							value: "襄阳市",
							label: "襄阳市",
						},
						{
							value: "十堰市",
							label: "十堰市",
						},
						{
							value: "宜昌市",
							label: "宜昌市",
						},
						{
							value: "荆门市",
							label: "荆门市",
						},
						{
							value: "仙桃市",
							label: "仙桃市",
						},
						{
							value: "荆州市",
							label: "荆州市",
						},
						{
							value: "黄冈市",
							label: "黄冈市",
						},
						{
							value: "咸宁市",
							label: "咸宁市",
						},
						{
							value: "恩施土家族苗族自治州",
							label: "恩施土家族苗族自治州",
						},
						{
							value: "随州市",
							label: "随州市",
						},
						{
							value: "潜江市",
							label: "潜江市",
						},
						{
							value: "天门市",
							label: "天门市",
						},
						{
							value: "神农架林区",
							label: "神农架林区",
						},
					],
				},
				{
					value: "山西省",
					label: "山西省",
					children: [
						{
							value: "太原市",
							label: "太原市",
						},
						{
							value: "大同市",
							label: "大同市",
						},
						{
							value: "阳泉市",
							label: "阳泉市",
						},
						{
							value: "晋城市",
							label: "晋城市",
						},
						{
							value: "长治市",
							label: "长治市",
						},
						{
							value: "晋中市",
							label: "晋中市",
						},
						{
							value: "朔州市",
							label: "朔州市",
						},
						{
							value: "忻州市",
							label: "忻州市",
						},
						{
							value: "运城市",
							label: "运城市",
						},
						{
							value: "临汾市",
							label: "临汾市",
						},
						{
							value: "吕梁市",
							label: "吕梁市",
						},
					],
				},
				{
					value: "四川省",
					label: "四川省",
					children: [
						{
							value: "成都市",
							label: "成都市",
						},
						{
							value: "自贡市",
							label: "自贡市",
						},
						{
							value: "攀枝花市",
							label: "攀枝花市",
						},
						{
							value: "泸州市",
							label: "泸州市",
						},
						{
							value: "德阳市",
							label: "德阳市",
						},
						{
							value: "绵阳市",
							label: "绵阳市",
						},
						{
							value: "乐山市",
							label: "乐山市",
						},
						{
							value: "广元市",
							label: "广元市",
						},
						{
							value: "遂宁市",
							label: "遂宁市",
						},
						{
							value: "内江市",
							label: "内江市",
						},
					],
				},
				{
					value: "广东省",
					label: "广东省",
					children: [
						{
							value: "广州市",
							label: "广州市",
						},
						{
							value: "深圳市",
							label: "深圳市",
						},
						{
							value: "韶关市",
							label: "韶关市",
						},
						{
							value: "珠海市",
							label: "珠海市",
						},
						{
							value: "汕头市",
							label: "汕头市",
						},
						{
							value: "佛山市",
							label: "佛山市",
						},
						{
							value: "东莞市",
							label: "东莞市",
						},
					],
				},
				{
					value: "安徽省",
					label: "安徽省",
					children: [
						{
							value: "合肥市",
							label: "合肥市",
						},
						{
							value: "阜阳市",
							label: "阜阳市",
						},
						{
							value: "芜湖市",
							label: "芜湖市",
						},
						{
							value: "蚌埠市",
							label: "蚌埠市",
						},
						{
							value: "淮南市",
							label: "淮南市",
						},
						{
							value: "黄山市",
							label: "黄山市",
						},
						{
							value: "滁州市",
							label: "滁州市",
						},
					],
				},
				{
					value: "湖南省",
					label: "湖南省",
					children: [
						{
							value: "长沙市",
							label: "长沙市",
						},
						{
							value: "株洲市",
							label: "株洲市",
						},
						{
							value: "湘潭市",
							label: "湘潭市",
						},
						{
							value: "岳阳市",
							label: "岳阳市",
						},
						{
							value: "衡阳市",
							label: "衡阳市",
						},
						{
							value: "邵阳市",
							label: "邵阳市",
						},
						{
							value: "常德市",
							label: "常德市",
						},
					],
				},
				{
					value: "辽宁省",
					label: "辽宁省",
					children: [
						{
							value: "沈阳市",
							label: "沈阳市",
						},
						{
							value: "大连市",
							label: "大连市",
						},
						{
							value: "本溪市",
							label: "本溪市",
						},
						{
							value: "丹东市",
							label: "丹东市",
						},
						{
							value: "锦州市",
							label: "锦州市",
						},
						{
							value: "辽阳市",
							label: "辽阳市",
						},
						{
							value: "营口市",
							label: "营口市",
						},
					],
				},
			],
		}
	},
	methods: {
		// Clear the data in the pop-up box
		clearForm() {
			this.form.name = ""
			this.form.gender = ""
			this.form.age = ""
			this.form.area = ""
			this.form.registerTime = ""
			this.form.password = ""
			this.form.des = ""
		},
		// Resetting form contents
		resetForm(formName) {
			this.$refs[formName].resetFields()
		},
		// Login
		login() {
			getLogin({
				uid: this.ruleForm.username,
				password: this.ruleForm.password,
			}).then((response) => {
				console.log(response.data)
				const userData = response.data.data
				// console.log(userData.password)
				if (response.data.code == "200") {
					userData.password = this.ruleForm.password
					console.log(userData)
					// when the user login save the data to vuex，When the exit use remove
					this.$store.commit("SET_USER", userData)

					// console.log(validateUsername())
					this.$router.push({
						path: "/discover",
					})
					this.$message({
						type: "success",
						message: userData.name + "  Welcome!",
					})
				} else {
					// 重置表单
					this.resetForm("ruleForm")
					// 弹出提示
					this.$message({
						type: "info",
						message: "User name or password is incorrect, login failed!",
					})
				}
			})
		},
		// 提交表单的事件处理
		loginSubmitForm(formName) {
			this.$refs[formName].validate((valid) => {
				if (valid) {
					// callback(new Error('用户名或密码错误'));
					// alert("submit!");
					this.login()
				} else {
					console.log("error submit!!")
					// callback(new Error('用户名或密码错误'));
					return false
				}
			})
		},
		// 用户注册
		register() {
			_register(this.form).then((response) => {
				const data = response.data.data
				const code = response.data.code
				const msg = response.data.msg
				console.log(data)
				console.log(data.uid)
				if (code == 200) {
					this.$alert("Please use this account to log in:" + data.uid, "Registration", {
						confirmButtonText: "Confirm",
						callback: () => {
							this.$message({
								type: "success",
								message: "Success!",
							})
						},
					})
					this.registerDialogVisibleEdit = false
					this.clearForm()
					this.$router.push({
						path: "/login",
						query: {
							uid: data.uid,
						},
					})
				} else {
					this.$message({
						type: "info",
						message: msg,
					})
				}
			})
		},
		// 提交注册信息按钮监听
		registerSubmitForm() {
			this.registerDialogVisibleEdit = true
			this.clearForm()
		},
		// 地区选择器选择
		handleChange(value) {
			console.log(value)
			this.form.area = value[0] + "-" + value[1]
		},
	},
}
</script>

<style scoped>
/* // scoped表示旨在本文件内起作用，局部声明
//@import url(); 引入公共css类 */

#login-form {
	z-index: 1;
	position: absolute;
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	margin: auto;
	width: 400px;
	height: 300px;
	background-color: rgba(204, 204, 204, 0.6);
	border-radius: 10px;
}

#login-form .el-form {
	position: absolute;
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	margin: auto;
	height: 200px;
	width: 400px;
}
#login-form .el-form .el-form-item {
	margin-right: 30px;
}
#login-form .el-form .el-form-item .el-form-item__content {
	margin-left: 0 !important;
}

/* 尽在登录界面显示背景 */
.login-background {
	background: url("../../assets/imgs/background-user.jpg") no-repeat center
		center;
	background-size: cover;
	background-attachment: fixed;
	background-color: #cccccc;
	z-index: -1;
	position: absolute;
	width: 100%;
	height: 100%;
}

.register-dialog >>> .gender-item >>> div.el-form-item__content {
	text-align: left;
}

/* 设置 .el-form-item 标签的label的颜色*/
.font-color-form-label .el-form-item__label {
	color: #303133;
}
</style>

<style>
.register-dialog >>> .gender-item >>> div.el-form-item__content {
	text-align: left;
}
</style>
