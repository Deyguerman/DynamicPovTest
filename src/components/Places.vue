<template>
  <div class="q-pa-md row items-center justify-center q-gutter-md">
    <div class="row q-pa-md" v-if="!portsCodes">
      <div class="text-h5">Please, choose a destination!</div>
    </div>

    <div v-else-if="portsCodes && places.length == 0" class="row q-gutter-md">
      <q-card style="width: 240px" v-for="item in 10" :key="item">
        <q-skeleton height="200px" square />

        <q-card-section>
          <q-skeleton type="text" class="text-h6" />
          <q-skeleton type="text" width="50%" class="text-subtitle2" />
          <q-skeleton type="text" width="50%" class="text-caption" />
          <q-skeleton type="text" width="50%" class="text-caption" />
        </q-card-section>
      </q-card>
    </div>

    <div v-for="(place, i) in places" :key="i" v-else>
      <q-card class="my-card">
        <q-img :src="place.imagePath" style="height: 150px; width: 250px" />
        <q-card-section>
          <div class="text-h6">{{ place.title }}</div>
          <div class="text-subtitle2">
            <q-icon name="place" />
            {{ place.location }}
          </div>
          <div class="text-subtitle2">
            <q-icon name="alarm" />
            {{ place.duration }}
          </div>
          <div class="text-subtitle2">
            <q-icon name="attach_money" />
            {{ place.price.childPrice }} / {{ place.price.adultPrice }}
          </div>
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
  min-height: 350px
  max-height: 550px
</style>

<script>
import axios from "axios";
export default {
  name: "Places",
  data() {
    return {
      places: [],
    };
  },
  created() {},
  methods: {
    getPlaces() {
      this.places = [];
      const codes = this.portsCodes.codes;

      axios.get("./data/shorex.json").then((response) => {
        this.places = response.data
          .filter((elem) => {
            return codes.includes(elem.port.code);
          })
          .map((elem) => ({
            imagePath: elem.imagePath,
            title: elem.title,
            location: elem.port.name,
            duration: elem.duration,
            price: { adultPrice: elem.adultPrice, childPrice: elem.childPrice },
          }));
      });
    },
  },
  watch: {
    portsCodes: {
      immediate: true,
      handler(newVal) {
        if (newVal) {
          return this.getPlaces();
        }
      },
    },
  },
  props: {
    portsCodes: {
      type: Object,
      default: () => {},
    },
  },
};
</script>
