<template>
  <v-app>
    <v-main>
      <v-btn @click="formStateHandler">
         {{buttonText}} 
      </v-btn>
      <v-row>
        <v-col>
            <CardDetailsCreate
              v-on:capturedCardDetails="addToallCapturedCardDetails"
            />
        </v-col>
        <v-col>
            <CardDetailsView
              v-bind:capturedCardDetails=allCapturedCardDetails
            />
        </v-col>        
      </v-row>
      <v-row>
        <v-col>

        </v-col>
        <v-col>

        </v-col>        
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import CardDetailsCreate from './components/CardDetailsCreate';
import CardDetailsView from './components/CardDetailsView'

export default {
  name: 'App',
  data() {
    return {
      buttonText: 'Show Countries',
      allCapturedCardDetails: [],
      state: { countryView: false, cardView: true,}
    }
  },
  methods: {
    addToallCapturedCardDetails(card){
      this.allCapturedCardDetails.push(card);
    },
    formStateHandler(){
      this.state.countryView = !this.state.countryView;
      this.state.cardView = !this.state.cardView;
      if(this.state.cardView) this.buttonText = 'Show Countries'
      if(this.state.countryView) this.buttonText = 'Show Card Details'      
    }
  },
  components: {
    CardDetailsCreate,
    CardDetailsView
  }
};
</script>
