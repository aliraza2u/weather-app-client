<script>
import axios from "axios";
import ForcastCard from '../components/ForcastCard.vue';
import RingLoader from 'vue-spinner/src/RingLoader.vue'
export default {
    name: "HomeView",
    components: { ForcastCard, RingLoader },
    data() {
        return {
            value: null,
            cityTime: null,
            loading: false,
            hourlyForcastLoading: false,
            list: [],
            hourlyForcast: [],
            date: new Date().toDateString(),
            time: new Date().toLocaleTimeString('en-Us', { hour: "numeric", minute: "numeric" }),
            baseURL: 'http://localhost:4000'
        }
    },
    methods: {
        async fetchWeatherData(lat, lon) {
            this.loading = true;
            try {
                const result = await axios({
                    method: "GET",
                    baseURL: "http://localhost:4000/forcast",
                    params: {

                        city: this.value && this.value,
                        latitude: lat && lat,
                        longitude: lon && lon,
                    },
                });
                this.list = result.data;
                // console.log(new Date(1669593600 * 1000 + (18000 * 1000))); // plus //timezone=18000 //time in sec=1669593600
            } catch (error) {
                console.log("Error -:", error);
            }
            finally { this.loading = false }
        },

        async fetchHourlyForcast(lat, lon) {
            this.hourlyForcastLoading = true;
            try {
                const response = await axios({
                    method: "GET",
                    baseURL: "http://localhost:4000/hourly-forcast",
                    params: {
                        city: this.value && this.value,
                        latitude: lat && lat,
                        longitude: lon && lon,
                        ctn: 5
                    },
                });
                this.hourlyForcast = response.data.hourlyForcast
                this.hourlyForcast.shift()

            } catch (error) {
                console.log("Horuly Forcast Error -:", error);
            }
            finally { this.hourlyForcastLoading = false }
        },
        handleClick() {
            this.fetchWeatherData()
            this.fetchHourlyForcast()
        },


        getCurrentLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition((position) => {
                    this.fetchWeatherData(position.coords.latitude, position.coords.longitude);
                    this.fetchHourlyForcast(position.coords.latitude, position.coords.longitude);
                    this.value = '';
                });
            }
        }
    },

    async mounted() {
        this.getCurrentLocation();

    },
}
</script>

<template>
    <main class="mainWrapper">
        <div class="weatherWrapper">
            <div class="currentWeatherWrapper">
                <h1 class="heading">Weather Forcast</h1>
                <div class="searchWrapper">
                    <input v-model="value" type="search" placeholder="Search By City Name" />
                    <button v-on:click="handleClick">Search</button>
                    <img src="../assets/location.png" v-on:click="getCurrentLocation" />
                </div>
                <p>{{ date }}</p>
                <div v-if="loading" class="loadingWrapper">
                    <RingLoader color="#fff" />
                </div>
                <div v-if="!loading" class="currentWeather">
                    <h1>{{ list?.location?.name }} , {{ list?.location?.country }}</h1>
                    <div class="weather">
                        <div class="status">
                            <img
                                :src="`http://openweathermap.org/img/wn/${list?.weatherResult?.[0]?.weather?.[0]?.icon}@2x.png`" />
                            <p>{{ list?.weatherResult?.[0]?.weather?.[0]?.main }}</p>
                        </div>
                        <h2>{{ list?.weatherResult?.[0]?.main?.temp || 0 }} ℃</h2>
                        <div class="temperature">
                            <p>Min Temp: {{ list?.weatherResult?.[0]?.main?.temp_min || 0 }} ℃</p>
                            <p>Max Temp: {{ list?.weatherResult?.[0]?.main?.temp_max || 0 }} ℃</p>
                            <p>Humidity: {{ list?.weatherResult?.[0]?.main?.humidity || 0 }} %</p>
                        </div>
                    </div>
                    <div v-if="!loading" class="hourlyForcastWrapper">
                        <div v-for="item in hourlyForcast" :key="item.dt" class="cardOutline">
                            <ForcastCard :item="item" hourly=true />
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="!loading" class="forcastWrapper">
                <div class="cardOutline" v-for="item in list.weatherResult"
                    :key="list?.weatherResult?.[0]?.main?.temp_min">
                    <ForcastCard :item="item" daily=true />
                </div>
            </div>
        </div>
    </main>
</template>

<style scoped>
.mainWrapper {
    display: grid;
    place-items: center;
    height: 100vh;

}

.weatherWrapper {
    width: 100%;
    background: #6ec7db;
    background: radial-gradient(circle, #3d8ac9 0%, #587cbb 63%, #3d8ac9 100%);
    height: 100vh;
    padding: 4px;
    display: grid;

}

.currentWeatherWrapper p {
    color: #fff;
    font-weight: 600;
    padding: 1rem 2rem;
}

.heading {
    background-color: #3d8ac9;
    text-align: center;
    color: #fff;
    font-size: 36px;
    font-weight: 600;
}

.searchWrapper {
    margin-top: 1rem;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    place-items: center;
    ;
    margin-left: 2rem;
}

.searchWrapper input {
    width: 70vw;
    height: 5vh;
    border: none;
    outline: none;
    border-radius: 4px;
    font-size: 16px;
    font-weight: 600;
    padding: 0 8px;
}

.searchWrapper button {
    width: 10vw;
    height: 5vh;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    background-color: #3d8ac9;
    color: #fff;
    font-weight: 600;
}

.searchWrapper button:hover {
    background-color: #fff;
    color: #3d8ac9;
}

.searchWrapper img {
    width: 30px;
    height: 30px;
    object-fit: contain;
    cursor: pointer;
}

.currentWeather {
    box-shadow: 0px 0px 6px #6ec7db;
    border-radius: 6px;
    margin-left: 2rem;
    margin-right: 2rem;
    padding: 1rem 1rem;
    height: 48vh;

}

.currentWeather h1 {
    color: #fff;
    font-weight: 600;
    font-size: 22px;
}

.currentWeather .weather {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    place-items: center;
    column-gap: 30%;
}

.currentWeather .weather h2 {
    color: #fff;
    font-weight: 600;
    font-size: 20px;

}

.currentWeather .weather .status {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    place-items: center;
}

.currentWeather .weather .status p {
    color: #fff;
    font-weight: 600;
    padding: 0;
}

.currentWeather .weather .temperature p {
    color: #fff;
    font-weight: 600;
    padding: 4px;
}

.forcastWrapper {
    padding-left: 2rem;
    padding-right: 2rem;
    padding-bottom: 1rem;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 1rem;
}
.hourlyForcastWrapper {
    padding-left: 2rem;
    padding-right: 2rem;
    padding-bottom: 1rem;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
}

.cardOutline {
    height: 25vh;
    box-shadow: 0px 0px 6px #6ec7db;
    border-radius: 8px;
    color: #fff;
    display: grid;
    place-items: center;
    width: 210px;
    margin-top: 1rem;
}

.loadingWrapper {
    display: grid;
    place-items: center;
    height: 70%;
}
</style>