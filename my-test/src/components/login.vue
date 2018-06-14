<style>
	.login-wrap{
		text-align: center;
	}
	.register-wrap{text-align:center;}
	input{
		display: block;
		width: 250px;
		height: 40px;
		line-height: 40px;
		outline:none;
		margin: 0 auto;
		margin-bottom: 10px;
		padding:10px;
		border:1px solid #888;
		box-sizing:border-box;
	}
	p{
		color: red;
	}
	button{
		display: block;
		width: 250px;
		height: 40px;
		line-height: 40px;
		margin: 0 auto;
		background-color: #41b883;
		color: #fff;
		border:none;
		font-size: 16px;
		margin-bottom: 5px;
	}
	span{
		cursor:pointer;
	}
	span:hover {
		color: #41b883;
	}
</style>
<template>
	<div class="login">
		<div class="login-wrap" v-show="showLogin">
			<h3>登录</h3>
			<p v-show="showTishi">{{tishi}}</p>
			<input type="text" placeholder="请输入用户名" v-model="username">
			<input type="password" placeholder="请输入密码" v-model="password">
			<button v-on:click="login" >登录</button>
			<span v-on:click="ToRegister" >没有账号？马上注册</span>
		</div>

		<div class="register-wrap" v-show="showRegister">
			<h3>注册</h3>
			<p v-show="showTishi">{{tishi}}</p>
			<input type="text" placeholder="请输入用户名" v-model="newUsername">
			<input type="password" placeholder="请输入密码" v-model="newPassword">
			<button v-on:click="register" >注册</button>
			<span v-on:click="Tologin">已有注册？马上登录</span>
		</div>
	</div>
</template>
<script>
	import {setCookie,getCookie} from '@/assets/js/cookie.js'
	export default  {
		data() {
			return{
				showLogin: true,
				showRegister: false,
				showTishi:false,
				tishi:'',
				username:'',
				password:'',
				newUsername:'',
				newPassword:'',
			}
		},
		methods: {
			Tologin() {
				this.showLogin= true;
				this.showRegister= false;
			},
			ToRegister() {
				this.showLogin = false;
				this.showRegister =true;
			},
			login() {
				if(this.usename == "" || this.password == "" ) {
					alert("请输入用户名和密码")
				} else {
					let data ={
					'username': this.usename,
					'password':this.password,
					}
					if(data.usename==="admin" && data.password==="123456") {
						this.tishi="登录成功";
						this.showTishi= true;
						setCookie('username',this.usename,1000 * 60)
						setTimeout(function() {
							this.$router.push('/home')
						}.bind(this), 1000)
					} else {
						this.tishi="登录失败";
						this.showTishi= true;
						setTimeout(function() {
							this.tishi="";
							this.showTishi= false;
						}.bind(this), 1000)
					}
				}
			},
			register() {
				if(this.newUsername == "" || this.newPassword == "" ) {
					alert("请输入用户名和密码")
				} else {
					let data ={
					'username': this.newUsername,
					'password':this.newPassword,
					}
					this.tishi="注册成功";
					this.showTishi= true;
					this.usename="";
					this.password="";
					setTimeout(function() {
						this.showRegister= false;
						this.showTishi=false;
						this.showLogin=true;
					}.bind(this), 1000)
				}
			},
		},
		mounted(){
	      /*页面挂载获取cookie，如果存在username的cookie，则跳转到主页，不需登录*/
	      if(getCookie('username')){
	        this.$router.push('/home')
	      }
	    }
	}
</script>
