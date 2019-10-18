<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Newcomers</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <v-toolbar-title class="headline text-uppercase">
        <span class="font-weight-light">introduction form</span>
      </v-toolbar-title>
    </v-app-bar>

    <v-content>
      <Form />
    </v-content>
  </v-app>
</template>

<script>
import Form from "./components/Form"

export default {
  name: "App",
  components: {
    Form
  },
  data: () => ({
    //
  })
}
</script>
