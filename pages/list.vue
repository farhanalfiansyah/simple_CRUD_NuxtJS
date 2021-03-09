<template>
  <v-card>
    <v-card-title>
      Registrations
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="registrations"
      item-key="Id"
      :search="search"
    >
      <template v-slot:item.photo="{ item }">
        <img style="width:17rem;" :src="item.photo" alt="" srcset="">
      </template>
      <template v-slot:item.ktp="{ item }">
        <img style="width:17rem;" :src="item.ktp" alt="" srcset="">
      </template>
    </v-data-table>
  </v-card>
</template>
<script>
export default {
  data () {
    return {
      search: '',
      headers: [
        {text: 'Id',
          align: 'center',
          sortable: false,
          value: 'id',},
        {
          text: 'Nama',
          align: 'center',
          sortable: false,
          value: 'name',
        },
        { text: 'Alamat', align: 'center', value: 'address' },
        { text: 'Tempat Lahir', align: 'center', value: 'place_of_birth' },
        { text: 'Tanggal Lahir', align: 'center', value: 'date_of_birth' },
        { text: 'Foto', align: 'center', value: 'photo' },
        { text: 'KTP', align: 'center', value: 'ktp' },
      ],
      registrations: [],
    }
  },
  mounted(){
    this.getRegistrations();
  },
  methods:{
    async getRegistrations(){
      try {
        const {data} = await this.$axios.$get(`http://localhost:8000/api/users`)
        this.registrations = data
      } catch (error) {
        console.log(error);

      }
    }
  }
}
</script>

