<template>
  <v-app>
    <appbar/>
  </v-app>
</template>

<script>
import appbar from './components/appbar';

export default {
  name: 'App',

  components: {
    appbar,
  },

  data: () => ({
    //
  }),
};
</script>
