<template>
  <div class="card" @mouseenter="cardHover = true" @mouseleave="cardHover = false">
    <div class="card__img"><img :src="item.itemImg" alt=""></div>
    <div class="card__text-box">
      <p class="card__name">{{item.itemName}}</p>
      <p class="card__descr">{{item.itemDescr}}</p>
      <p class="card__price">{{item.itemPrice}} руб.</p>
    </div>
    <transition name="fade">
      <div class="delete-btn" v-show="cardHover" @click="$emit('deleteitem', item.id)">
        <img src="../assets/delete.svg" alt="">
      </div>
    </transition>
  </div>
</template>

<script>
  export default {
    props: ['item'],
    data(){
      return {
        cardHover: false,
      }
    },
    methods:{
      
    }
  }
</script>

<style lang="scss" scoped>
  .card {
    max-width: 340px;
    position: relative;
    background: #FFFEFB;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    border-radius: 4px;
    display: flex;
    flex-direction: column;
    .delete-btn {
      position: absolute;
      right: -11px;
      top: -11px;
      cursor: pointer;
      z-index: 999;
    }
    &__text-box {
      padding: 0 16px;
    }
		&__img {
      border-radius: 4px 4px 0px 0px;
      min-width: 340px;
      width: 100%;
      height: 280px;
      img {
        object-fit:cover;
        width: 100%;
        height: 100%;
      }
		}
		// .card__name
		&__name {
      margin-top: 16px;
      font-style: normal;
      font-weight: 600;
      font-size: 20px;
      line-height: 25px;
      color: #3F3F3F;
		}
		// .card__descr
		&__descr {
      margin-top: 16px;
      font-style: normal;
      font-weight: 400;
      font-size: 16px;
      line-height: 20px;
      color: #3F3F3F;
      max-height: 80px;
      height: 100%;
      overflow: hidden;
		}
		// .card__price
		&__price {
      width: 100%;
      margin-top: 32px;
      font-style: normal;
      font-weight: 600;
      font-size: 24px;
      line-height: 30px;
      color: #3F3F3F;
      padding-bottom: 24px;
		}
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>