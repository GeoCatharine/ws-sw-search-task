<template>
    <div class="container q-my-none q-mx-auto relative-position">
      <h1 class="text-h2 q-py-xl text-center q-ma-none">StarWars Search</h1>
      <q-input 
        outlined 
        v-model="search" 
        label="Name" 
        :dense="dense" 
        debounce="500" 
        :loading="isLoading"
        @input="getAllPeople"
        class="transparent q-mx-auto"
        > 
          <template v-slot:append>
            <q-icon v-show="!isLoading" name="search" />
          </template> 
          <template v-slot:default>
            <span v-bind:class="{ 'highlight': !hasNames, 'hide': hasNames }"> {{ search }} </span>
          </template>
      </q-input>
      <div v-if="!search">
        <p class="text-center q-py-xl q-ma-none">Enter search pharse</p>
      </div>
      <div class="text-center q-py-xl q-ma-none" v-else-if="showError">
        <p>Oops, something went wrong.</p>
      </div>
      <div v-else-if="hasNames" class="q-my-none q-mx-auto" >
        <div class="text-white q-pb-lg">
          <q-list class="bg-ws-navy q-mt-xs q-mx-auto cursor-inherit" v-for="person in names" :key="person.index" dark>
            <q-item v-ripple class="row">
              <BoldenListItem 
                :name="person.name"
                :search="search"
              />
            </q-item>
          </q-list>
        </div>
      </div>
      <div v-else>
        <p class="text-center q-py-xl q-ma-none">Nothing found</p>
      </div>
    </div>
</template>
<script>

import swapi from '../api/swapi.js';
import BoldenListItem from "./BoldenListItem.vue";

export default {
  components: { BoldenListItem },
  data() {
    return {
      search: "",
      names: [],
      dense: false,
      isLoading: false,
      showError: false,
    };
  },
  methods: {
    getAllPeople() {
      this.isLoading = true;

      if (!this.search) {
        this.isLoading = false;
        return;
      }

      if (this.showError) {
        this.showError = false;
      }

      swapi.get("people/", { params: { search: this.search } })
        .then((response) => {
          this.names = response.data.results;
        })
        .catch(err => {
          this.showError = true;
        })
        .finally( () => {
          this.isLoading = false;
        });
    },
  },
  computed: {
    hasNames() {
      return this.names.length > 0;
    },
  }
}
</script>