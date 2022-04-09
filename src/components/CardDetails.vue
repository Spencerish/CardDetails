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
              :v-model="cardNumber"
              :counter="10"
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
          :items="countries"
          :rules="selectCountryRules"
          label="Country"
          required
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
    name: 'HelloWorld',
    data: () => ({
      valid: true,
      datePickerMenuVisibility:false,
      cardNumber:1235467890,
      cardNumberRules: [v => /^\d+$/.test(v) || 'Please make sure the card number is valid', v => (v && v.length <= 16) || 'The card number must be 16 digits',],
      cardHolder: '',
      cardHolderRules: [v => !!v || 'Card holder details are required',v => (v && v.length <= 12) || 'Please enter a valid name'],
      cardCVV: '',
      cardCVVRules: [v => (v && v.length <= 4) || 'Please enter a valid CVV'],
      expiryDate:'',
      selectedCountry: null,
      countries: ['South Africa', 'Namibia', 'Lesotho'],
      selectCountryRules: [v => !!v || 'A country is required'],
    }),
    methods: {
      submit() {
        if(!this.$refs.frmCardDetails.validate()) return;
        console.log("cardNumber"+this.cardNumber);
        console.log("cardHolder"+this.cardHolder);
        console.log("cardCVV"+this.cardCVV);
        console.log("expiryDate"+this.expiryDate);
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