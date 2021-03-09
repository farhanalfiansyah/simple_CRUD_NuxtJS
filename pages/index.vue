<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-alert type="success" v-model="alert" :dismissible="true">
        Data Sudah Tersimpan
      </v-alert>
      <v-card>
        <v-card-title class="headline">
          Registrasi Pelanggan
        </v-card-title>
        <v-card-text>
          <v-text-field
            label="Nama"
            v-model="registrasi.name"
            hint="Alexander Graham Bell"
            persistent-hint
            outlined
            :error-messages="registrasi.error"
          ></v-text-field>
          <v-text-field
            label="Alamat"
            v-model="registrasi.address"
            :error-messages="registrasi.error"
            hint="Jalan Kenangan Blok C1 Nomor 4"
            persistent-hint
            outlined
          ></v-text-field>
          <v-row>
            <v-col>
              <v-file-input
                color="blue darken-4"
                counter
                label="Upload Foto"
                multiple
                @change="uploadPhoto"
                placeholder="Select your files"
                prepend-icon="mdi-paperclip"
                outlined
                :show-size="1000"
                :error-messages="registrasi.error"
              >
                <template v-slot:selection="{ index, text }">
                  <v-chip
                    v-if="index < 2"
                    color="blue darken-4"
                    dark
                    label
                    small
                  >
                    {{ text }}
                  </v-chip>

                  <span
                    v-else-if="index === 2"
                    class="overline grey--text text--darken-3 mx-2"
                  >
                    +{{ files.length - 2 }} File(s)
                  </span>
                </template>
              </v-file-input>
            </v-col>
            <v-col>
              <v-file-input
                color="blue darken-4"
                @change="uploadKTP"
                counter
                label="Upload KTP"
                multiple
                placeholder="Select your files"
                prepend-icon="mdi-paperclip"
                outlined
                :show-size="1000"
                :error-messages="registrasi.error"
              >
                <template v-slot:selection="{ index, text }">
                  <v-chip
                    v-if="index < 2"
                    color="blue darken-4"
                    dark
                    label
                    small
                  >
                    {{ text }}
                  </v-chip>

                  <span
                    v-else-if="index === 2"
                    class="overline grey--text text--darken-3 mx-2"
                  >
                    +{{ files.length - 2 }} File(s)
                  </span>
                </template>
              </v-file-input>
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <v-text-field
                label="Tempat Lahir"
                v-model="registrasi.place_of_birth"
                hint="Las Vegas"
                persistent-hint
                outlined
                :error-messages="registrasi.error"
              ></v-text-field>
            </v-col>
            <v-col>
              <v-menu
                ref="menu1"
                v-model="menu1"
                :close-on-content-click="false"
                transition="scale-transition"
                offset-y
                max-width="290px"
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="registrasi.date_of_birth"
                    outlined
                    label="Tanggal Lahir"
                    hint="YYYY-MM-DD format"
                    persistent-hint
                    prepend-icon="mdi-calendar"
                    v-bind="attrs"
                    v-on="on"
                    :error-messages="registrasi.error"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="registrasi.date_of_birth"
                  no-title
                  @input="menu1 = false"
                ></v-date-picker>
              </v-menu>
            </v-col>
          </v-row>

        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            @click.prevent="validate()"
          >
            Registrasi
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>


export default {
  data (){
    return {
      date: new Date().toISOString().substr(0, 10),
      menu1: false,
      error:false,
      alert:false,
      registrasi:{
        error:[],
        name:'',
        address:'',
        place_of_birth:'',
        date_of_birth: new Date().toISOString().substr(0, 10),
        ktp: null,
        photo:null
      }
    }
  },
  methods: {
    async uploadPhoto(value){
      if (value.length > 0) {
        const file = value[0];
        const reader = new FileReader();
        reader.onload = (e)=>{
          this.registrasi.photo = e.target.result;
        }
        reader.readAsDataURL(file);
      }
    },
    async uploadKTP(value){

      if (value.length > 0) {
        const file = value[0];
        const reader = new FileReader();
        reader.onload = (e)=>{
          this.registrasi.ktp = e.target.result;
        }
        reader.readAsDataURL(file);
      }
    },
    async submit(){
      try {
        await this.$axios.$post(`http://localhost:8000/api/users`, this.registrasi)
        this.alert = true;
      } catch (error) {

      }
    },
    async validate(){
      console.log("Data Registrasi",this.registrasi);
      if(!this.registrasi.name){
        this.registrasi.error.push('Name Wajib Diisi');
      }
      if(!this.registrasi.address){
        this.registrasi.error.push('Address Wajib Diisi');
      }
      if(!this.registrasi.place_of_birth){
        this.registrasi.error.push('Tempat Lahir Wajib Diisi');
      }
      if(!this.registrasi.date_of_birth){
        this.registrasi.error.push('Tanggal Lahir Wajib Diisi');
      }
      if(!this.registrasi.photo){
        this.registrasi.error.push('Wajib Upload Foto');
      }
      if(!this.registrasi.ktp){
        this.registrasi.error.push('Wajib Upload KTP');
      }
      if (!this.error) {
        await this.submit();
      }
    }
  },
}
</script>
