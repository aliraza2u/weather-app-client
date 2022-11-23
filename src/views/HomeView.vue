<script>
import axios from "axios";
import ForcastCard from '../components/ForcastCard.vue';
export default {
    name: "HomeView",
    components: { ForcastCard },
    data() {
        return {
            value: null,
            list: [],
            date: new Date().toDateString(),
            time: new Date().toLocaleTimeString('en-Us', { hour: "numeric", minute: "numeric" })
        }
    },
    methods: {
        handleClick() {
            console.log("value--------", this.value);
        },
        getCurrentLocation() { }
    },
    async mounted() {
        let result = await axios.get(`http://localhost:4000/forcast?=lahoref&latitude=44.34&longitude=10.99`)
        this.list = result.data
    }


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
                <p>{{ date }} | {{ time }}</p>
                <div class="currentWeather">
                    <h1>{{ list?.location?.name || "Loading..." }} , {{ list?.location?.country }}</h1>
                    <div class="weather">
                        <div class="status">
                            <img
                                :src="`http://openweathermap.org/img/wn/${list?.weatherResult?.[0]?.weather?.[0]?.icon}@2x.png`" />
                            <p>{{list?.weatherResult?.[0]?.weather?.[0]?.main}}</p>
                        </div>
                        <h2>{{ list?.weatherResult?.[0]?.main?.temp || 0 }} ℃</h2>
                        <div class="temperature">
                            <p>Min Temp: {{ list?.weatherResult?.[0]?.main?.temp_min || 0 }} ℃</p>
                            <p>Max Temp: {{ list?.weatherResult?.[0]?.main?.temp_max || 0 }} ℃</p>
                            <p>Humidity: {{ list?.weatherResult?.[0]?.main?.humidity || 0 }} %</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="forcastWrapper">
                <ForcastCard />
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
    width: 70%;
    background: #6ec7db;
    background: radial-gradient(circle, #3d8ac9 0%, #587cbb 63%, #3d8ac9 100%);
    height: 75vh;
    border-radius: 8px;
    padding: 4px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

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
    display: flex;
    gap: 24px;
    align-items: center;
    margin-left: 2rem;
}

.searchWrapper input {
    width: 70%;
    height: 5vh;
    border: none;
    outline: none;
    border-radius: 4px;
    font-size: 16px;
    font-weight: 600;
}

.searchWrapper button {
    height: 5vh;
    border: none;
    width: 10%;
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
    widows: 30px;
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

}

.currentWeather h1 {
    color: #fff;
    font-weight: 600;
    font-size: 22px;
}

.currentWeather .weather {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.currentWeather .weather h2 {
    color: #fff;
    font-weight: 600;
    font-size: 20px;

}

.currentWeather .weather .status {
    display: flex;
    align-items: center;
    gap: 1rem;
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
}
</style>