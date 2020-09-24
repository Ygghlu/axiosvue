<template>
  <div>
    <div class="content">
      <b-jumbotron>
        <img src="../assets/logo.png" alt />
        <template v-slot:header>Muse House</template>
        <template v-slot:lead>Search for your favorite song on Apple music</template>
        <b-row align-h="center"></b-row>

        <hr class="my-4" />

        <b-row align-h="center" v-if="searchmet==1">
          <b-col cols="5">
            <b-form-input v-model="keyword" placeholder="Enter keyword"></b-form-input>
          </b-col>
          <b-col cols="2" md="auto">
            <b-button pill @click="searchdata()">Search</b-button>
            <b-button @click="advance()" size="sm" variant="outline">Advance Search</b-button>
          </b-col>
        </b-row>
        <b-row align-h="center" v-if="searchmet==2">
          <b-col cols="5">
            <b-form-input v-model="key" placeholder="Enter keyword"></b-form-input>
          </b-col>
          <b-col cols="5">
            <b-form-select v-model="selected" :options="options"></b-form-select>
          </b-col>
          
        </b-row >
        <b-row class="my-4" align-h="center"   v-if="searchmet==2"><b-col cols="4" md="auto">
            <b-button pill @click="searchAdvance()">Search music</b-button>
          </b-col></b-row>
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
        <b-card-group columns v-if="selected === 'entity=album&attribute=albumTerm'">
          <b-card
            no-body
            class="overflow-hidden"
            style="max-width: 540px"
            :per-page="perPage"
            :current-page="currentPage"
            v-for="data in resultData.slice((currentPage-1)*perPage,(currentPage-1)*perPage+perPage)"
            :key="data.collectionId"
          >
            <b-row no-gutters>
              <b-col md="4">
                <a :href="data.collectionViewUrl" target="_blank">
                  <b-card-img :src="data.artworkUrl100" class="rounded-0"></b-card-img>
                </a>
              </b-col>
              <b-col md="8">
                <b-card-body :title="data.collectionName">
                  <b-card-text>
                    <p>Artist:{{data.artistName}}</p>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-card-group>
        <b-card-group columns v-else-if="selected === 'entity=allArtist'">
          <b-card
            no-body
            class="overflow-hidden"
            style="max-width: 540px"
            :per-page="perPage"
            :current-page="currentPage"
            v-for="data in resultData.slice((currentPage-1)*perPage,(currentPage-1)*perPage+perPage)"
            :key="data.artistId"
          >
            <b-row no-gutters>
              <b-col>
                <b-card-body :title="data.artistName">
                  <b-card-text>
                    <a :href="data.artistLinkUrl" target="_blank">Artist Profile</a>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-card-group>
        <b-card-group columns v-else-if="selected === 'entity=allTrack&attribute=allTrackTerm'">
          <b-card
            no-body
            class="overflow-hidden"
            style="max-width: 540px"
            :per-page="perPage"
            :current-page="currentPage"
            v-for="data in resultData.slice((currentPage-1)*perPage,(currentPage-1)*perPage+perPage)"
            :key="data.artistId"
          >
            <b-row no-gutters>
              <b-col>
                <b-card-body :title="data.artistName">
                  <b-card-text>
                    <a :href="data.artistLinkUrl" target="_blank">Artist Profile</a>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-card-group>

        <b-card-group columns v-else>
          <b-card
            no-body
            class="overflow-hidden"
            style="max-width: 540px"
            :per-page="perPage"
            :current-page="currentPage"
            v-for="data in resultData.slice((currentPage-1)*perPage,(currentPage-1)*perPage+perPage)"
            :key="data.trackId"
          >
            <b-row no-gutters>
              <b-col md="4">
                <a :href="data.trackViewUrl" target="_blank">
                  <b-card-img :src="data.artworkUrl100" class="rounded-0"></b-card-img>
                </a>
              </b-col>
              <b-col md="8">
                <b-card-body :title="data.trackName">
                  <b-card-text>
                    <p>Artist:{{data.artistName}}</p>
                    <p>Album : {{data.collectionName}}</p>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
            <b-row align-h="center">
              <audio controls>
                <source :src="data.previewUrl" />
              </audio>
            </b-row>
          </b-card>
        </b-card-group>
      </b-container>
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
      key: "",
      selected: null,
      searchmet: 1,
      options: [
        { value: null, text: "Please select season" },
        { value: "entity=allTrack&attribute=allTrackTerm", text: "Track name" },
        { value: "entity=album&attribute=albumTerm", text: "Album name" },
        { value: "entity=allArtist", text: "Artist Name" },
      ],
    };
  },
  methods: {
    searchdata() {
      axios
        .get("https://itunes.apple.com/search?term=" + this.keyword)
        .then(
          (response) => (
            (this.resultData = response.data.results),
            (this.load = true),
            (this.currentPage = 1)
          )
        )
        .catch((error) => console.log(error));
    },
    searchAdvance() {
      axios

        .get(
          "https://itunes.apple.com/search?term=" +
            this.key +
            "&" +
            this.selected
        )
        .then(
          (response) => (
            (this.resultData = response.data.results),
            (this.load = true),
            (this.currentPage = 1)
          )
        )
        .catch((error) => console.log(error));
    },
    advance() {
      this.searchmet = 2;
    },
  },
};
</script>




<style>
.content {
  background: rgba(0, 0, 0, 0.5);
  position: relative;
  width: 100%;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>