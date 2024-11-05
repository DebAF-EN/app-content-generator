<template>
  <button
    class="btn primary"
    type="button"
    @click="$emit('loadComments')"
    v-if="users.length === 0"
  >Загрузить комментарии</button>
  <div class="card" v-else>
    <div class="comment" v-for="(user, index) in users" :key="index">
      <h3 class="email">{{ user.email }}</h3>
      <p>{{ user.comment }}</p>
      <hr v-if="index < users.length - 1" />
    </div>
  </div>
</template>

<script>
export default {
  props: ['users'],
  emits: ['loadComments']
}
</script>

<style scoped>

</style>
