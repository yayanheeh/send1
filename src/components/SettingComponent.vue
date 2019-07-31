<template>
  <v-layout wrap row align-center>
    <v-flex xs12 >
      <span class="subheading primary--text">Simpan Nilai</span>
    </v-flex>
    <v-flex xs8>
      <v-autocomplete class="subheading" autocomplete :items="code" item-value="callingCode" return-object v-model="country.default" label="Negara default"
        item-text="name"  :error-messages="errors.collect('country')" v-validate="'required'" data-vv-name="country"
        required></v-autocomplete>
    </v-flex>
    <v-flex xs4>
      <v-btn flat @click.native="saveCountry" class="primary--text">Simpan</v-btn>
    </v-flex>
    <v-flex xs12>
    </v-flex>
    <v-flex xs8>
      <v-textarea v-model="message.text" label="pesan default" placeholder="Hello ..." multi-line auto-grow></v-textarea>
    </v-flex>
    <v-flex xs4>
      <v-btn flat guardar class="primary--text" @click.native="saveMessage"> Simpan</v-btn>
    </v-flex>
    
  </v-layout>
</template>
<script>
import { mapState } from "vuex";
export default {
  props: ["code"],
  data() {
    return {
      m: null
    };
  },
  computed: {
    ...mapState(["message", "country"])
  },
  methods: {
    saveCountry() {
      this.$store.commit("SET_COUNTRY", this.country);
    },
    saveMessage() {
      this.$store.commit("SET_MESSAGE", this.message);
    }
  }
};
</script>

