<template>
    <div v-if="info.main" class="info">
        <p>City: {{ info.name }}</p>
        <p>Humidity: {{ info.main.humidity }}</p>
        <p>Temp: {{ (info.main.temp - 273.15).toFixed(2) }}</p>
        <p>Pressure: {{ info.main.pressure }}</p>
        <p>Description: {{ info.weather[0].description }}</p>
        <p>Wind: {{ info.wind.speed }}</p>
        <p>Deg: {{ info.wind.deg }}</p>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'WeatherDetailed',
        props: {
            city: String,
        },
        data() {
            return {
                info: {}
            }
        },
        mounted() {
            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=7914d5a440960cfd5df3bd0388a7ad0f`)
                .then((response) => this.info = response.data)
        }
    }
</script>

<style scoped>

</style>
