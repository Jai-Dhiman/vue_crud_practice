<script>
import axios from "axios";
import PhotosIndex from "./PhotosIndex.vue";
import PhotosNew from "./PhotosNew.vue";

export default {
  components: {
    PhotosIndex,
    PhotosNew,
  },
  data: function () {
    return {
      photos: [
        { id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150 },
        { id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300 },
      ],
    };
  },
  created: function () {
    this.handleIndexPhotos();
  },
  methods: {
    handleIndexPhotos: function () {
      axios.get("http://localhost:3000/photos.json").then((response) => {
        console.log("photos index", response);
        this.photos = response.data;
      });
    },
    handleCreatePhoto: function (params) {
      axios
        .post("http://localhost:3000/photos.json", params)
        .then((response) => {
          console.log("photos create", response);
          this.photos.push(response.data);
        })
        .catch((error) => {
          console.log("photos create error", error.response);
        });
    },
  },
};
</script>

<template>
  <main>
    <PhotosNew v-on:createPhoto="handleCreatePhoto" />
    <PhotosIndex v-bind:photos="photos" />
  </main>
</template>

<style></style>
