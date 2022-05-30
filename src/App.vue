<template>
  <div class="container">
    <div class="top__section">
      <page-title :text="'Добавление товара'"></page-title>
      <Filter @toMax="sortToMax" @toMin="sortToMin" @toAlph="sortToAlph" @default="toDefault"/>
    </div>
    <div class="bottom__section">
      <add-item-form @updatecards="getItems()"></add-item-form>
      <div class="cards-box">
        <div class="loader" v-if="loading"><img src="./assets/load.gif" alt=""></div>
        <transition-group name="fade">
          <item-card v-for="(item, index) in databaseItems" :key="item.id" :item="item" @deleteitem="deleteItem(item.id, index)"></item-card>
        </transition-group>
        </div>
    </div>

  </div>
</template>
<script>
import PageTitle from "./components/PageTitleComponent.vue"
import AddItemForm from "./components/AddItemComponent.vue"
import Filter from "./components/FilterItemComponent.vue"
import ItemCard from "./components/ItemCardComponent.vue"
import axios from 'axios'
export default {
  components: { PageTitle, AddItemForm, Filter, ItemCard },
  name: 'App',
  data(){
    return {
      loading: true,
      databaseItems: null,
    }
  },
  methods:{
    async deleteItem(id, index){
      await axios.delete(`http://localhost:3000/products/`+id)
        this.databaseItems.splice(index, 1)
    },
    async getItems(){
      await axios.get('http://localhost:3000/products')
      .then((res)=>{
        let {data} = res
        this.loading = false
        this.databaseItems = data;
      })
    },
    sortToMax(){
      this.databaseItems.sort((prev, next)=> prev.itemPrice - next.itemPrice)
    },
    sortToMin(){
      this.databaseItems.sort((prev, next)=> next.itemPrice - prev.itemPrice )
    },
    sortToAlph(){
      this.databaseItems.sort((prev, next)=> {
        var aname = prev.itemName.toLowerCase(),
            bname = next.itemName.toLowerCase();
          if(aname < bname) return -1;
          if(aname > bname) return 1;
      } )
    },
    toDefault(){
      this.loading = true;
      setTimeout(()=>{
        this.loading = false;
      }, 200)
      this.getItems()
    }
  },
  mounted: async function(){
    this.getItems()
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@600&family=Source+Sans+Pro:wght@400;600&display=swap');
* {
  font-family: 'Source Sans Pro', sans-serif;
  padding: 0;
  margin: 0;
}
html {
  min-height: 100vh;
}
.container {
  max-width: 1420px;
  padding: 0 20px;
  margin: 0 auto;
}
.top__section {
  padding: 31px 0 8px;
  display: flex;
  justify-content: space-between;
}
.bottom__section {
  display: grid;
  grid-template-columns: minmax(200px, 332px) auto;
  gap: 16px;
  @media (max-width: 976px){
    grid-template-columns: 280px auto;
  }
  @media (max-width: 670px){
    grid-template-columns: auto;
  }
}
.flex {
  display: flex;
}

.cards-box {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
  row-gap: 15px;
  @media (max-width: 976px){
    justify-content: center;
  }
}
.loader {
  width: 100%;
  height: 100%;
  top: 0;
  right: 0;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  background: white;
  opacity: 0.5;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to{
  opacity: 0;
}
</style>
