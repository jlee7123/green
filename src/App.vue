<template>
  <v-app>
    <div>
      <v-spacer></v-spacer>
      
        <AppBar/>

    <v-main>
      <Header></Header>
      <router-view/>
      <Footer></Footer>
    </v-main>
    </div>
  </v-app>
</template>

<script>
import Header from './layout/Header';
import Footer from './layout/Footer';
//import AppBar from './components/AppBar.vue'

export default {
  name: 'App',
  components: {
    //AppBar,
    Header,
    Footer,
  },

  data: () => ({

  }),
};
</script>





