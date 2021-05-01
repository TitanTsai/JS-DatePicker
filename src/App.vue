<template>
  <DatePicker v-model:modelValue="dateValue"/>
</template>

<script>

import DatePicker from '@/components/DatePicker.vue'

export default {
  name: 'App',
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

:root  {
    --background:#ffffff;
    --select:#F2F4F6;
    --primary:#008FFD;
    --white:#f2f2f2;
    --caption:#19191B;
    --disabled:#c2c2c2;
}


</style>
