<template>
    <div class="flex items-center justify-between p-5 border-b-2 border-neutral-800 md:flex-col md:items-center md:justify-around md:h-full md:border-0 md:border-r-2">
        <p>Монет: {{money}} </p>
        <p>Вместимость: {{cart}}</p>
        <p>Город: {{city}}</p>
        <button class="btn" @click="this.$emit('save')">Сохранить</button>
    </div>
</template>

<script>
export default {
    name:'gameInfo',
    props: ['money','cart','city'],
    data(){
        return{
           money: this.money,
           cart: this.cart,
           city: this.city,
        }
    }
}
</script>