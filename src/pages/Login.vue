<template>
	<div id="login">
		<div class="avatar">
			<div class="circel">
				<img :src="s" alt="">
			</div>
		</div>
		<div class="inputs">
			<mt-field label="用户名" placeholder="请输入用户名" v-model="username"></mt-field>
			<mt-field label="密码" placeholder="请输入密码" type="password" v-model="password"></mt-field>
			<mt-button type="default" size="large" @click.native="login">登录</mt-button>
		</div>
	</div>
</template>

<script>
	import r from '../assets/joo.png';
	import { mapMutations } from 'vuex';
	export default {
		name: 'login',
		data() {
			return {
				s: '',
				username: '',
				password: '',
				redirect: ''
			};
		},
		created: function () {
			this.s = r;
			this.redirect = this.$route.query.redirect;
		},
		methods: {
			...mapMutations(['setToken']),
			login: function() {
				let data = {
					username: this.username,
					password: this.password
				};
				this.post(data);
			},
			post: function(data) {
				this.$axios.post('/api/login', data).then(result => {
					if (result.status == 1) {
						this.$toast('10s登录有效期');
						this.setToken(result);
						if (this.redirect) {
							this.$router.push(this.redirect);
						} else {
							this.$router.push('/desk/public');
						}
					}
				}).catch(error => {

				});
			}
		}
	};

</script>

<style lang="less" scoped>
	#login {
		.avatar {
			padding-top: 20%;
			text-align: center;

			.circel {
				display: inline-block;
				width: 100px;
				height: 100px;

				img {
					width: 100%;
					height: 100%;
					border-radius: 50%;
				}
			}
		}

		.inputs {
			margin-top: 20px;
			padding: 0 10px;
		}
	}

</style>
