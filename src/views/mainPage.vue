<template>
    <div class="container mx-auto shadow-lg bg-neutral-900 h-96 overflow-hidden rounded-lg md:max-w-3xl md:flex">
        <ModalStart v-if="!gameStart" v-on:close='start'/>
        <div class="gameWindows md:flex md:items-center w-full" v-if="gameStart">
            <!-- Информация о игровой сессии-->
            <GameInfo :money='this.money' :cart='this.cart' :city='this.city' v-on:save="saveInLocal"/> 
            <!-- Логи инровой сессии -->
            <GameLog :money='this.money' :cart='this.cart' :cities='this.cities' :city='this.city' :events='this.events'/>
        </div>
    </div>
</template>


<script>
import axios from "axios";
import ModalStart from '@/components/newGameModal.vue'
import GameInfo from '@/components/gameInfo.vue'
import GameLog from '@/components/gameLog.vue'

export default {
    name:'MainPage',
    components:{
        ModalStart, GameInfo, GameLog
    },
    data(){
        return{
            gameStart: false,
            cities:[],
            events:[],
            // Начальные данные после старта
            city: null,
            money: null,
            cart: null,
            // Сохранение маршрута для LS
            saveData:{}
        }
    },
    // Получение всех городов с БД
    async created(){
        try {
            const citiesJSON = await axios.get(`http://localhost:3000/city`);
            this.cities = citiesJSON.data;
            const eventsJSON = await axios.get(`http://localhost:3000/event`);
            this.events = eventsJSON.data;
        } 
        catch (error) {
            console.log(error);
        }
    },
    methods:{
        // Указание рандомных значений при старте игры (Начальные деньги, размер телеги, и город)
        start(){
            const gameData = localStorage.getItem('savedGameData')
            this.gameStart=true
            if(gameData){
                this.saveData = JSON.parse(gameData)
                this.cart = this.saveData.cart
                this.money = this.saveData.money
                this.city = this.saveData.city
            }
            else{
                this.cart = Math.floor(Math.random() * 50) + 20,
                this.money = Math.floor(Math.random() * 500) + 100
                this.city = this.cities[Math.floor(Math.random() * this.cities.length)].name
            }
        },
        // Сохранение в localStorage (Костыль)
        // ~ Дописать кнопку продолжить или не показывать начальную модалку
        // получить фаил логов и запушить в localStorage
        saveInLocal(){
            this.saveData={money: this.money, cart: this.cart, city: this.city}
            localStorage.setItem('savedGameData', JSON.stringify(this.saveData))
        },
    }
}
</script>