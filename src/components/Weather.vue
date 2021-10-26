<template>
    <div class="weather">
        <div class="block add">
            <input type="text" v-model="inputCity">
            <button @click="addCity()">Add</button>
        </div>
        <div class="block cities" v-if="cities.length">
            <select name="cities" v-model="selectedCity">
                <option v-for="city in cities" :key="city" :value="city">{{ city }}</option>
            </select>
            <router-link :to="'/' + selectedCity">Search</router-link>
        </div>
        <div v-else class="block">
            <p>List cities empty</p>
        </div>
        <div v-if="info.main" class="info">
            <p>City: {{ info.name }}</p>
            <p>Humidity: {{ info.main.humidity }}</p>
            <p>Temp: {{ (info.main.temp - 273.15).toFixed(2) }}</p>
            <p>Pressure: {{ info.main.pressure }}</p>
            <p>Description: {{ info.weather[0].description }}</p>
            <p>Wind: {{ info.wind.speed }}</p>
            <p>Deg: {{ info.wind.deg }}</p>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    
    export default {
        name: 'Weather',
        data() {
            return {
                cities: [],
                inputCity: '',
                selectedCity: '',
                info: {},
            }
        },
        mounted() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition((position) => {
                    axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=7914d5a440960cfd5df3bd0388a7ad0f`)
                        .then((response) => this.info = response.data)
                })
            }
        },
        methods: {
            addCity() {
                this.cities.push(this.inputCity)
                this.selectedCity = this.inputCity
                this.inputCity = ''
            },
        }
    }
</script>

<style scoped>
    h3 {
        margin: 40px 0 0;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
    .block {
        margin-bottom: 20px;
    }
    input, select {
        margin-right: 4px;
    }
    .info {
        background-color: aliceblue;
        width: fit-content;
        margin: 0 auto;
        padding: 6px 10px;
        border-radius: 12px;
    }
</style>
