<template>
  <div>
    <b-jumbotron>
      <template v-slot:header>ANIME ARSENAL</template>
      <template v-slot:lead>Search for your favorite anime</template>
      <hr class="my-4" />

      <b-row align-h="center">
        <b-col cols="2">
          <b-form-input v-model="keyword" placeholder="Enter Anime name"></b-form-input>
        </b-col>
        <b-col cols="4" md="auto">
          <b-button @click="searchdata()">Search Anime</b-button>
        </b-col>
      </b-row>
    </b-jumbotron>

    <b-container v-if="load==true">
      <b-row>
      <b-col md="6" class="my-1">
        <b-pagination v-model="currentPage" :total-rows="resultData.length" :per-page="perPage" first-text="First" prev-text="Prev" next-text="Next" last-text="Last"></b-pagination>
      </b-col>
    </b-row>
      <b-card-group columns>
        <b-row>
          <b-col >
            <b-card
              no-body
              class="overflow-hidden"
              style="max-width: 540px;"
              :per-page="perPage"
              :current-page="currentPage"
               v-for="data in resultData.slice((currentPage-1)*perPage,(currentPage-1)*perPage+perPage)" :key="data.mal_id"
            >
              <b-row no-gutters>
                <b-col md="4">
                  <a :href="data.url"  target="_blank"><b-card-img :src="data.image_url" :alt="data.mal_id" class="rounded-0"></b-card-img></a>
                Episode:{{data.episodes}}
                rated:{{data.rated}}
                </b-col>
                <b-col md="8">
                  <b-card-body :title="data.title">
                    <b-card-text>{{data.synopsis}}</b-card-text>
                  </b-card-body>
                </b-col>
              </b-row>
            </b-card>
          </b-col>
        </b-row>
        
      </b-card-group>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";


export default {
  data: function () {
    return {
      resultData: null,
      keyword: "",
      load:false,
      perPage:6,
      currentPage:1,
      totalRows:0
  
    };
  },
  methods: {
    searchdata() {
      axios
        .get(
          "https://api.jikan.moe/v3/search/anime?q=" + this.keyword + "&page=1"
        )
        .then(
          (response) => (
            (this.resultData = response.data.results),
            (this.load = true),
            (this.currentPage=1)
          )
        )
        .catch((error) => console.log(error));
    }



}
}
</script>




<style>
</style>