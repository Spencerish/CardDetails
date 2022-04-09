<template>
  <v-app>
    <v-main>
      <CardDetails/>
    </v-main>
  </v-app>
</template>

<script>
import CardDetails from './components/CardDetails';

export default {
  name: 'App',
  components: {
    CardDetails,
  }
};
</script>
