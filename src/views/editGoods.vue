<template>
    <div class="container mx-auto shadow-lg bg-neutral-900 h-96 rounded-lg md:max-w-3xl overflow-auto">
        <!-- Контент с продуктом/товаром -->
        <!-- state Editable нужен для выбора одного элемента и доступа к его данным -->
        <div class="flex p-5 justify-between items-center border-neutral-800" :class="{'border-b-2':product.id !== products.length, 'border-2 border-orange-300':editable===product}" v-for="product in products" :key="product.name">
            <h3>{{product.id}}. {{product.name}}</h3>
            <div class="flex justify-between">
                <input type="number" :value="product.price" class="w-10 bg-transparent text-slate-200 mx-2 outline-none" :disabled='editable!==product'>
                <input type="number" :value="product.size" class="w-10 bg-transparent text-slate-200 mx-2 outline-none" :disabled='editable!==product'>
                <button @click="editProduct(product)" class="btn" v-if="editable===product">Сохранить</button>
                <button @click="selectProduct(product)" class="btn" v-else>Изменить</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name:'EditProduct',
    data(){
        return{
            products:[],
            editable:null
        }
    },
    // Получение данных с БД
    async created(){
        try {
            const productJSON = await axios.get(`http://localhost:3000/product`);
            this.products = productJSON.data;
        } 
        catch (error) {
            console.log(error);
        }
    },
    methods:{
        // 
        selectProduct(product){
            this.editable = product
        },
    // 
    // * ОБЯЗАТЕЛЬНО ДОДЕЛАТЬ
    // 
        // Переписать изменение элемента и привязать отправку в БД
        editProduct(product){
            console.log(this.editable.name)

            this.editable=null
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