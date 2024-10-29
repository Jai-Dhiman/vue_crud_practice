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
      photos: [
        { id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150 },
        { id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300 },
      ],
      currentPhoto: {},
      isPhotosShowVisible: false,
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
    handleShowPhoto: function (photo) {
      console.log("handleShowPhoto", photo);
      this.currentPhoto = photo;
      this.isPhotosShowVisible = true;
    },
    handleUpdatePhoto: function (id, params) {
      console.log("handleUpdatePhoto", id, params);
      axios
        .patch(`http://localhost:3000/photos/${id}.json`, params)
        .then((response) => {
          console.log("photos update", response);
          this.photos = this.photos.map((photo) => {
            if (photo.id === response.data.id) {
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
      axios.delete(`http://localhost:3000/photos/${photo.id}.json`).then((response) => {
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
      <PhotosShow v-bind:photo="currentPhoto" v-on:updatePhoto="handleUpdatePhoto" v-on:destroyPhoto="handleDestroyPhoto" />
      </Modal>
    </main>
  </template>

  <style></style>


    </Modal>
  </main>
</template>

<style></style>
