<template>
  <v-container>
    <v-row class="text-center">
      <v-card class="mx-auto my-12">    
        <v-data-table
        :headers="headers"
        :items="cardDetails"
        ></v-data-table>
      </v-card>  
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'CardDetailsView',
    data() {
        return {
            headers:[],
        }
    },
    computed:{
        cardDetails(){
            this.createTableHeaders();
            return this.capturedCardDetails
        }
    },
    props:{
        capturedCardDetails:Array        
    },
    methods: {
        createTableHeaders(){
            this.headers = [
                { text: 'Card Number', align: 'start', sortable: false,value: 'cardNumber'},
                { text: 'Card Holder', value: 'cardHolder'},
                { text: 'CVV', value: 'cardCVV'},
                { text: 'Expiry Date', value: 'expiryDate'},
                { text: 'Country', value: 'selectedCountry'}];
        },
        showData(){
            this.createTableHeaders();
            console.log(this.cardDetails);

        },
    },
    onMounted() {
        this.createTableHeaders();
    }      
  }
</script>