<template>
  <v-app>
    <v-app-bar
      app
    >
      <div class="d-flex align-center">
        <div class="text-sm-h5 black--text">Рассчитать дешевый маршрут</div>
      </div>

    </v-app-bar>

    <v-main>
      <v-container>
        <v-row>
          <v-col sm="4">
            <v-card>
              <Form
                  :countries="routeObj.data.country"
                  @changedSide="proxySide"
              />
            </v-card>
          </v-col>
          <v-col sm="8">
            <v-card>
              <Routes
                  :side="side"
                  :routes="routeObj.data.company"
              />
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Routes from './components/Routes.vue';
import Form from "./components/Form";
import routeObj from '../call-paths.json';

export default {
  name: 'App',

  components: {
    Routes,
    Form
  },

  data: () => ({
    routeObj,
    side: null
  }),
  methods: {
    proxySide(eventObj){
      this.side = eventObj;
    }
  }
};
</script>
