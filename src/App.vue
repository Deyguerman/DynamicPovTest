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
            v-model="model"
            :options="options"
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
      <Places />
    </q-page-container>
  </q-layout>
</template>

<script>
import Places from "./components/Places.vue";

export default {
  name: "LayoutDefault",

  components: {
    Places,
  },

  data() {
    return {
      model: null,
      options: ["Google", "Facebook", "Twitter", "Apple", "Oracle"],
    };
  },
};
</script>

<style></style>
