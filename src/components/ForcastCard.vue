<script>
export default {
    name: 'ForcastCard',
    props: {
        item: Object,
        daily: Boolean,
        hourly: Boolean
    },
    data() {
        return {
            today: new Date(this.item.dt_txt).getDay(),
            hour: new Date(this.item?.dt_txt).getHours(),
            getDay: {
                0: "Sunday",
                1: "Monday",
                2: "Tuesday",
                3: "Wednesday",
                4: "Thursday",
                5: "Friday",
                6: "Saturay",
            }
        }
    },
}

</script>

<template >
    <p v-if="daily">{{ this.getDay[this.today] }}</p>
    <p v-if="hourly">{{ this.hour }} {{ (this.hour >= 12 ? "pm" : "am") }}</p>
    <img :src="`http://openweathermap.org/img/wn/${item?.weather?.[0]?.icon}@2x.png`" />
    <p>{{ item?.weather?.[0]?.main }}</p>
    <p>{{ item?.main?.temp || 0 }} ℃</p>
</template>


<style scoped>
p {
    font-size: 20px;
    font-weight: 600;
}
</style>


<!--
@media (min-width: 1024px) {

    .greetings h1,
    .greetings h3 {
        text-align: left;
    }
} -->

