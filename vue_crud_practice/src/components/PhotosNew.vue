<script>
export default {
  data: function () {
    return {
      newPhotoParams: {
        name: "",
        url: "",
        body: "",
        width: "",
        height: "",
      },
    };
  },
  methods: {
    handleSubmit: function () {
      const params = {
        name: this.newPhotoParams.name,
        url: this.newPhotoParams.url,
        body: this.newPhotoParams.body,
        width: parseInt(this.newPhotoParams.width),
        height: parseInt(this.newPhotoParams.height),
      };
      this.$emit("createPhoto", this.newPhotoParams);
      this.newPhotoParams = {
        name: "",
        url: "",
        body: "",
        width: "",
        height: "",
      };
    },
  },
};
</script>

<template>
  <div>
    <h1>New Photo</h1>

    <form v-on:submit.prevent="handleSubmit">
      <div>
        Name:
        <input type="text" v-model="newPhotoParams.name" />
      </div>
      <div>
        Url:
        <input type="text" v-model="newPhotoParams.url" />
      </div>
      <div>
        Body:
        <input type="text" v-model="newPhotoParams.body" />
      </div>
      <div>
        Width:
        <input type="text" v-model="newPhotoParams.width" />
      </div>
      <div>
        Height:
        <input type="text" v-model="newPhotoParams.height" />
      </div>
      <button type="submit">Create photo</button>
    </form>
  </div>
</template>

<style></style>
