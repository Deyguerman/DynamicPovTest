<template>
  <div class="q-pa-md row items-end q-gutter-md">
    <div v-for="place in places" :key="place">
      <q-card class="my-card">
        <img src="https://cdn.quasar.dev/img/mountains.jpg" />

        <q-card-section>
          <div class="text-h6">Our Changing Planet</div>
          <div class="text-subtitle2">by John Doe</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Placeat,
          voluptatibus mollitia sint, similique, eveniet consequatur rem explicabo
          recusandae voluptates nesciunt aperiam. Quidem excepturi expedita
          necessitatibus dolore, consequuntur autem quam architecto!
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
</style>

<script>
export default {
  name: "Places",
  data() {
    return {
      model: null,
      options: ["Google", "Facebook", "Twitter", "Apple", "Oracle"],
      places: [1,2,3,4,5,6,7,8,9,10]
    };
  },
};
</script>
