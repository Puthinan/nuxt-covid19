<template>
  <v-row>
    <v-col md="3">
      <v-card color="orange">
        <v-card-title text-md-center class="display-1 font-weight-bold">
          {{ post.Confirmed }} (
          <v-icon>mdi-chevron-up</v-icon>
          {{post.NewConfirmed}})
        </v-card-title>
        <v-card-subtitle class="title">ผู้ป่วยสะสม</v-card-subtitle>
      </v-card>
    </v-col>
    <v-col md="3">
      <v-card color="teal">
        <v-card-title class="display-1 font-weight-bold">{{ post.Hospitalized }}</v-card-title>
        <v-card-subtitle class="title">
          <v-icon>mdi-hospital-building</v-icon>กำลังรักษา
        </v-card-subtitle>
      </v-card>
    </v-col>
    <v-col md="3">
      <v-card color="success">
        <v-card-title class="display-1 font-weight-bold">{{ post.Recovered }}</v-card-title>
        <v-card-subtitle class="title">
          <v-icon>mdi-dumbbell</v-icon>รักษาหาย
        </v-card-subtitle>
      </v-card>
    </v-col>
    <v-col md="3">
      <v-card color="red">
        <v-card-title class="display-1 font-weight-bold">{{ post.Deaths }}</v-card-title>
        <v-card-subtitle class="title">เสียชีวิต</v-card-subtitle>
      </v-card>
    </v-col>
    <v-row>
      <v-col md="12">
        <v-data-table :headers="headers" :items="poststimeline.Data" :items-per-page="10" class="elevation-1"></v-data-table>
      </v-col>
    </v-row>
  </v-row>
</template>



<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";

export default {
  async fetch() {
    this.post = await this.$http.$get(
      "https://covid19.th-stat.com/api/open/today"
    );
    this.posts = await this.$http.$get(
      "https://covid19.th-stat.com/api/open/cases/sum"
    );
    this.poststimeline = await this.$http.$get("https://covid19.th-stat.com/api/open/timeline");
  },
  data() {
    return {
      post: {},
      posts: {},
      poststimeline: [],
      headers: [
          {
            align: 'start',
            sortable: true,
            value: 'name',
          },
          { text: 'วันที่', value: 'Date' },
          { text: 'จำนวนเคส', value: 'NewConfirmed' },
          { text: 'จำนวนรักษาหาย', value: 'NewRecovered' },
          { text: 'จำนวนที่รักาษาอยู่', value: 'NewHospitalized' }
        ],
    };
    console.log(this.posts);
  },
  components: {
    Logo,
    VuetifyLogo
  }
};
</script>

