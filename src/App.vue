<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        
      </div>

      <v-spacer></v-spacer>
      

    </v-app-bar>

    <v-main>
      <!-- <HelloWorld/> -->
      <Weather/>
      <Calander/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import Weather from './components/Weather';
import Calander from './components/Calander';

export default {
  name: 'App',

  components: {
    HelloWorld,
    Weather,
    Calander,
    Weather,
  },

  data: () => ({
    //
  }),
};
</script>
