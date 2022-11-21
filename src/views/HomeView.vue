<script setup>
// import TheWelcome from '../components/TheWelcome.vue'
</script>

<template>
    <main class="mainWrapper">
        <div class="weatherWrapper">
            <h1 class="heading">Weather Forcast</h1>
            <div class="searchWrapper">
                <input type="search" placeholder="Search By City Name" />
                <img src="../assets/location.png" />
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

.searchWrapper img {
    widows: 30px;
    height: 30px;
    object-fit: contain;
    cursor: pointer;
}
</style>