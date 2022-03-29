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
        @input="getAllPeople"
        > 
          <template v-slot:append>
            <q-icon name="search" />
          </template> 
      </q-input>
      <div v-if="!search">
        <p>Enter search pharse</p>
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
            dense: false
        };
    },
    methods: {
      getAllPeople() {
        axios.get("https://swapi.py4e.com/api/people/", { params: { search: this.search } })
          .then((response) => {
            this.names = response.data.results;
            console.log(this.names.length)
          })
          .catch(err => {
            console.log("ohh something went wrong");
          });
      },
    },
    components: { BoldenListItem }
}
</script>
<style scss>

  .container {
    width: 50%;
    margin: 0 auto;
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
    width: 50%;
    margin: 0 auto;
    border-radius: 5px;
    padding: 20px 0;
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