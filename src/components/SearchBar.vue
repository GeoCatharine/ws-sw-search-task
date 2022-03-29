<template>
    <div class="container">
      <h1>StarWars Search</h1>
      <q-input 
        outlined 
        v-model="search" 
        label="Name" 
        :dense="dense" 
        debounce="1000" 
        color="ws-gem"
        :loading="isLoading"
        @input="getAllPeople"
        > 
          <template v-slot:append>
            <q-icon v-show="!isLoading" name="search" />
          </template> 
          <template v-slot:default>
            <span v-bind:class="{ 'highlight': !match, 'hide': match }"> {{ search }} </span>
          </template>
      </q-input>
      <div v-if="!search">
        <p>Enter search pharse</p>
      </div>
      <div v-else-if="showError">
        <p>Oops, something went wrong.</p>
      </div>
      <div v-else-if="names.length > 0" class="list" >
        <div class="text-white">
          <q-list class="bg-ws-navy" v-for="person in names" :key="person.index" dark>
            <q-item v-ripple class="row">
              <BoldenListItem 
                :name="person.name"
                :search="search"
              />
            </q-item>
          </q-list>
        </div>
      </div>
      <div v-else-if="this.names.length === 0">
        <p>Nothing found</p>
      </div>
    </div>
</template>
<script>

import axios from "axios";
import BoldenListItem from "./BoldenListItem.vue";

export default {
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

        axios.get("https://swapi.py4e.com/api/people/", { params: { search: this.search } })
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
      match() {
        return this.names.length > 0;
      },
    },
    components: { BoldenListItem }
}
</script>
<style sass>

  .container {
    width: 50%;
    margin: 0 auto;
    position: relative;
  }

  .list {
    width: 50%;
    margin: 0 auto;
  }

  .q-list {
    margin: 5px 0;
    border-radius: 5px;
    cursor: pointer;
  }

  .q-input {
    z-index: 2;
    width: 50%;
    margin: 0 auto;
    border-radius: 5px;
    padding: 20px 0;
    background-color: transparent;
    overflow: hidden;
  }

  .highlight {
    position: absolute;
    left: 0;
    bottom: 10px;
    background-color: #FEE6E0;
    color: transparent;
    z-index: -1;
    font-size: 15px;
  }

  .hide {
    visibility: hidden;
  }

  h1 {
    text-align: center;
    font-size: 4rem;
    margin:0;
    padding: 20px 0;
  }

  p {
    text-align: center;
    padding: 20px 0;
    margin: 0;
  }


</style>