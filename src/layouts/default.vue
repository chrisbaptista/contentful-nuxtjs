<template>
  <v-app>
    <HeaderComponent></HeaderComponent>
    <v-main>
      <Nuxt/>
    </v-main>
  </v-app>
</template>

<script>
import HeaderComponent from "~/components/layout/HeaderComponent";

export default {
  components: {
    HeaderComponent
  }
};
</script>