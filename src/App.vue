<template>
  <v-app>
    <v-main>
      <AgeFilter/>
    </v-main>
  </v-app>
</template>

<script>
import AgeFilter from './components/AgeFilter.vue';

export default {
  name: 'App',

  components: {
    AgeFilter,
  },

  data: () => ({
    //
  }),
}
</script>
