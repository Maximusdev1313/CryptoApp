<template>
  <q-page >
    <q-list bordered class="rounded-borders" >
      <q-item-label header>Friends</q-item-label>
     <div
       v-for="coin in coins"
       :key="coin.index">
        <q-item
          clickable
          v-ripple
        
          :to="'/coin/'+coin.id">
        <!-- for displaying image and name of the coins -->
          <q-item-section avatar class="gt-sm " >
            <q-avatar>
              <img :src="coin.image"  >
            </q-avatar>
          </q-item-section>

          <q-item-section>
            <q-item-label
              lines="1"
              class="text-h5 text-weight-medium">
              {{coin.name}}
            </q-item-label>
            <q-item-label
              caption
              lines="2">
              <span class="text-weight-bold">
                {{coin.symbol}}
              </span>
              -- {{coin.ath_date}}
            </q-item-label >
          </q-item-section>
        <!-- for display current prices -->
          <q-item-section side top>
            <div>Price: {{coin.current_price}}  <span class="gt-sm">&bull; Cap: {{coin.market_cap}}$</span> </div><br>
            <div
              :class="[ priceChange(coin.price_change_percentage_24h) ? 'negative' : 'positive']">
              {{coin.price_change_24h}} &bull; 
              ({{coin.price_change_percentage_24h}})% 
            </div>
          </q-item-section>
        </q-item> 
        <q-separator />
      </div>
   </q-list>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue';
import axios from 'axios' 
export default defineComponent({
  name: 'PageIndex',
  setup(){
    const coins = ref([])
    const getCoins = async ()=>{
      try {
        const res = await axios.get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&price_change_percentage=1h')
        coins.value = res.data
       console.log(coins.value);
      }
      catch(err){
        console.log(err);
      }
    }
    setInterval(()=>{
      getCoins()
    }, 1500)

    const priceChange = (value)=>{
      value = value.toString()
      if(value.includes('-')){
        return value
      }else{
        return ''
      }
    }
    return{
      coins,
      priceChange
    }
  }
})
</script>
<style lang="scss" scoped>
  .negative{
    color: red
  }
  .positive{
    color: rgb(29, 255, 8);
  }
</style>

