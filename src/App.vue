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
        alert("Duplicate Card Number");
      }else{
        this.allCapturedCardDetails.push(card);
      }
    },
    addToallCapturedCountries(country){
      if(this.duplicateBannedCountryCheck(country)){
        alert("Country has already been banned");
      }else{
      this.allCapturedCountries.push(country)
      }      
    },
    duplicateCardCheck(card){
      for (let index = 0; index < this.allCapturedCardDetails.length; index++) {
        if(card.cardNumber == this.allCapturedCardDetails[index].cardNumber){
          return true;
        } 
      }
      return false;      
    },
    duplicateBannedCountryCheck(country){
      for (let index = 0; index < this.allCapturedCountries.length; index++) {
        if(country.countryName == this.allCapturedCountries[index].countryName){
          return true;
        } 
      }
      return false;      
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
