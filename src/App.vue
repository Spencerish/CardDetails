<template>
  <v-app>
    <v-main>
      <v-btn @click="formStateHandler">
         {{buttonText}} 
      </v-btn>
      <v-row v-if="state.cardView">
      
        <v-col class="mx-auto">
            <CardDetailsCreate
              v-on:capturedCardDetails="addToallCapturedCardDetails"
              v-bind:allCountries="allCountries"
            />
        </v-col >
        <v-col class="mx-auto">
            <CardDetailsView
              v-bind:capturedCardDetails="allCapturedCardDetails"
            />
        </v-col>        
      </v-row>
      <v-row v-if="state.countryView">
        <v-col>
          <CountryCreate 
          v-on:capturedCountryDetails="addToallCapturedCountries"
          v-bind:allCountries="allCountries"
          />
        </v-col>
        <v-col 
          class="mx-auto"
        >
          <CountryView 
          v-bind:capturedCountryDetails="allCapturedCountries"/>
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
      allCountries: ["Afghanistan","Albania","Algeria","Andorra","Angola","Anguilla","Antigua &amp; Barbuda","Argentina","Armenia","Aruba","Australia","Austria","Azerbaijan","Bahamas","Bahrain","Bangladesh","Barbados","Belarus","Belgium","Belize","Benin","Bermuda","Bhutan","Bolivia","Bosnia &amp; Herzegovina","Botswana","Brazil","British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi","Cambodia","Cameroon","Cape Verde","Cayman Islands","Chad","Chile","China","Colombia","Congo","Cook Islands","Costa Rica","Cote D Ivoire","Croatia","Cruise Ship","Cuba","Cyprus","Czech Republic","Denmark","Djibouti","Dominica","Dominican Republic","Ecuador","Egypt","El Salvador","Equatorial Guinea","Estonia","Ethiopia","Falkland Islands","Faroe Islands","Fiji","Finland","France","French Polynesia","French West Indies","Gabon","Gambia","Georgia","Germany","Ghana","Gibraltar","Greece","Greenland","Grenada","Guam","Guatemala","Guernsey","Guinea","Guinea Bissau","Guyana","Haiti","Honduras","Hong Kong","Hungary","Iceland","India","Indonesia","Iran","Iraq","Ireland","Isle of Man","Israel","Italy","Jamaica","Japan","Jersey","Jordan","Kazakhstan","Kenya","Kuwait","Kyrgyz Republic","Laos","Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg","Macau","Macedonia","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Mauritania","Mauritius","Mexico","Moldova","Monaco","Mongolia","Montenegro","Montserrat","Morocco","Mozambique","Namibia","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand","Nicaragua","Niger","Nigeria","Norway","Oman","Pakistan","Palestine","Panama","Papua New Guinea","Paraguay","Peru","Philippines","Poland","Portugal","Puerto Rico","Qatar","Reunion","Romania","Russia","Rwanda","Saint Pierre &amp; Miquelon","Samoa","San Marino","Satellite","Saudi Arabia","Senegal","Serbia","Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","South Africa","South Korea","Spain","Sri Lanka","St Kitts &amp; Nevis","St Lucia","St Vincent","St. Lucia","Sudan","Suriname","Swaziland","Sweden","Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","Timor L'Este","Togo","Tonga","Trinidad &amp; Tobago","Tunisia","Turkey","Turkmenistan","Turks &amp; Caicos","Uganda","Ukraine","United Arab Emirates","United Kingdom","Uruguay","Uzbekistan","Venezuela","Vietnam","Virgin Islands (US)","Yemen","Zambia","Zimbabwe"],
      state: { countryView: false, cardView: true}
    }
  },
  mounted() {
    this.getLocalStorage();
  },   
  methods: {
    getLocalStorage(){
      if(localStorage.getItem('allCapturedCardDetails')) this.allCapturedCardDetails = JSON.parse(localStorage.getItem('allCapturedCardDetails'));
      if(localStorage.getItem('allCapturedCountries')) this.allCapturedCountries = JSON.parse(localStorage.getItem('allCapturedCountries'));       
    },
    addToallCapturedCardDetails(card){
      if(this.duplicateCardCheck(card)) return alert("Duplicate Card Number");
      if(this.countryHasBeenBanned(card)) return alert("The selected country is on the Ban List");
      this.allCapturedCardDetails.push(card);
      localStorage.setItem('allCapturedCardDetails', JSON.stringify(this.allCapturedCardDetails));
    },
    countryHasBeenBanned(card){
      for (let index = 0; index < this.allCapturedCountries.length; index++) {
        if(card.selectedCountry == this.allCapturedCountries[index].countryName) return true;
      }
      return false;       
    },
    addToallCapturedCountries(country){
      if(this.duplicateBannedCountryCheck(country)) return alert("Country has already been banned");
      this.allCapturedCountries.push(country)
      localStorage.setItem('allCapturedCountries', JSON.stringify(this.allCapturedCountries));
    },
    duplicateCardCheck(card){
      for (let index = 0; index < this.allCapturedCardDetails.length; index++) {
        if(card.cardNumber == this.allCapturedCardDetails[index].cardNumber) return true;
      }
      return false;      
    },
    duplicateBannedCountryCheck(country){
      for (let index = 0; index < this.allCapturedCountries.length; index++) {
        if(country.countryName == this.allCapturedCountries[index].countryName) return true;
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
}
</script>
