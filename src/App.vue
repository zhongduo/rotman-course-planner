<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">

        To Beloved Caroline
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <CoursePlanner/>
    </v-main>
  </v-app>
</template>

<script>
import CoursePlanner from './components/CoursePlanner';

export default {
  name: 'App',

  components: {
    CoursePlanner,
  },

  data: () => ({
    //
  }),
};
</script>
