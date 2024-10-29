<script>
export default {
  props: {
    photos: Array,
  },
};
</script>

<template>
  <div>
    <h1>All photos</h1>
    <div v-for="photo in photos" v-bind:key="photo.id">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
      <button v-on:click="$emit('showPhoto', photo)">More info</button>
    </div>
  </div>
</template>

<style></style>
