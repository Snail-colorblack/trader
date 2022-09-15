<template>
    <!-- Зачем? да просто могу! -->
    <modal class="flex justify-center items-center h-full w-full">
        <button class="btn" @click="this.$emit('close')" >Начать</button>
    </modal>
</template>

<script>
export default {
    name: 'ModalStart',
}
</script>


<style>
.btn{
    border: 1px solid #f2ae25;
    padding: 10px 20px;
    color: #eee;
    transition: .3s;
}
.btn:hover{
    background: #f2ae25;
    color: #202020;
}
</style>
