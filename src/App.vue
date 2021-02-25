<template>
  <v-app id="mainweb">
    <div class="header-nav">
      <div><router-link to="/">Home</router-link></div>
      <div><router-link to="/markets">Markets</router-link></div>
    </div>
    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>
<style lang="scss">
.header-nav {
  display: flex;
  gap: 10px;
  font-size: 24px;
  height: 70px;
  background: black;
  align-items: center;
  padding: 0px 5%;
  width: 100%;
  justify-content: flex-start;
}
</style>
