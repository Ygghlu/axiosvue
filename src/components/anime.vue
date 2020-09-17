<template>
  <div>
    <b-form-input v-model="keyword"  placeholder="Enter Anime name"></b-form-input>
    <b-button @click="searchdata()">Search Anime</b-button>
    <!-- {{resultData}} -->
    <div >
      <b-card-group columns>
        <b-card no-body class="overflow-hidden" style="max-width: 540px;" v-for="data in resultData" :key="data.mal_id">
          <b-row no-gutters>
            <b-col md="6">
              <b-card-img
                :src="data.image_url"
                :alt="data.mal_id"
                class="rounded-0"
              ></b-card-img>
            </b-col>
            <b-col md="6">
              <b-card-body :title="data.title">
                <b-card-text>
                  {{data.synopsis}}
                </b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>

      </b-card-group>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      resultData: null,
      keyword: ""
    };
  },
  methods: {
    searchdata() {
      axios
        .get("https://api.jikan.moe/v3/search/anime?q="+this.keyword+"&page=1")
        .then((response) => (this.resultData = response.data.results))
        .catch((error) => console.log(error));
    },
  },
};
</script>




<style>
</style>