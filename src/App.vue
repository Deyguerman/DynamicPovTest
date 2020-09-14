<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="glossy">
      <q-toolbar>
        <q-toolbar-title>
          DynamicPov Test
        </q-toolbar-title>
        <div class="col q-gutter-md q-mr-sm" style="max-width: 500px">
          <q-select
            outlined
            dense
            bg-color="white"
            transition-show="scale"
            transition-hide="scale"
            label="Destinations"
            v-model="portsCodes"
            :options="destinations"
            :option-value="
              (opt) =>
                Object(opt) === opt && 'codes' in opt ? opt.codes : null
            "
            :option-label="
              (opt) =>
                Object(opt) === opt && 'destination' in opt
                  ? opt.destination
                  : '- Null -'
            "
          >
            <template v-slot:append>
              <q-icon name="place" />
            </template>
          </q-select>
        </div>
        <q-separator dark vertical />
        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <Places :portsCodes="portsCodes" />
    </q-page-container>
  </q-layout>
</template>

<script>
import Places from "./components/Places.vue";
import axios from "axios";

export default {
  name: "LayoutDefault",
  components: {
    Places,
  },
  data() {
    return {
      portsCodes: null,
      destinations: [],
    };
  },
  created() {
    axios.get("./data/ports.json").then((response) => {
      this.getDestinationList(response.data, "destination");
    });
  },
  methods: {
    getDestinationList(list, key) {
      // Get Array uniques port names
      const destinationNames = [...new Set(list.map((item) => item[key]))];
      for (const elem of destinationNames) {
        const destinationList = list.filter((item) => item[key] == elem);
        // Get Array with unique codes
        const codes = [...new Set(destinationList.map((item) => item.code))];

        this.destinations.push({ destination: elem, codes });
      }
    },
  },
};
</script>

<style></style>
