<template>
  <v-container>
    <v-row class="text-center">
      <v-card class="mx-auto my-12">
        <v-img
          :src="require('../assets/debit_card.png')"
          class="my-3"
          contain
          height="250"
        />
        <v-card-title class="text-center">Capture Card Details</v-card-title>
        <v-divider class="mx-4"></v-divider>
        <v-card-text>
          <v-form
            ref="frmCardDetails"
            v-model="valid"
            lazy-validation
          >      
            <v-text-field
              v-model="cardNumber"
              :rules="cardNumberRules"
              label="Card Number"
              required
            ></v-text-field>

            <v-text-field
              v-model="cardHolder"
              :rules="cardHolderRules"
              label="Card Holder"
              required
            ></v-text-field>

          <v-menu
            ref="menu"
            v-model="datePickerMenuVisibility"
            :close-on-content-click="false"
            :return-value.sync="expiryDate"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="auto"
            v-on="on"
          >
            <template v-slot:activator="{on}">
              <v-row no-gutters>
                <v-col class="pa-2">
                  <v-text-field
                    v-model="expiryDate"
                    label="Expiry Date"
                    prepend-icon="mdi-calendar"
                    width=200px           
                    v-on="on"
                    :rules="expiryDateRules"
                    required
                  ></v-text-field>
                </v-col>
                <v-col class="pa-2">
                  <v-text-field
                    v-model="cardCVV"
                    :rules="cardCVVRules"
                    label="CVV"
                    required
                  ></v-text-field>
                </v-col>            
              </v-row>
            </template>
            <v-date-picker
              v-model="expiryDate"
              type="month"
              no-title
              scrollable
            >
              <v-spacer></v-spacer>
              <v-btn color="primary" @click="closeMenu">Cancel</v-btn>
              <v-btn color="primary" @click="selectDate">OK</v-btn>
            </v-date-picker>
          </v-menu>

        <v-select
          v-model="selectedCountry"
          :items="allCountries"
          :rules="selectCountryRules"
          label="Country"
          required
          dense
        ></v-select>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="submit"
        >
          Submit
        </v-btn>

        <v-btn
          color="error"
          class="mr-4"
          @click="cancel"  
        >
          Clear
        </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'CardDetailsCreate',
  data() {
    return {
      on:false,
      valid: true,
      datePickerMenuVisibility:false,
      cardNumber:null,
      // Using a regex pattern to check for most common card types
      cardNumberRules: [v => /^(?:4[0-9]{12}(?:[0-9]{3})?|[25][1-7][0-9]{14}|6(?:011|5[0-9][0-9])[0-9]{12}|3[47][0-9]{13}|3(?:0[0-5]|[68][0-9])[0-9]{11}|(?:2131|1800|35\d{3})\d{11})$/.test(v) || 'Please make sure the card number is valid'],
      cardHolder: '',
      cardHolderRules: [v => !!v || 'Card holder details are required',v => (v && v.length <= 12) || 'The name is too long'],
      cardCVV: '',
      cardCVVRules: [v => !!v || 'CVV is required', v => /^\d+$/.test(v) || 'CVV should be a number', v => (v && v.length == 4 || v && v.length == 3) || 'Please enter a valid CVV'],
      expiryDate:'',
      expiryDateRules:  [v => !!v || 'Please enter an expiry date'],
      selectedCountry: null,
      selectCountryRules: [v => !!v || 'A country is required']
    }
  },
  props:{
    allCountries:Array
  },  
  computed: {
    countries(){
      return this.allCountries
    }
  },
  methods: {
    submit() {
      if(!this.$refs.frmCardDetails.validate()) return;
      let capturedCardDetail={
        cardNumber:this.cardNumber,
        cardHolder:this.cardHolder,
        cardCVV:this.cardCVV,
        expiryDate:this.expiryDate,
        selectedCountry: this.selectedCountry
      };
      this.$emit('capturedCardDetails',capturedCardDetail);
    },
    cancel() {
      this.$refs.frmCardDetails.reset()
    },
    closeMenu(){
      this.datePickerMenuVisibility = false
    },
    selectDate(){
      this.$refs.menu.save(this.expiryDate)
    }
  },      
}
</script>