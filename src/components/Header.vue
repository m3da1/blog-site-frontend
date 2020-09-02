<template>
  <div>
  <v-app-bar>
      <v-toolbar-title>My Blog Site</v-toolbar-title>
      <v-spacer></v-spacer>
      <span class="hidden-sm-and-up">
        <v-btn @click.stop="drawer = !drawer">Menu</v-btn>
      </span>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn to="/" text>
          <v-icon small left>fa-home</v-icon>Home
        </v-btn>
        <v-btn to="about" text>
          <v-icon small left>fa-info-circle</v-icon>About
        </v-btn>
      </v-toolbar-items>
  </v-app-bar>
  <v-navigation-drawer v-model="drawer" absolute temporary right>
      <v-list>
        <v-list-item v-for="item in items" v-bind:key="item.title" v-bind:to="item.link" link>
          <v-list-item-icon>
            <v-icon small>fa-{{item.icon}}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
              <v-list-item-title>{{item.title}}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
    name: "Header",
    data() {
        return {
      drawer: false,
      items: [
        { title: "Home", link: "/", icon: "home" },
        { title: "About", link: "about", icon: "info-circle" }
      ],
    };
    }
}
</script>

<style>

</style>