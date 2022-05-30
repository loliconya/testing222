<template>
  <form class="add_form" @submit.prevent="submitHandler">
  <label for="name" class="form__label">Наименование товара <span v-if="itemName.length <= 0"></span></label>
  <input type="text" placeholder="Введите наименование товара" v-model="itemName" :class="{'danger-input' : itemName.length <= 0 && trySubmit}">
  <small v-if="itemName.length <= 0 && trySubmit" class="danger-error">Поле является обязательным</small>

  <label for="name" class="form__label">Описание товара</label>
  <textarea placeholder="Введите описание товара" v-model="itemDescr"></textarea>
  
  <label for="name" class="form__label">Ссылка на изображение товара <span v-if="itemImg.length <= 0"></span></label>
  <input type="text" placeholder="Введите ссылку" v-model="itemImg" :class="{'danger-input' : itemImg.length <= 0 && trySubmit}">
  <small v-if="itemImg.length <= 0 && trySubmit" class="danger-error">Поле является обязательным</small>

  <label for="name" class="form__label">Цена товара <span v-if="itemPrice.length <= 0"></span></label>
  <input type="number" placeholder="Введите цену" v-model="itemPrice" :class="{'danger-input' : itemPrice.length <= 0 && trySubmit}" @blur="replaceThouns">
  <small v-if="itemPrice.length <= 0 && trySubmit" class="danger-error">Поле является обязательным</small>

  <button type="submit" class="form__btn" :class="{disabled: !validateH}" @click="trySubmit = true">Добавить товар</button>
    <div class="loader" v-if="loading"><img src="../assets/load.gif" alt=""></div>
    <div class="done" v-if="done"><img src="../assets/check-circle.gif" alt=""></div>
</form>
</template>

<script>
import axios from 'axios'
  export default {
    data(){
      return {
        loading: false,
        done: false,
        showAddForm: false,
        trySubmit: false,
        showError: true,
        validate: false,
        itemName: '',
        itemDescr: null,
        itemImg: '',
        itemPrice: '',
      }
    },
    methods: {
      async submitHandler(){
        if(this.validateH) {
          let formData = {}
          formData.id = Date.now() * Math.random(1, Date.now())
          formData.itemName = this.itemName
          formData.itemDescr = this.itemDescr || 'Нет описания'
          formData.itemImg = this.itemImg
          formData.itemPrice = this.itemPrice
          this.loading = true;
          await axios.post('http://localhost:3000/products', formData)
            .then(response => (console.log(response)));
            this.loading = false;
            this.done = true;
            this.$emit('updatecards')
            setTimeout(()=>{
              this.done = false;
            },500)
        } else {
          return
        }
      },
    },
    computed: {
      validateH(){
        return this.itemName.length != 0 && this.itemImg.length != 0 && this.itemPrice.length != 0 
      },
    },
  }
</script>

<style lang="scss" scoped>
form {
  position: sticky;
  top: 10px;
  width: 100%;
  background: #FFFEFB;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  height: fit-content;
  :first-child {
    margin-top: 0;
  }
  display: flex;
  flex-direction: column;
  max-width: 332px;
  padding: 24px;
  @media (max-width: 670px){
    position: relative;
    margin: 0 auto;
    margin-bottom: 20px;
  }
  .loader {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    background: white;
    opacity: 1;
  }
  .done {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    background: white;
    opacity: 1;
    img {
      width: 20%;
    }
  }
  input {
    margin-top: 4px;
    padding: 11px 16px;
    background: #FFFEFB;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    outline: none;
    -webkit-appearance: none;
    &:focus {
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.6);
      transition: 0.4s all ease;
    }
    &::placeholder {
      font-style: normal;
      font-weight: 400;
      font-size: 12px;
      line-height: 15px;
      text-align: left;
      color: #B4B4B4;
    }
  }
  textarea {
    margin-top: 4px;
    padding: 11px 16px;
    background: #FFFEFB;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    resize: none;
    min-height: 108px;
    outline: none;
    &:focus {
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.6);
      transition: 0.4s all ease;
    }
    &::placeholder {
      font-style: normal;
      font-weight: 400;
      font-size: 12px;
      line-height: 15px;
      text-align: left;
      color: #B4B4B4;
    }
  }
}
.form {
  // .form__label
  &__label {
    margin-top: 16px;
    font-style: normal;
    font-weight: 400;
    font-size: 10px;
    line-height: 13px;
    letter-spacing: -0.02em;
    color: #49485E;
    span {
      background: #FF8484;
      border-radius: 4px;
      width: 4px;
      height: 4px;
      margin-left: 5px;
      position: absolute;
    }
  }
  // .form__btn
  &__btn {
    font-family: 'Inter';
    font-style: normal;
    font-weight: 600;
    font-size: 12px;
    line-height: 15px;
    text-align: center;
    letter-spacing: -0.02em;
    background: #7BAE73;
    border-radius: 10px;
    padding: 11px 0;
    margin-top: 24px;
    color: white;
    transition: 0.4s all ease;
    &:hover:not(.disabled) {
      transition: 0.4s all ease;
      background: #84d378;
    }

    &.disabled {
      background: #EEEEEE;
      color: #B4B4B4;
      transition: 0.4s all ease;
    }
  }
}
.danger-error {
  margin-top: 4px;
  color: red;
  font-size: 10px;
}
.danger-input {
  border: 1px solid red;
}
input[type='number'] {
    -moz-appearance:textfield;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
}
</style>