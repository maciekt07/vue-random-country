<script setup>
import Flag from "./components/Flag.vue";
import Table from "./components/Table.vue";
import { ref } from "vue";

let name = ref();
let search = ref("");
let nameOfficial = ref();
let capital = ref();
let population = ref(0);
let area = ref(0);
let region = ref();
let flagSRC = ref();
let mapsLink = ref();
let error = ref(false);
let arr = ref([]);
let select = ref("");

const switchSelect = (e) => {
  select.value = e.target.value;
};

const main = () => {
  error.value = false;
  // flagSRC.value = "loading.gif";
  fetch("https://restcountries.com/v3.1/all")
    .then((response) => response.json())
    .then((data) => {
      arr.value = data.map((country) => country.name.official).sort();

      const countries = data.map((country) => country.name.official);

      const randomNumber = Math.floor(Math.random() * 249);
      const randomCountry = countries[randomNumber];
      console.log(randomCountry);
      let randomCountryLink = `https://restcountries.com/v3.1/name/${randomCountry}`;
      if (search.value != "") {
        randomCountryLink = `https://restcountries.com/v3.1/name/${search.value}`;
      } else if (select.value != "") {
        randomCountryLink = `https://restcountries.com/v3.1/name/${select.value}`;
      }
      document.getElementById("selectCountry").selectedIndex = 0;
      search.value = "";
      fetch(randomCountryLink)
        .then((response) => response.json())
        .then((data) => {
          flagSRC.value = data[0].flags.svg;
          name.value = data[0].name.common;
          nameOfficial.value = data[0].name.official;
          mapsLink.value = data[0].maps.googleMaps;
          capital.value = data[0].capital[0];
          population.value = data[0].population;
          area.value = data[0].area;
          region.value = `${data[0].region} (${data[0].subregion})`;
          console.log();
          const flagSVG = data[0].flags.svg;
          const googleMaps = data[0].maps.googleMaps;
          const commonName = data[0].name.common;
          console.log(commonName.replace(/\s/g, ""));
          document.title = `${commonName} - Random Country`;
        })
        .catch((err) => {
          console.log(err);
          flagSRC.value = "";
          mapsLink.value = "";
          nameOfficial.value = "";
          name.value = "Country not found";
          error.value = true;
        });
    });
};
const searchEnter = () => {
  search.value.length > 0 ? main() : console.log(search.value);
};
main();
</script>
<template>
  <div class="container">
    <select
      @change="
        switchSelect($event);
        main();
      "
      name=""
      id="selectCountry"
    >
      <option value="">Select country</option>
      <option v-bind:key="item" v-for="item in arr" :value="item">
        {{ item }}
      </option>
    </select>
    <nav>
      <input
        @keyup.enter="searchEnter"
        class="search"
        v-model="search"
        id="search"
        type="text"
        placeholder="Search for a country
"
      />
      <button :disabled="search <= 0" class="searchBtn" @click="main">
        <i class="fa-solid fa-magnifying-glass"></i>
      </button>
    </nav>
    <div class="content">
      <Flag v-if="!error" :src="flagSRC" />
      <div class="names">
        <h1 class="name">{{ name }}</h1>
        <h2 class="nameOfficial">{{ nameOfficial }}</h2>
      </div>
      <Table
        v-if="!error"
        :capital="capital"
        :population="population"
        :area="area"
        :region="region"
      />
      <br />
      <div class="btnContainer">
        <button
          class="randomBtn"
          @click="
            main();
            select = '';
          "
        >
          <span><i class="fa-solid fa-globe"></i> Random</span>
        </button>
      </div>
      <br />
      <div class="btnContainer">
        <a :class="{ disabled: error }" target="_blank" :href="mapsLink">
          <button v-if="!error" :disabled="error" class="gmBtn">
            <i class="fa-solid fa-map"></i> View on google maps
          </button>
        </a>
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
@use "./style.scss" as *;

select {
  padding: 2px;
  color-scheme: light;
  font-size: 16px;
  font-family: poppins;
  max-width: 250px;
}

nav {
  display: flex;
  justify-content: center;
  align-content: center;
}
.search {
  margin-top: 20px;
  outline: none;
  border: none;
  padding: 16px;
  font-size: 18px;
  border-radius: 100px;
  width: 400px;
  transition: 0.3s all;
  &:focus {
    box-shadow: 0px 0px 18px 1px #8e37ffcd;
  }
}
.searchBtn {
  margin-top: 20px;
  outline: none;
  border: none;
  font-size: 23px;
  cursor: pointer;
  color: $themeClr;
  background: transparent;
  padding: 13px;
  border-radius: 100px;
  position: absolute;
  margin-left: 340px;
  transition: 0.3s all;
  &:hover {
    text-shadow: 0px 0px 19px #8e37ffa5;
  }
  &:disabled {
    cursor: not-allowed;
    color: rgb(150, 150, 150);
    &:hover {
      text-shadow: none;
    }
  }
}
.header {
  text-align: center;
  color: white;
}
.names {
  margin-top: 50px;
  color: white;
  text-align: center;
}

.name {
  font-size: 40px;
  line-height: 5px;
}
.nameOfficial {
  font-size: 20px;
}

.btnContainer {
  @include center;
}
.randomBtn {
  @include Btn($themeClr);
}
.gmBtn {
  @include Btn(rgb(35, 152, 63));
}
a.disabled {
  pointer-events: none;
}
</style>
