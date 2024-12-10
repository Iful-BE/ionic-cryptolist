<template>
  <div class="container">
    <button @click="fetchData" class="btn-refresh">Refresh</button>
    <div v-if="loading" class="loading">Loading...</div>
    <div v-else>
      <div v-for="crypto in cryptos" :key="crypto.id" class="crypto-item">
       
        <div class="group1">
          <div class="rank">Rank</div>
          {{ crypto.rank }}
        </div>
        
        <div class="group">
          <div class="name">{{ crypto.name }}</div>
          <div>{{ crypto.symbol }}</div>
        </div>
       
        <div class="group">
          <div class="price">USD</div>
          <div>{{ crypto.price_usd }}</div>
        </div>
      </div>
    </div>
  </div>
</template>



<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";

export default defineComponent({
  name: "CryptoList",
  setup() {
    const cryptos = ref([]);
    const loading = ref(false);

    const fetchData = async () => {
      loading.value = true;
      try {
        const response = await axios.get("https://api.coinlore.net/api/tickers/");
        cryptos.value = response.data.data;
      } catch (error) {
        console.error("Error fetching data", error);
      } finally {
        loading.value = false;
      }
    };

    return {
      cryptos,
      loading,
      fetchData,
    };
  },
});
</script>

<style scoped>

.container {
  max-width: 360px;
  margin: 20px auto;
  padding: 15px;
  border: 2px solid #ccc;
  border-radius: 20px;
  background-color: #f5f5f5;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  position: relative;
}


.btn-refresh {
  display: block;
  width: 100%;
  background-color: #007bff;
  color: #fff;
  padding: 12px 0;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  text-align: center;
  margin-bottom: 15px;
}

.crypto-item {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border: 1px solid #ddd;
  background-color: #f1c40f; 
  color: #151313; 
  padding: 10px;
  margin-bottom: 8px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}


.crypto-item .group div {
  color: #151313; 
}

.crypto-item .rank {
  font-weight: bold;
  font-size: 14px;
  color: #151313; 
}

.crypto-item .name {
  font-size: 16px;
  font-weight: bold;
  color: #151313; 
  margin-bottom: 4px;
}

.crypto-item .price {
  font-size: 14px;
  font-weight: bold;
  color: #151313; 
}

/* Grup */
.group {
  display: flex;
  flex-direction: column;
  align-items: flex-start; 
  text-align: left; 
  margin-right: 10px;
  flex: 1; 
}

.group1 {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center; 
  margin-right: 10px;
  flex: 1; 
}
.rank {
  font-weight: bold;
  font-size: 14px;
  color: #333;
}

.name {
  font-size: 16px;
  font-weight: bold;
  color: #555;
  margin-bottom: 4px;
}

.price {
  font-size: 14px;
  font-weight: bold;
  color: #007bff;
}

.group div {
  margin-bottom: 2px; 
}


.loading {
  text-align: center;
  margin: 20px 0;
  font-size: 16px;
  font-weight: bold;
  color: #888;
}
</style>

