<script setup>
import { useMenu } from "../hooks/useMenu";
import { vOnClickOutside } from '@vueuse/components';
const { handleMenu, closeModal, handleMenuClass } = useMenu();
</script>

<template>
	<header style="margin-top: 50px; position: relative;">

		<modal v-model:show="dialogVisible">
			<div class="myform_title">
				<div @click="showSignIn">
					Вход
				</div>
				<span>/</span>
				<div @click="showSignUp">
					Регистрация
				</div>
			</div>

			<form v-if="signIn" @submit.prevent>

				<input type="username" placeholder="Имя пользователя" class="inputbox" required v-model="loginForm.username">
				<input type="password" placeholder="Пароль" class="inputbox" required v-model="loginForm.password">
				<div class="forgot-password">
					<a href="#">Забыли пароль?</a>
				</div>
				<button class="login_button" @click="tryLogin">Войти</button>
			</form>

			<form v-if="signUp" @submit.prevent>

				<input type="username" placeholder="Имя пользователя" class="inputbox" required v-model="registerForm.username">
				<input type="email" placeholder="Электронная почта" class="inputbox" required v-model="registerForm.email">
				<input type="password" placeholder="Пароль" class="inputbox" required v-model="registerForm.password">

				<button class="login_button" @click="tryRegister">Зарегистрироваться</button>
			</form>
		</modal>

		<div class="header_all_info">
			<section role="logo" style="display: flex">
				<div style="margin-right: 20px;" class="logo">
					<a href="/">
						NicePC
						<p class="best-for-you">лучшее для вас</p>
					</a>

				</div>
				<div class="choose_number">
					<select name="">
						<option value="">Петрозаводск</option>
						<option value="">Хабаровск</option>
						<option value="">Саратов</option>
						<option value="">Калуга</option>
					</select>
					<div>+7(981)459-09-38</div>
				</div>
			</section>
			<Hamburger @click="handleMenu" v-on-click-outside="closeModal" />
		</div>
		<div :class="handleMenuClass">
			<ul>
				<li @click="showDialog" class='login_ui' v-if="localStorageToken === null">
					<button>Войти</button>
					<UserIcon />
				</li>
				<li @click="$router.push('/profile')" class='login_ui' v-if="localStorageToken !== null">
					<button>{{localStorageUsername }}</button>
					<UserIcon/>
				</li>
				<li class='compare_ui' @click="$router.push('/profile/orders')"><button>Заказы</button>
					<Compare />
				</li>
				<li @click="tryOpenCart" class='cart_ui'><button>Корзина</button>
					<Cart />
				</li>
				<li @click="tryLogout" v-if="localStorageToken !== null">
					<button>Выйти</button>
					<Logout/>
				</li>
			</ul>
		</div>

		<div class="catalog">
			<div class="catalog_search">
				<input class="input_search" type="text" placeholder="Поиск по комплектующим">
			</div>


		</div>
	</header>
</template>

<script>
import Modal from '@/components/Modal.vue'
import Hamburger from '@/components/icons/Hamburger.vue'
import UserIcon from '@/components/icons/UserIcon.vue';
import Cart from '@/components/icons/Cart.vue';
import Compare from '@/components/icons/Compare.vue';
import Favorites from '@/components/icons/Favorites.vue';
import Logout from '@/components/icons/Logout.vue';
import axios from 'axios'
export default {
	components: {
		Modal, Hamburger, UserIcon, Cart, Compare, Favorites, Logout
	},
	data() {
		return {
			dialogVisible: false,
			loginForm: {
				username: '',
				password: '',
			},
			registerForm: {
				username: '',
				email: '',
				password: ''
			},
			localStorageToken: localStorage.getItem('token'),
			localStorageUsername: localStorage.getItem('username'),
			upHere: false,
			signIn: true,
			signUp: false
		}
	},
	methods: {
		tryOpenCart() {
			if (this.localStorageToken !== null) {
				location.replace("cart")
			}
			else {
				this.showDialog()
			}
		},
		showDialog() {
			this.dialogVisible = true
		},
		tryLogin() {
			this.dialogVisible = false
			this.$store.dispatch('onLogin', {
				login: this.loginForm.username,
				password: this.loginForm.password
			})
				.then(() => {
					location.reload()
				})
		},
		tryLogout() {
			this.$store.dispatch('onLogout')
				.then(() => {
					location.replace('/')
				})
		},
		showSignIn() {
			this.signIn = true
			this.signUp = false
		},
		showSignUp() {
			this.signIn = false
			this.signUp = true
		},
		async tryRegister() {
			await axios.post('https://nicepc-1-s2894705.deta.app/user/', this.registerForm).then((res) => {
				if (res.status === 200) {
					this.dialogVisible = false
					this.signIn = true
					this.signUp = false
				}
			})
		}
	},
}
</script>

<style lang="scss">
@import "@/components/MyHeader.scss";
</style>