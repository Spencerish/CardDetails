<template>
  <v-container>
    <v-row class="text-center">
      <v-card class="mx-auto my-12">
        <v-card-title class="text-center">Ban Country</v-card-title>
        <v-divider class="mx-4"></v-divider>
        <v-card-text>
          <v-form
            ref="frmCardDetails"
            v-model="valid"
            lazy-validation
          >      

        <v-select
          v-model="selectedCountry"
          :items="countries"
          :rules="selectCountryRules"
          label="Country"
          required
        ></v-select>

        <v-checkbox
        v-model="validStatus"
          input-value="true"
          value
          label="Ban"
          disabled
        >
        </v-checkbox>

            <v-btn
            :disabled="!valid"
            color="success"
            class="mr-4"
            @click="submit"
            >
            Submit
            </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'CountryCreate',
    data() {
        return {
          valid: true,
          validStatus:true,
          selectedCountry: null,
          countries: ['South Africa', 'Namibia', 'Lesotho'],
          selectCountryRules: [v => !!v || 'A country is required'],
        }
    },
    methods: {
      submit() {
        if(!this.$refs.frmCardDetails.validate()) return;
        let capturedCountry = {
          validStatus: this.validStatus,
          countryName: this.selectedCountry
        };
        this.$emit('capturedCountryDetails',capturedCountry);
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