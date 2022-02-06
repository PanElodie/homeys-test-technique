<template>
    <div class="total-appels">
        <p class="ref">Total d'appels</p>
        <p>{{count}}</p>
        <p class="ref">Période</p>
        <p>{{start}} - {{end}}</p>
    </div>
</template>

<script>
export default {
    name: 'Total Calls',
    props: {
        count: Number,
        start: String,
        end: String
    }
}
</script>

<style scoped>
    .total-appels {
        width: 50%;
        margin: 50px auto;
        padding: 20px 40px;
        background-color: #33475b;
        border-radius: 10px;
    }

    p {
        color: #fff;
    }

    .ref {
        color: #179aaf;
    }

    @media screen and (min-width: 360px){
        .total-appels {
            width: fit-content;
        }
    }
</style>