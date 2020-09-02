<template>
  <div>
    <HeaderTemplate></HeaderTemplate>
    <v-container>
      <v-card class="mx-auto" max-width="800">
        <v-img
          class="white--text align-end"
          height="200px"
          src="https://cdn.vuetifyjs.com/images/cards/docks.jpg">
          <v-card-title>Blog Site</v-card-title>
        </v-img>
        <v-card-text class="text--primary">
          <div>Blog site front for blog management</div>
          <div>This was done in Vue.js and Vuetify as the UI compoment</div>
        </v-card-text>
      </v-card>
    </v-container>
    <FooterTemplate></FooterTemplate>
  </div>
</template>

<script>
import HeaderTemplate from "../components/Header.vue";
import FooterTemplate from "../components/Footer.vue";
export default {
  name: "About",
  components: {
    HeaderTemplate,
    FooterTemplate,
  },
};
</script>

<style>
</style>