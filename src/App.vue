<template>
  Selected Date:
  <DatePicker v-model:modelValue="dateValue" :instantSelect="true"/>
</template>

<script>

import DatePicker from '@/components/DatePicker.vue'

export default {
  name: 'App',
  props:{instantSelect:Boolean},
  components: {
    DatePicker
  },
  data(){
    return{
      dateValue:'',
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}




</style>
