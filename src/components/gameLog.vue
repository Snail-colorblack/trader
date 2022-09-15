<template>
    <div class="flex items-start flex-col w-full h-full p-5 px-10 overflow-auto">
        <h4 class="font-medium text-xl border-b-2 border-neutral-800 py-2">Ты едешь в '{{this.moveTo}}'. До него осталось {{this.leftLigs}} лиг</h4>
        <div class=" my-3 text-left border-b-2 border-neutral-800" v-for="log in logs" :key="log.id">
            <p>День {{log.day}}</p>
            <p>Сегодня: {{log.event}}</p>
            <p>Проехал {{log.speed}}</p>
            <p>Осталось проехать: {{log.ligs}}</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Logs',
        props: ['money','cart','cities','city','events'],
    data(){
        return{
            // Торговец
            dealler:[],
            // Продукты
            cartProducts:[],
            products:[],
            // Процесс игры:
            event:null,
            moveTo:null,
            passedLigs: null,
            leftLigs:null, // Оставшиеся
            day:0,
            speed: 4 + this.speedIvent,
            speedIvent: 0,
            // Массив с логами
            logs:[],
            logObj:{cart: this.cartProducts ,eventname:this.event, speed: this.speed,city:this.city,to: this.moveTo, day: this.day, ligs: this.leftLigs},
            // Проверка цикла
            process: true,
        }
    },
    methods:{
        gameProcess(){
            // Назначаю лиги городам
            this.cityLigs();
            // Выполняем закупку товара в выбранном городе. (Покупать случайный товар, пока есть деьги или место)
            this.bueProduct();
            // Выбираем пункт назначения
            this.toCity();
            while(this.process = true){
                // День увеличился.
                this.day+=1;
                console.log("День: ", this.day)
                console.log("Осталось проехать: ", this.leftLigs)
                // Событие
                this.roadEvent();
                // Запись в логи*
                //Проверка на достижение цели
                if(this.leftLigs === 0){
                    console.log(`Ты на месте! Пройдено ${this.passedLigs}, за ${this.day}`)
                    //Продажа товара
                    this.sellProduct();
                    let confirm = confirm('Едем дальше?')
                    if(confirm){
                        this.process = true
                    }
                    else{
                        this.process = false
                        break
                    }
                }
                else if(this.day>50){
                    break
                }
                else{
                    this.process = true
                }
            }
        },
        roadEvent(){
            let randivent = Math.floor(Math.random() * this.events.length)
            this.event = this.events[randivent].name
            this.logs.push(this.logObj)
            // Если есть доп событие 
            if(this.event[randivent].random === true){
                if(this.event[randivent].randomevent == 'productLose'){
                    console.log('Портится качество тавара')
                }
                else if(this.event[randivent].randomevent == 'thiefs'){
                    $emit.this.money = 0
                    console.log('Тебя ограбили. Ты остался без денег, но с товаром')
                }
                else if(this.event[randivent].randomevent == 'tavern'){
                    console.log('Чилим в таверне')
                }
            }
            // Получение скорости от события
            else if(this.event[randivent]?.speed){
                if(this.event[randivent].speed == null){
                    this.speedIvent = (-this.speed)
                    this.leftLigs - this.speed;
                    console.log('Мы никуда не едем')
                }
                else if(this.event[randivent].speed.length > 0){
                    this.speedIvent = this.event[randivent].speed[Math.floor(Math.random() * this.event.speed.length)]
                    this.leftLigs - this.speed;
                    console.log('Скорость увеличилась на случайное значение')
                }
                else{
                    this.speedIvent = this.event[randivent].speed
                    this.leftLigs - this.speed;
                    console.log('Обычная скорость')

                }
            }
        },
        cityLigs(){
            for(let i=0;  i< this.cities.length; i++){
                this.cities[i].distanse = Math.floor(Math.random() * 50) + 50
            }  
        },
        bueProduct(){   
            console.log('Закупка') 
        },
        toCity(){
            let selector = Math.floor(Math.random() * this.cities.length)
            this.moveTo = this.cities[selector].name
            this.leftLigs = this.cities[selector].distanse
            this.passedLigs = this.cities[selector].distanse
            console.log('Едем в ',this.moveTo, " в ", this.leftLigs, "от нас")
        },
        sellProduct(){
            console.log('Продажа')
        }

    },
    async created(){
        this.gameProcess();
        try {
            const productJSON = await axios.get(`http://localhost:3000/product`);
            this.products = productJSON.data;
        } 
        catch (error) {
            console.log(error);
        }
    }
}
</script>

<style>
*::-webkit-scrollbar, .conteiner *::-webkit-scrollbar {
  height: 4px;
  width: 4px;
}
*::-webkit-scrollbar-track, .conteiner *::-webkit-scrollbar-track {
  background: rgb(23 23 23 / var(--tw-bg-opacity));
}
*::-webkit-scrollbar-thumb, .conteiner *::-webkit-scrollbar-thumb {
  background: #f2ae25;
}
</style>