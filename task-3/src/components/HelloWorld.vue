<template>
  <div class="hello">

  </div>
</template>

<script>
import paths from '../../call-paths.json';

export default {
  name: 'HelloWorld',
  data() {
    return{
      paths: paths,
      testSrc: "kz",
      testDes: "de",
      routes: [],
      localRoutes: [],
      mainRoute: null
    }
  },
  mounted() {
    this.generate(this.testSrc);
  },
  methods:{
    /**
     * Рекурсивная генерация всех возможных путей
     * Работает так:
     *  В главном цикле обходим все элементы.
     *  Если от звонящей стороны из пользовательской формы можно добраться до принимающей
     *  через условия route.src===src и route.des===this.testDes, то добавляем маршрут
     *
     * @param src
     */
    generate(src){
      for(let company in this.paths.data.company){
        this.paths.data.company[company].forEach(route=>{
          // При помощи этого условия зацепляемся за следующий элемент
          if(route.src===src) {
            // Здесь обновляем последний элемент в localRoutes
            if(this.localRoutes.length>1 && this.localRoutes[this.localRoutes.length-1].src === src) {
              this.localRoutes.splice(this.localRoutes.length - 1, 1);
            }
            // Всегда добавляем элемент во временный путь, если зацепление произошло
            this.localRoutes.push({...route, company: company});
            // Если принимающая сторона не та, которую задали в форме, то вызываем функцию повторно
            if(route.des!==this.testDes){
              //Передаем принимающую сторону, которая будет использоваться как звонящая (итерируемся)
              this.generate(route.des);
            } else {
              this.routes.push(this.localRoutes);
              // Если это маршрут прямого соединения то сбрасываем временный маршрут
              // Чтобы была возможность формироваться новому маршруту
              if(route.src === this.testSrc && route.des === this.testDes) this.localRoutes=[];
            }
          }
        })
      }
      // Возможность формироваться новому маршруту после очередной рекурсии
      this.localRoutes = [];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
