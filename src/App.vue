<template>
  <v-app
    id="mainweb"
    :class="{
      'xl-main-screen': $vuetify.breakpoint.xlOnly,
      'lg-main-screen': $vuetify.breakpoint.lgOnly,
      'md-main-screen': $vuetify.breakpoint.mdOnly,
      'sm-main-screen': $vuetify.breakpoint.smOnly,
      'xs-main-screen': $vuetify.breakpoint.xsOnly,
    }"
  >
    <div>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      <router-link to="/markets">About</router-link>
    </div>
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>
<style lang="scss">
</style>
