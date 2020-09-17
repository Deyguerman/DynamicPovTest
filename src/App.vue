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
            @input="handlerChange($event)"
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
const { DeepstreamClient } = window.DeepstreamClient;
export default {
  name: "LayoutDefault",
  components: {
    Places,
  },
  data() {
    return {
      ds: null,
      scroll: null,
      places: {},
      portsCodes: null,
      destinations: [],
    };
  },
  created() {
    this.ds = new DeepstreamClient("192.168.0.10:6020"); // Replace with the Ip Server
    this.ds.login();
    this.places = this.ds.record.getRecord("test/place");

    this.ds.event.subscribe("setPortsCodes", (eventData) => {
      this.portsCodes = eventData;
    });

    this.ds.event.subscribe("scrollPage", (eventData) => {
      window.scrollTo(eventData.x, eventData.y);
    });

    axios.get("./data/ports.json").then((response) => {
      this.getDestinationList(response.data, "destination");
    });
  },
  mounted() {
    window.addEventListener("scroll", () => {
      clearTimeout(this.scroll);
      this.scroll = setTimeout(() => {
        this.ds.event.emit("scrollPage", {
          x: window.scrollX,
          y: window.scrollY,
        });
      }, 500);
    });
  },
  methods: {
    handlerChange(value) {
      this.places.set(value);
      this.ds.event.emit("setPortsCodes", value);
    },
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
