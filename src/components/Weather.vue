<template>
    <div class="weather-div">
        <div class="weather-display">
            <div class="card">
                <div class="card-header">
                    <p>{{ city }}, {{ state}} {{ zip }}</p>
                </div>
                <div class="card-body">
                    <p v-if="weather">Todays Forecast: {{ weatherDescription }}</p>
                    <p v-if="temp">Temp: {{ temp }}&#8457;</p>
                </div>
            </div>
          <img src="~@/assets/rainy.jpg" v-if="weather === 'Rainy'" />
          <img src="~@/assets/cloudy.jpg" v-else-if="weather === 'Cloudy'" />
          <img src="~@/assets/sunny.jpg" v-else />
        </div>
    </div>

</template>

<script>
    import axios from 'axios';

    export default {
        name: 'Weather',
        data() {
            return {
                weather: '',
                weatherDescription: '',
                temp: 0.0,
                city: 'Nashville',
                state: 'TN',
                zip: '37214',
                appID: process.env.VUE_APP_ID
            }
        },
        methods: {
            kelvinToFahrenheit(kelvin) {
                return (kelvin - 273.15) * (9 / 5) + 32;
            }
        },
        created() {
            let zipCode = '37214';
            let countryCode = 'us';

            axios.get(`http://api.openweathermap.org/data/2.5/weather?zip=${zipCode},${countryCode}&appid=${this.appID}`)
                .then(res => {
                    this.weather = res.data.weather[0]['main'];
                    this.weatherDescription = res.data.weather[0]['description'];
                    this.city = res.data.name;
                    this.temp = this.kelvinToFahrenheit(res.data.main.temp)
                })
                // eslint-disable-next-line no-console
                .catch(err => console.log(err));
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
