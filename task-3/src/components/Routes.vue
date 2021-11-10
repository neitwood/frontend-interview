<template>
  <div class="hello">
    <div
      v-for="(route,id) in calcRoutes" :key="id"
    >
      <v-container>
        <v-row>
          <div
              v-for="(detailRoute) in route" :key="id + detailRoute.src + detailRoute.des"
          >
            {{detailRoute.src + ' '+ detailRoute.company +'-> ' + detailRoute.des}}
          </div>
        </v-row>
      </v-container>

    </div>
  </div>
</template>

<script>
export default {
  name: 'Routes',
  props:{
    routes: Object,
    side: Object
  },
  data() {
    return{
      testSrc: "ru",
      testDes: "kz",
      calcRoutes: [],
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
      for(let company in this.routes){
        this.routes[company].forEach(route=>{
          console.log('iterate');
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
              this.calcRoutes.push(this.localRoutes);
              // Если это маршрут прямого соединения то сбрасываем временный маршрут
              // Чтобы была возможность формироваться новому маршруту
              if(route.src === this.testSrc && route.des === this.testDes) this.localRoutes=[];
            }
          }
        })
      }
      // Возможность формироваться новому маршруту после очередной рекурсии
      this.localRoutes = [];
      // this.deleteLastElement(src);
    },
    deleteLastElement(src){
      if(this.localRoutes.length>1 && this.localRoutes[this.localRoutes.length-1].src === src) {
        this.localRoutes.splice(this.localRoutes.length - 1, 1);
      }
    }
  },
  watch:{
    side: function (event) {
      if(event.el === 'src') this.testSrc = event.val;
      if(event.el === 'des') this.testDes = event.val;
      this.calcRoutes=[];
      this.generate(this.testSrc);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
