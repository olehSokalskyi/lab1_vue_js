<template>
  <v-app>
    <v-app-bar
      app
      fixed
      style="
        color: whitesmoke;
        background-image: url('https://picsum.photos/1920/1080?random');
        background-size: cover;
      "
    >
      <v-app-bar-nav-icon @click="drawer = !drawer">
        <v-icon>mdi-menu</v-icon>
      </v-app-bar-nav-icon>
      <v-app-bar-title>My Project</v-app-bar-title>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item v-for="item in items" :key="item.title" :to="item.to">
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="px-10">
      <router-view />
    </v-main>
  </v-app>
</template>

<script lang="ts">
export default {
  name: 'App',
  data: () => ({
    drawer: false,
    items: [
      { title: 'List', icon: 'mdi-format-list-checks', to: '/' },
      { title: 'About', icon: 'mdi-help-box', to: '/about' },
      { title: 'Student table', icon: 'mdi-table', to: '/students' },
    ],
    right: null,
  }),
}
</script>

<style>
/* Додаємо стилі для вирівнювання router-view */
v-main {
  display: flex;
  flex-direction: column;
  min-height: calc(100vh - 64px); /* Висота екрану мінус висота AppBar */
  padding: 16px;
  box-sizing: border-box;
}

router-view {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
