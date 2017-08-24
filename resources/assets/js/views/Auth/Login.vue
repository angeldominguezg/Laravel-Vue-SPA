<template>
	<form class="form" @submit.prevent="login">

		<h1 class="form__title">Welcome!</h1>


		<div class="form__group">
			<label>Email</label>
			<input type="text" class="form__control" v-model="form.email">
			<small class="error__control" v-if="error.email">{{ error.email[0] }}</small>
		</div>

		<div class="form__group">
			<label>Password</label>
			<input type="password" class="form__control" v-model="form.password">
			<small class="error__control" v-if="error.password">{{ error.password[0] }}</small>
		</div>

		<div class="form__group">
			<button :disabled="isProcessing" class="btn btn__primary">
				Access
			</button>
		</div>	
	</form>
</template>

<script>

	import { post } from '../../helpers/api'
	
	import Flash from '../../helpers/flash'

	import Auth from '../../store/auth'

	export default {
		data() {
			return {
				form: {
					email: '',
					password: '',
				}, 
				error: {

				}, 
				isProcessing:false
			}
		},
		methods: {
			login() {
				
				this.isProcessing = true
				
				this.error = {}

				post(`/api/login`, this.form)
					.then((res) => {

						if(res.data.autenticated){
							Auth.set(res.data.api_token, res.data.user_id)
							Flash.setSuccess('You have successfully logged in!')
							this.$router.push('/')
						}

						this.isProcessing = false

					})
					.catch((err) => {
						if(err.response.status === 422) {
				
							this.error = err.response.data

							console.log(this.error)

						}

						this.isProcessing = false
					})
			}	
		}
	}	
</script>
