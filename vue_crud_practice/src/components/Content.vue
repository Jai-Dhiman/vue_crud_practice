<script>
import axios from "axios";
import PhotosIndex from "./PhotosIndex.vue";
import PhotosNew from "./PhotosNew.vue";
import PhotosShow from "./PhotosShow.vue";
import Modal from "./Modal.vue";

export default {
  components: {
    PhotosIndex,
    PhotosNew,
    PhotosShow,
    Modal,
  },
  data: function () {
    return {
      photos: [],
      currentPhoto: {},
      isPhotosShowVisible: false,
    };
  },
  created: function () {
    this.handleIndexPhotos();
  },
  methods: {
    handleIndexPhotos: function () {
      axios.get("http://localhost:5000/photos.json").then((response) => {
        console.log("photos index", response);
        this.photos = response.data;
      });
    },
    handleCreatePhoto: function (params) {
      axios
        .post("http://localhost:5000/photos.json", params)
        .then((response) => {
          console.log("photos create", response);
          this.photos.push(response.data);
        })
        .catch((error) => {
          console.log("photos create error", error.response);
        });
    },
    handleShowPhoto: function (photo) {
      console.log("handleShowPhoto", photo);
      this.currentPhoto = photo;
      this.isPhotosShowVisible = true;
    },
    handleUpdatePhoto: function (id, params) {
      console.log("handleUpdatePhoto", id, params);

      const updateParams = {
        name: params.name,
        url: params.url,
        body: params.body,
        width: parseInt(params.width),
        height: parseInt(params.height),
      };

      axios
        .patch(`http://localhost:5000/photos/${id}.json`, updateParams)
        .then((response) => {
          console.log("photos update", response);
          this.photos = this.photos.map((photo) => {
            if (photo.id === parseInt(id)) {
              return response.data;
            } else {
              return photo;
            }
          });
          this.handleClose();
        })
        .catch((error) => {
          console.log("photos update error", error.response);
        });
    },
    handleDestroyPhoto: function (photo) {
      axios.delete(`http://localhost:5000/photos/${photo.id}.json`).then((response) => {
        console.log("photos destroy", response);
        var index = this.photos.indexOf(photo);
        this.photos.splice(index, 1);
        this.handleClose();
      });
    },
    handleClose: function () {
      this.isPhotosShowVisible = false;
    },
  },
};
</script>

<template>
  <main>
    <PhotosNew v-on:createPhoto="handleCreatePhoto" />
    <PhotosIndex v-bind:photos="photos" v-on:showPhoto="handleShowPhoto" />
    <Modal v-bind:show="isPhotosShowVisible" v-on:close="handleClose">
      <PhotosShow
        v-bind:photo="currentPhoto"
        v-on:updatePhoto="handleUpdatePhoto"
        v-on:destroyPhoto="handleDestroyPhoto"
      />
    </Modal>
  </main>
</template>

<style></style>
