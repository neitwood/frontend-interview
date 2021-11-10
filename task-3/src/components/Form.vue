<template>
  <v-container>
    <v-col>
      <v-select
          label="Звонящая сторона (Caller)"
          item-text="desc"
          item-value="code"
          v-model="src"
          :items="formatCountries"
      ></v-select>
      <v-select
          label="Принимающая сторона (Callee)"
          item-text="desc"
          item-value="code"
          v-model="des"
          :items="formatCountries"
      ></v-select>
      <v-checkbox
          v-model="checkbox"
          :label="`Checkbox: ${checkbox.toString()}`"
      ></v-checkbox>
    </v-col>
  </v-container>
</template>

<script>
export default {
  name: "Form",
  props:{
    countries: Object
  },
  data() {
    return{
      src: 'ru',
      des: 'kz',
      checkbox: true,
    }
  },
  methods:{
    proxyCaller(val){
      console.log(val);
    }
  },
  computed:{
    // getNodesCount(){
    //
    // }
    formatCountries(){
      return Object.keys(this.countries).map(code=>{
        return {
          code: code,
          desc: `${this.countries[code]} (${code})`
        }
      })
    }
  },
  watch:{
    src: function (val) {
      this.$emit('changedSide', {
        el: 'src',
        val: val
      });
    },
    des: function (val) {
      this.$emit('changedSide', {
        el: 'des',
        val: val
      });
    },
  }
}
</script>

<style scoped>

</style>