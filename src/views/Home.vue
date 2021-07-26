<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div> 
      <p> Name: <input type="text" v-model="newPlaceParams.name"> </p> 
      <p> Address: <input type="text" v-model="newPlaceParams.address"> </p> 
    <button v-on:click="newPlace();"> Add a new place </button>
    </div>
    
    <div v-for="place in places"> 

      <p> Name: {{ place.name }} </p> 
      <p> Address: {{ place.address }} </p>   
      
      <button v-on:click="showPlace(place);">More info</button>





  <hr> 

    </div>
    <dialog id="place-details"> 
      <form method="dialog">
        <h1> Place Information </h1>
        <!-- <p> Name: {{ currentPlace.name }}</p>
        <p> Address: {{ currentPlace.address }}</p> -->
        <p> Name: <input type="text" v-model ="currentPlace.name"> </p>
        <p> Address:  <input type="text" v-model="currentPlace.address"> </p>

        <button v-on:click="updatePlace(currentPlace);"> Update Place </button>
        <button> Close </button>
      </form>

    </dialog> 
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "This is the Places App. There are a lot of cool places here",
      places: [],
      newPlaceParams: {},
      currentPlace: {},
      editPlaceParams: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      console.log("index action!!");
      axios.get("/places").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    newPlace: function () {
      console.log("Making a new place!");
      axios.post("/places", this.newPlaceParams).then((response) => {
        console.log(response.data);
        this.places.push(response.data);
        this.newPlaceParams = {};
      });
    },

    showPlace: function (place) {
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (currentPlace) {
      console.log("updating place.....");
      axios.patch(`/places/${thePlace}`).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>