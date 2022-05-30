<template>
  <h1 class="page__title">{{text}}</h1>
</template>

<script>
  export default {
    props: ['text']
  }
</script>

<style lang="scss" scoped>
.page__title {
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;
  color: #3F3F3F;
}

</style>