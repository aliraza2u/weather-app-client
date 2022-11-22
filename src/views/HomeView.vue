<script>
import axios from "axios";
export default {
    name: "HomeView",
    data() {
        return {
            value: null,
            list: []
        }
    },
    methods: {
        handleClick() {
            console.log("value--------", this.value);
        },
        getCurrentLocation() { }
    },
    async mounted() {
        let result = await axios.get(`http://localhost:4000/forcast?=lahore&latitude=44.34&longitude=10.99`)
        this.list = result.data
    }
}
</script>

<template>
    <main class="mainWrapper">
        <div class="weatherWrapper">
            <h1 class="heading">Weather Forcast</h1>
            <div class="searchWrapper">
                <input v-model="value" type="search" placeholder="Search By City Name" />
                <button v-on:click="handleClick">Search</button>
                <img src="../assets/location.png" v-on:click="getCurrentLocation" />
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
    height: 70vh;
    border-radius: 8px;
    padding: 4px;

}

.heading {
    background-color: #3d8ac9;
    text-align: center;
    color: #fff;
    font-size: 36px;
}

.searchWrapper {
    margin-top: 16px;
    display: flex;
    gap: 24px;
    align-items: center;
    /* text-align: center; */
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
</style>