<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
    <h2>JSAU-WEBAPP</h2>
     
      <v-spacer></v-spacer>

     

    </v-app-bar>

    <v-main>
    
                   <v-container>
                       <!-- Modal Add New Product -->
            

<br>      
<HelloWorld/>

         </v-container>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';
//import axios from 'axios'
export default {
  
  name: 'App',

  components: {
    HelloWorld,
    
  },



    
};
</script>
