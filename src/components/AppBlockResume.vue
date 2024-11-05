<template>
  <div class="card resume">
    <h2 v-if="resumeElements === 0">Добавьте первый блок, чтобы увидеть результат</h2>
  </div>
</template>

<script>
export default {
  props: ['resumeElements']
}
</script>

<style lang="scss" scoped>

</style>
