<template>
  <div class="home">
    <CreateNewWallet></CreateNewWallet>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import CreateNewWallet from "@/components/CreateNewWallet.vue"; // @ is an alias to /src

export default Vue.extend({
  name: "Home",
  components: {
    CreateNewWallet
  }
});
</script>
