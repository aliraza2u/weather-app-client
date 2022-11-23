<script>
export default {
    name: 'ForcastCard'
}

</script>

<template>
    <div class="cardOutline">
        <p>Clear</p>
        <p>Icon</p>
        <p>15.82 ℃</p>
    </div>
</template>


<style scoped>
.cardOutline {
    width: 20%;
    height: 30vh;
    box-shadow: 0px 0px 6px #6ec7db;
    border-radius: 8px;
    color: #fff;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
}

.cardOutline p {
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

