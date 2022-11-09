<template>
  <v-app>
        <Header></Header>
        <AppBar/>
      <router-view/>
  </v-app>
</template>

<script>
import Header from './layout/Header';
//import Footer from './layout/Footer';
//import AppBar from './components/AppBar.vue'

export default {
  name: 'App',
  components: {
    //AppBar,
    Header,
    //Footer,
  },

  data: () => ({

  }),
};
</script>





