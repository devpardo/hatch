<template>
  <v-app>
    <v-main class="grey lighten-3">
      <TableStepper/>
    </v-main>
  </v-app>
</template>

<script>
import TableStepper from './components/TableStepper';

export default {
  name: 'App',

  components: {
    TableStepper,
  },

  data: () => ({
    //
  }),
};
</script>