<!-- eslint-disable camelcase -->
<template>
  <div class="main-box">
    <div class="search-box">
      <input class="search-bar" placeholder="Search..." v-model="search" @keyup="takeCountry"/>
    </div>
    <div v-for="item in datas" v-bind:key="item.place_id" class="info-box">
      <p class="item-name">{{item.display_name}}</p>
      <img alt="..." src={{item.icon}} class="image"/>
      <p class="text">Лицензия: {{item.licence}}</p>
      <p class="text">Координаты: {{item.boundingbox}}</p>
      <p class="text">Место в рейтинге: {{item.place_rank}}</p>
      <p class="text">Категория: {{item.category}}</p>
      <p class="text">Тип: {{item.type}}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { assertExpressionStatement } from '@babel/types';
import { defineComponent } from 'vue';
import axios from 'axios';

interface Data{
  place_id:string;
  display_name:string;
  licence:string;
  osm_type:string;
  osm_id:string;
  boundingbox:[ ];
  lat:string;
  lon:string;
  place_rank: number;
  category:string;
  type:string;
  importance:string;
  icon:string | undefined;
}

interface Countries{
  search: string
  datas: Data[]
}

export default defineComponent({
  name: 'App',
  data(): Countries {
    return {
      search: '',
      datas: [],
    };
  },

  methods: {
    takeCountry():void {
      axios
        .get(`https://nominatim.openstreetmap.org/search?q=${this.search}&format=jsonv2`).then((res) => {
          this.datas = res.data;
        });
    },
  },

  setResults(results: Data[]) {
    this.datas = results;
  },

});
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
  background-color:  rgb(214, 214, 214);
}
.main-box {
  width: 70%;
  margin-left: 15%;
  margin-right: 15%;
}
.search-box {
  margin-bottom: 30px;
  margin-top: 30px
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: white;
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgb(224, 255, 245);
  border-radius: 16px 0px 16px 0px;
}
.info-box {
  display: flex;
  flex-direction: column;
  background-color: white;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  padding: 16px 16px 16px 16px;
  margin-bottom: 16px;
  border-radius: 5px;
}
.item-name{
  font-size: 20px;
  align-self: center;
  text-align: center;
}
.image{
  align-self: center;
  width: 50px;
  height: 50px;
}
p{
  margin: 5px 0px 5px 0px;
}
</style>
