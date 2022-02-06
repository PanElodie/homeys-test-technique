<template>
    <form>
        <label :for="type">{{name}}</label>

        <input type="date" :min="datas.min" :max="datas.max" v-model="value" @change="$emit('send-dates', {name : this.type, date: this.value})">
    </form>
    
</template>

<script>
    export default {
        name: 'Calendar',
        props: {
            type: String,
            datas: Object
        },
        data() {
            return {
                value: '',
                name: '',
            }
        },
        created() {
            if (this.type == 'debut') {
                this.name = 'Début';
            } else {
                this.name = 'Fin'
            }
        }
    }
</script>

<style scoped>
    form {
        margin: 0 20px;
    }

    label {
        display: block;
        margin: 20px 0;
        text-align: center;
    }

    @media screen and (min-width: 360px){
        label {
            text-align: left;
        }
    }
</style>