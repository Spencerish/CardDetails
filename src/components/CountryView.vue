<template>
  <v-container>
    <v-row class="text-center">
      <v-card class="mx-auto my-12">    
        <v-data-table
        :headers="headers"
        :items="countryDetails"
        ></v-data-table>
      </v-card>  
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'CountryView',
    data() {
        return {
            headers:[],
        }
    },
    computed:{
        countryDetails(){
            this.createTableHeaders();
            return this.capturedCountryDetails
        }
    },
    props:{
        capturedCountryDetails:Array
    },
    methods: {
        createTableHeaders(){
            this.headers = [
                { text: 'Name', value: 'countryName'},
                { text: 'Valid Status', value: 'validStatus'}]
        },
        showData(){
        },
    },
    onMounted() {
        this.createTableHeaders();
    }      
  }
</script>