<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <span class="mr-2">Sort visualizer</span>
      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/pierre-vigier/sorting-viz"
        target="_blank"
        text
      >
        <span class="mr-2">Github</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <SortVisualizer />
    </v-content>
  </v-app>
</template>

<script>
import SortVisualizer from "./components/SortVisualizer";

export default {
  name: "App",

  components: {
    SortVisualizer
  },

  data: () => ({
    //
  })
};
</script>
