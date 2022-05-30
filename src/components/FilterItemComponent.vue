<template>
  <select v-model="selected" class="filter" :class="{open: true, close: false}" @change="$emit(selected)">
    <option value="default">По умолчанию</option>
    <option value="toMax">По возрастанию</option>
    <option value="toMin">По убыванию</option>
    <option value="toAlph">По алфавиту</option>
  </select>
</template>

<script>
  export default {
    data(){
      return {
        selected: 'default',
      }
    },
  }
</script>

<style lang="scss" scoped>
.filter {
  padding: 10px 5px 10px 16px;
  align-items: center;
  text-align: center;
  background: #FFFEFB;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  font-style: normal;
  font-weight: 400;
  font-size: 12px;
  line-height: 15px;
  color: #B4B4B4;
  outline: none;
  border: none;
  transition: 0.5s ease-in;
  &:focus {
    transition: 0.5s ease-in;
  }
  option {
    color: black;
    text-align: left;
    border: none;
    background: #FFFEFB;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: 0.5s ease-in;
  }
}

</style>