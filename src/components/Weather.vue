<script>
import axios from 'axios'

export default {
	data() {
		return {
			city: "",
			error: "",
			fetchError: null,
			info: null,
			shownCity: ""
		}
	},
	computed: {
		cityName() {
			return "'" + this.city + "'"
		},
		showTemp() {
			return "Температура: " + this.info.main.temp
		},
		showFeelsLike() {
			return "Ощущается как: " + this.info.main.feels_like
		},
		showMinTemp() {
			return "Мин. температура: " + this.info.main.temp_min
		},
		showMaxTemp() {
			return "Мак. температура: " + this.info.main.temp_max
		},
	},
	methods: {
		getWeather() {
			this.info = null
			this.fetchError = null
			if (this.city.trim().length < 3) {
				this.error = "Название города должно быть более 2 символов!"
				console.error(this.error)
				return false
			}
			else {
				this.error = ""
			}
			if (this.error == "") {
				axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=fb785ba3df0fb1716f138b4ffa6f7f3e`)
					.then(response => (this.info = response.data))
					.then(() => this.shownCity = this.city)
					.catch(() => (this.fetchError = "Город не найден!"))
			}
		}
	},
	props: {
		msg: String
	}
}


</script>

<template>
	<div class="wrapper">
		<h1>{{ msg }}</h1>
		<p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
		<input v-model="city" type="text" placeholder="Введите город" />
		<button v-bind:disabled="city == ''" @click="getWeather()">
			Узнать погоду
		</button>
		<p class="error">{{ error }}</p>

		<div v-if="info != null && fetchError == null">
			<p>{{ this.shownCity }}</p>
			<p>{{ showTemp }}</p>
			<p>{{ showFeelsLike }}</p>
			<p>{{ showMinTemp }}</p>
			<p>{{ showMaxTemp }}</p>
		</div>
		<div class="error" v-else>
			<p>{{ this.fetchError }}</p>
		</div>
	</div>
</template>

<style>
.error {
	color: #FF0000;
}

.wrapper {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

input {
	width: 350px;
	height: 40px;
	padding-left: 10px;
	padding-right: 10px;
	margin-top: 30px;
	border-radius: 10px;
}

button {
	margin-top: 30px;
}
</style>
