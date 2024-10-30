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
      <img v-bind:src="photo.url" v-bind:style="{ width: photo.width + 'px', height: photo.height + 'px' }" />
      <button v-on:click="$emit('showPhoto', photo)">More info</button>
    </div>
  </div>
</template>

<style></style>
