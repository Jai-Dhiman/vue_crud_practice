<script>
export default {
  props: {
    photo: Object,
  },
  data: function () {
    return {
      editPhotoParams: {},
    };
  },
  created: function () {
    this.editPhotoParams = { ...this.photo };
  },
  methods: {
    handleSubmit: function () {
      this.$emit("updatePhoto", this.photo.id, this.editPhotoParams);
    },
  },
};
</script>

<template>
  <div>
    <div class="photo-display">
      <img
        :src="photo.url"
        :alt="photo.name"
        :style="{ maxWidth: photo.width + 'px', maxHeight: photo.height + 'px' }"
      />
    </div>
    <h1>Photo information</h1>
    <p>Name: {{ photo.name }}</p>
    <p>Url: {{ photo.url }}</p>
    <p>Width: {{ photo.width }}</p>
    <p>Height: {{ photo.height }}</p>
    <form v-on:submit.prevent="handleSubmit">
      <div>
        Name:
        <input v-model="editPhotoParams.name" type="text" />
      </div>
      <div>
        Url:
        <input v-model="editPhotoParams.url" type="text" />
      </div>
      <div>
        Width:
        <input v-model="editPhotoParams.width" type="text" />
      </div>
      <div>
        Height:
        <input v-model="editPhotoParams.height" type="text" />
      </div>
      <button type="submit">Update photo</button>
    </form>
    <button v-on:click="$emit('destroyPhoto', photo)">Destroy photo</button>
  </div>
</template>

<style></style>
