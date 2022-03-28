<template>
  <h2>detail {{$route.params.id}} 

  </h2>
  {{coins.name}} <br>
  <img :src="coins.image.thumb" alt="">
</template>

<script>
import {ref, onBeforeMount} from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'
export default {
setup(){
  const coins = ref([])
  const route = useRoute()

  onBeforeMount(()=>{
      fetch(
        (`https://api.coingecko.com/api/v3/coins/${route.params.id}?market_data=true`)
      )
      .then(res => res.json())
      .then(data =>{
        coins.value = data
        console.log(coins.value);

      })

  })
  return{
    coins
  }

}
}
</script>

<style>

</style>