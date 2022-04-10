<template>
  <v-container fluid>
    <v-row class="text-center">
      <v-card class="mx-auto my-12" min-width=350px>
        <v-card-title class="text-center">Ban Country</v-card-title>
        <v-divider class="mx-4"></v-divider>
        <v-card-text>
          <v-form
            ref="frmCountryDetails"
            v-model="valid"
            lazy-validation
          >      
        <v-select
          v-model="selectedCountry"
          :items="countries"
          :rules="selectCountryRules"
          label="Country"
          required
          dense 
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
      selectCountryRules: [v => !!v || 'A country is required'],
    }
  },
  computed:{
    countries(){
      return this.allCountries
    }
  },
  props:{
    allCountries:Array
  },  
  methods: {
    submit() {
      if(!this.$refs.frmCountryDetails.validate()) return;
      let capturedCountry = {
        validStatus: this.validStatus,
        countryName: this.selectedCountry
      };
      this.$emit('capturedCountryDetails',capturedCountry);
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