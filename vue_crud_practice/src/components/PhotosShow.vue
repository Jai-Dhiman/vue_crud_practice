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
      const params = {
        name: this.editPhotoParams.name,
        url: this.editPhotoParams.url,
        body: this.editPhotoParams.body,
        width: parseInt(this.editPhotoParams.width),
        height: parseInt(this.editPhotoParams.height),
      };
      this.$emit("updatePhoto", this.photo.id, params);
    },
  },
  watch: {
    photo: {
      handler(newPhoto) {
        this.editPhotoParams = { ...newPhoto };
      },
      deep: true,
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
    <h1>{{ photo.name }}</h1>
    <p>{{ photo.body }}</p>

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
        Body:
        <input v-model="editPhotoParams.body" type="text" />
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
