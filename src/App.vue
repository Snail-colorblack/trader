<template>
      <nav>
        <router-link to="/">Главная</router-link>
        <router-link to="/editGoods">Редактировать товары</router-link>
      </nav>
      <router-view/>
</template>

<script>
export default {
  name: 'Trader',
  created(){
    alert('В консоле есть важная инфа')
      console.log("\x1b[41m ЧТО НЕ ТАК С ПРОЕКТОМ!", "Сохранения в localStorage работают, но я не записываю в них ничего кроме начальных параметров. Кнопки удалить тоже нет, хотя я и мог бы ее сделать, просто забил =) \n\n",
      "Файлы логов я тоже не храню из-за ошибки в ивантах. Не заходит в if. Я пробовалд условие через ?obj.?field.?elem но бесполезно.\n\n" ,
      "Я не смог нормально их расписать. Вообще у меня куча коммов в коде, но это не помогает. \n\n",
      "Много чего нужно было делать через .map, но я слишком не шарю \n\n",
      "А вообще все превратилось в легаси уже на первом дне. Кстати я ничего не закупаю и не продаю. \n\n Я просто завтыкал исправлять, переписывать и 'доделывать' это 'код'")
      console.log("\x1b[36m  Я максимально не доволен тем, что сделал и как сделал. Я буду очень рад, если меня прям тыкнут, что я сделал не так. Как можно было-бы лучше.")
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #eeeeee;
  background: #1b1c1e;
  height: 100vh;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #eeeeee;
  margin: 0px 10px;
}

nav a.router-link-exact-active {
  color: #f2ae25;
}
</style>
