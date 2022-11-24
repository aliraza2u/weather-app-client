<script>
export default {
    name: 'ForcastCard',
    props: {
        item: Object
    },
}

</script>

<template >
    <p>{{ item?.weather?.[0]?.main }}</p>
    <img :src="`http://openweathermap.org/img/wn/${item?.weather?.[0]?.icon}@2x.png`" />
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

