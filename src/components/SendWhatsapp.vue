<template>
  <form v-on:submit.prevent="sendWhatsapp">
    <v-card flat>
      <v-card-text>
        <v-autocomplete autocomplete :items="code" item-value="callingCode" v-model="country.default" label="Negara"
          item-text="name" :error-messages="errors.collect('country')" v-validate="'required'" data-vv-name="country"
          required return-object></v-autocomplete>
      </v-card-text>
      <v-card-text>
        <v-text-field type="number" v-model="phoneNumber" auto-focus placeholder="0838510703xx" label="Nomor telepon"
          :error-messages="errors.collect('phoneNumber')" v-validate="'required|numeric'" data-vv-name="phoneNumber"
          required></v-text-field>
      </v-card-text>
      <v-card-text>
        <v-textarea v-model="message.text" :label="label" placeholder="Pesan text ..." auto-grow></v-textarea>
      </v-card-text>
      <v-card-actions>
        <v-layout wrap row text-xs-center>
          <v-flex xs6 offset-xs3>
            <v-btn block class="primary" type="submit" dark center>
              <v-icon class="mr-2" dark>fab fa-whatsapp</v-icon>Kirim
            </v-btn><br/>
<center>© 2019 -
          <a href="https://instagram.com/kres_008" target="_blank" class="primary--text">Kres_008</a>
      </center>

          </v-flex>
        </v-layout>
      </v-card-actions>
    </v-card>
  </form>
</template>
<script>
  import {
    mapState
  } from "vuex";
  export default {
    $_veeValidate: {
      validator: "new"
    },
    props: ["code"],
    data() {
      return {
        phoneNumber: null
      };
    },
    computed: {
      label() {
        return `Kode negara +${this.country.default.callingCode} ${
        this.phoneNumber ? this.phoneNumber : ""
      }`;
      },
      ...mapState(["country", "message"])
    },
    methods: {
      sendWhatsapp() {
        /* this is the magic */
        window.open(
          `https://api.whatsapp.com/send?phone=${
          this.country.default.callingCode
        }${this.phoneNumber}&text=${this.message.text}`,
          "_blank"
        );
      }
    }
  };
</script>
