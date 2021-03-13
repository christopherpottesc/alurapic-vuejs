<template>
  <nav>
      <ul>
        <li v-for="rota, i in rotas" :key="i">
          <router-link :to="rota.path ? rota.path : '/'">
            {{ rota.titulo }}
          </router-link>
        </li>
      </ul>
    </nav>
</template>

<script>
export default {
  props: {
    rotas: {
      required: true,
      type: Array,
      default: () => []
    }
  }
}
</script>

<style scoped>

</style>