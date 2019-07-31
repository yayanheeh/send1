<template>

  <v-card flat>
    <v-card-title class="grey lighten-5">
      Prosesnya semi-otomatis.
      <br/> 1. Tambahkan nomor Anda untuk mengirim tautan
      <br/> 2. Tambahkan nomor untuk dikirim
      <br/> 3. Tulis pesan siaran
      <br/> 4. Klik kirim
      <br/> <span class="red--text">5. Anda harus 
   menerima pesan dengan LINK daftar siaran (Tidak tersedia)</span>
    </v-card-title>    
    <v-card-text>
      <v-text-field type="number" v-model="ownPhoneNumber" auto-focus placeholder="123456789" label="Nomor anda" :error-messages="errors.collect('ownPhoneNumber')"
        v-validate="'required|numeric'" data-vv-name="ownPhoneNumber" required></v-text-field>
      <v-container fluid pa-0>
        <v-layout wrap row align-center>
          <v-flex xs12>Daftar siaran</v-flex>
          <v-flex xs9 text-xs-center>
            <v-text-field type="number" v-model="phoneNumber" auto-focus placeholder="83851070xxx" label="Nomor" v-validate="'required|numeric'"
              data-vv-name="phoneNumber" required @keyup.enter="addPhone(phoneNumber)"></v-text-field>
          </v-flex>
          <v-flex xs3 text-xs-center>
            <v-btn flat @click.native="addPhone(phoneNumber)">
              tambah
            </v-btn>
          </v-flex>
          <v-flex xs12>
            <v-layout wrap row :key="i" v-for="(item, i) in arrayNumbers">
              <v-flex xs10>
                {{item}}
              </v-flex>
              <v-flex xs2>
                <v-btn icon @click.native="clearPhoneNumber(i)">
                  <v-icon>clear</v-icon>
                </v-btn>
              </v-flex>
            </v-layout>
          </v-flex>
          <v-flex xs12>
            <v-text-field v-model="message.text" label="Pesan" placeholder="Pesan text..." multi-line auto-grow></v-text-field>
          </v-flex>
        </v-layout>
      </v-container>
    </v-card-text>
    <v-card-actions text-xs-center>
      <v-spacer></v-spacer>
      <v-btn large class="primary" @click.native="advanceSendWhatsapp" dark center>
        <icon name="whatsapp" class="mr-2" scale="2" dark></icon>Kirim Pesan Massal</v-btn>
      <v-spacer></v-spacer>
    </v-card-actions>    
    <!-- </form> -->
  </v-card>
</template>
<script>
import { mapState } from "vuex";
export default {
  props: ["code"],
  data() {
    return {
      urls: [],
      ownPhoneNumber: null,
      phoneNumber: null,
      arrayNumbers: []
    };
  },
  computed: {
    ...mapState(["country", "message"])
  },
  methods: {
    addPhone(item) {
      if (item) {
        this.arrayNumbers.push(item);
        this.phoneNumber = null;
      }
    },
    clearPhoneNumber(i) {
      this.arrayNumbers.splice(i, 1);
    },

    advanceSendWhatsapp() {
      this.arrayNumbers.forEach(element => {
        let url = `https://api.whatsapp.com/send?phone=${
          this.country.default.callingCode
        }${element}&text='${this.message.text}'`;

        this.urls.push(url);
      });
      this.urls = this.urls.join(", ");
      console.log(this.urls);
      window.open(
        `https://api.whatsapp.com/send?phone=${
          this.country.default.callingCode
        }${this.ownPhoneNumber}&text='Mensajes${this.urls}'`,
        "_blank"
      );
    }
  }
};
</script>
