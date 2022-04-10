<template>
  <v-app>
    <v-main>
      <v-btn @click="formStateHandler">
         {{buttonText}} 
      </v-btn>
      <v-row v-if="state.cardView">
        <v-col>
            <CardDetailsCreate
              v-on:capturedCardDetails="addToallCapturedCardDetails"
            />
        </v-col>
        <v-col>
            <CardDetailsView
              v-bind:capturedCardDetails="allCapturedCardDetails"
            />
        </v-col>        
      </v-row>
      <v-row v-if="state.countryView">
        <v-col>
        <CountryCreate v-on:capturedCountryDetails="addToallCapturedCountries"/>
        </v-col>
        <v-col>
          <CountryView v-bind:capturedCountryDetails="allCapturedCountries"/>
        </v-col>        
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import CardDetailsCreate from './components/CardDetailsCreate';
import CardDetailsView from './components/CardDetailsView'
import CountryView from './components/CountryView'
import CountryCreate from './components/CountryCreate';

export default {
  name: 'App',
  components: {
    CardDetailsCreate,
    CardDetailsView,
    CountryView,
    CountryCreate
  },
  data() {
    return {
      buttonText: 'Show Countries',
      allCapturedCardDetails: [],
      allCapturedCountries:[],
      state: { countryView: false, cardView: true,}
    }
  },
  methods: {
    addToallCapturedCardDetails(card){
      if(this.duplicateCheck(card)){

      }
      this.allCapturedCardDetails.push(card)
    },
    addToallCapturedCountries(country){
      this.allCapturedCountries.push(country)
    },
    validateCardDetails(card){
      

    },
    formStateHandler(){
      this.state.countryView = !this.state.countryView;
      this.state.cardView = !this.state.cardView;
      if(this.state.cardView) this.buttonText = 'Show Countries'
      if(this.state.countryView) this.buttonText = 'Show Card Details'      
    }
  }
};
</script>
