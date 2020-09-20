<template>
  <div>
    <b-jumbotron>
      <img src="../assets/logo.png" alt />
      <template v-slot:header>ANIME ARSENAL</template>
      <template v-slot:lead>Search for your favorite anime</template>
      <b-row align-h="center">
        <b-col cols="3">
          <b-form-select v-model="searchmet" :options="searchOption"></b-form-select>
        </b-col>
      </b-row>

      <hr class="my-4" />

      <b-row align-h="center" v-if="searchmet==1">
        <b-col cols="2">
          <b-form-input v-model="keyword" placeholder="Enter Anime name"></b-form-input>
        </b-col>
        <b-col cols="4" md="auto">
          <b-button @click="searchdata()">Search Anime</b-button>
        </b-col>
      </b-row>
      <b-row align-h="center" v-if="searchmet==2">
        <b-col cols="2">
          <b-form-input v-model="year" placeholder="Enter year"></b-form-input>
        </b-col>
        <b-col cols="2">
          <b-form-select v-model="selected" :options="options"></b-form-select>
        </b-col>
        <b-col cols="4" md="auto">
          <b-button @click="searchSeason()">Search Anime</b-button>
        </b-col>
      </b-row>
      <div v-if="searchmet==3"></div>
    </b-jumbotron>

    <b-container v-if="load==true">
      <b-row>
        <b-col md="6" class="my-1">
          <b-pagination
            v-model="currentPage"
            :total-rows="resultData.length"
            :per-page="perPage"
            first-text="First"
            prev-text="Prev"
            next-text="Next"
            last-text="Last"
          ></b-pagination>
        </b-col>
      </b-row>
      <b-card-group columns>
        <b-card
          no-body
          class="overflow-hidden"
          style="max-width: 540px"
          :per-page="perPage"
          :current-page="currentPage"
          v-for="data in resultData.slice((currentPage-1)*perPage,(currentPage-1)*perPage+perPage)"
          :key="data.mal_id"
        >
          <b-row no-gutters>
            <b-col md="4">
              <a :href="data.url" target="_blank">
                <b-card-img :src="data.image_url" class="rounded-0"></b-card-img>
              </a>
              Episode:{{data.episodes}}
            </b-col>
            <b-col md="8">
              <b-card-body :title="data.title">
                <b-card-text>{{data.synopsis}}</b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>
      </b-card-group>
    </b-container>
    <iframe
      width="50%"
      height="450"
      scrolling="no"
      frameborder="no"
      allow="autoplay"
      src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/playlists/389256011%3Fsecret_token%3Ds-MUoH3D0TLhc&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"
    ></iframe>
    <div
      style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"
    >
      <a
        href="https://soundcloud.com/mikaru04"
        title="Mikaru04 (Ygghlu)"
        target="_blank"
        style="color: #cccccc; text-decoration: none;"
      >Mikaru04 (Ygghlu)</a> ·
      <a
        href="https://soundcloud.com/mikaru04/sets/liked-song/s-MUoH3D0TLhc"
        title="Liked song"
        target="_blank"
        style="color: #cccccc; text-decoration: none;"
      >Liked song</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      resultData: null,
      keyword: "",
      load: false,
      perPage: 6,
      currentPage: 1,
      totalRows: 0,
      year: "",
      selected: null,
      searchmet: null,
      options: [
        { value: null, text: "Please select season" },
        { value: "spring", text: "Spring" },
        { value: "summer", text: "Summer" },
        { value: "fall", text: "Fall" },
        { value: "winter", text: "Winter" },
      ],
      searchOption: [
        { value: null, text: "Please select search method" },
        { value: 1, text: "Search by name" },
        { value: 2, text: "Search by Year and Season" },
        { value: 3, text: "Upcoming Anime" },
      ],
    };
  },
  methods: {
    searchdata() {
      axios
        .get("https://api.jikan.moe/v3/search/anime?q=" + this.keyword)
        .then(
          (response) => (
            (this.resultData = response.data.results),
            (this.load = true),
            (this.currentPage = 1)
          )
        )
        .catch((error) => console.log(error));
    },
    searchSeason() {
      axios
        .get(
          "https://api.jikan.moe/v3/season/" + this.year + "/" + this.selected
        )
        .then(
          (response) => (
            (this.resultData = response.data.anime),
            (this.load = true),
            (this.currentPage = 1)
          )
        )
        .catch((error) => console.log(error));
    },
    upcoming() {
      axios
        .get("https://api.jikan.moe/v3/season/later")
        .then(
          (response) => (
            (this.resultData = response.data.anime),
            (this.load = true),
            (this.currentPage = 1)
          )
        )
        .catch((error) => console.log(error));
    },
  },
  watch: {
    searchmet: function (val) {
      if (val == 3) {
        this.upcoming();
      }
    },
  },
};
</script>




<style>
iframe {
  position: relative;
  left: 0;
  bottom: 0;
}

body {
  background-image: url("../assets/main_pc.jpg");
}
</style>