<template>
  <div id="app">
    <h1>Heraldry Composer</h1>
    <p>
      Heraldry Composer is a humble library for generating & displaying medieval
      heraldries of all kinds.
    </p>
    <p>
      Visit
      <a href="https://github.com/vesamet/heraldry-composer">Github</a> for
      installation & usage.
    </p>
    <h2>Divisions</h2>
    <div class="heraldries-container">
      <div
        v-for="(heraldry, index) in divisionsHeraldries"
        :key="index + 'division'"
      >
        <Heraldry :heraldry="heraldry" :heraldries="heraldries" />
        <h3 style="text-align:center">
          {{ heraldry.division ? heraldry.division.name : "none" }}
        </h3>
      </div>
    </div>
    <h2>Ordinaries</h2>
    <div class="heraldries-container">
      <div
        v-for="(heraldry, index) in ordinariesHeraldries"
        :key="index + 'ordinary'"
      >
        <Heraldry :heraldry="heraldry" :heraldries="heraldries" />
        <h3 style="text-align:center">
          {{ heraldry.ordinary ? heraldry.ordinary.name : "none" }}
        </h3>
      </div>
    </div>
    <h2>Charges</h2>
    <div class="heraldries-container">
      <div
        v-for="(heraldry, index) in chargesHeraldries"
        :key="index + 'charge'"
      >
        <Heraldry :heraldry="heraldry" :heraldries="heraldries" />
        <h3 style="text-align:center">
          {{ heraldry.charge ? heraldry.charge.name : "none" }}
        </h3>
      </div>
    </div>
    <h2>Placements</h2>
    <div class="heraldries-container">
      <div
        v-for="(heraldry, index) in placementsHeraldries"
        :key="index + 'placement'"
      >
        <Heraldry :heraldry="heraldry" :heraldries="heraldries" />
        <h3 style="text-align:center">
          {{
            heraldry.charge && heraldry.charge.placement
              ? heraldry.charge.placement
              : "none"
          }}
        </h3>
      </div>
    </div>
    <h2>Shields</h2>
    <div class="heraldries-container">
      <div
        v-for="(heraldry, index) in shieldsHeraldries"
        :key="index + 'shield'"
      >
        <Heraldry :heraldry="heraldry" :heraldries="heraldries" />
        <h3 style="text-align:center">
          {{
            heraldry.shield && heraldry.shield.name
              ? heraldry.shield.name
              : "none"
          }}
        </h3>
      </div>
    </div>
    <h2>Random heraldries</h2>
    <div class="heraldries-container">
      <div
        v-for="(heraldry, index) in randomHeraldries"
        :key="index + 'random'"
      >
        <Heraldry :heraldry="heraldry" :heraldries="heraldries" />
      </div>
    </div>
  </div>
</template>

<script>
import Heraldry from "@/components/HeraldryComposer/Heraldry"
import { heraldries } from "@/components/HeraldryComposer/heraldries"
let iwanthue = require("iwanthue")
let chroma = require("chroma-js")
export default {
  name: "App",
  components: {
    Heraldry,
  },
  computed: {
    heraldries() {
      return heraldries
    },
    divisionsHeraldries() {
      let heraldriesArray = [
        {
          shield: { name: "default" },
          field: "#522d5b",
        },
      ]
      for (let [name] of Object.entries(heraldries.divisions)) {
        heraldriesArray.push({
          shield: { name: "default" },
          field: "#522d5b",
          division: { name: name, color: ["#d7385e", "#fb7b6b", "#e7d39f"] },
        })
      }
      return heraldriesArray
    },
    ordinariesHeraldries() {
      let heraldriesArray = []
      for (let [name] of Object.entries(heraldries.ordinaries)) {
        heraldriesArray.push({
          shield: { name: "default" },
          field: "#522d5b",
          ordinary: { name: name, color: ["#d7385e", "#fb7b6b", "#e7d39f"] },
        })
      }
      return heraldriesArray
    },
    chargesHeraldries() {
      let heraldriesArray = []
      for (let [name] of Object.entries(heraldries.charges)) {
        heraldriesArray.push({
          shield: { name: "default" },
          field: "#522d5b",
          charge: { name: name, color: ["#d7385e"] },
        })
      }
      return heraldriesArray
    },
    placementsHeraldries() {
      let heraldriesArray = []
      for (let [name] of Object.entries(heraldries.placements)) {
        heraldriesArray.push({
          shield: { name: "default" },
          field: "#522d5b",
          division: { name: "chess", color: ["#fb7b6b", "#e7d39f", "#B68DFF"] },
          charge: {
            name: "snake",
            color: ["#d7385e", "#d7385e", "#d7385e", "#d7385e", "#d7385e"],
            placement: name,
          },
        })
      }
      return heraldriesArray
    },
    shieldsHeraldries() {
      let heraldriesArray = [
        {
          field: "#522d5b",
        },
      ]
      for (let [name] of Object.entries(heraldries.shields)) {
        heraldriesArray.push({
          shield: { name: name },
          field: "#522d5b",
        })
      }
      return heraldriesArray
    },
    randomHeraldries() {
      let heraldriesArray = []
      let randomProperty = function(obj) {
        return Object.keys(obj)[
          Math.floor(Math.random() * Object.keys(obj).length)
        ]
      }
      let bgLum,
        chargeLum,
        partialHue,
        completeHue,
        backgroundColors,
        chargeColors,
        composeHeraldry
      for (let i = 0; i <= 20; i++) {
        // define colors
        let luminosity = [
          [0, 35], //dark
          [70, 80], //light
        ]
        //randomly choose if the charge or the background elements
        //will have a darker scheme
        if (Math.floor(Math.random() * Math.floor(2)) === 1) {
          bgLum = luminosity[0]
          chargeLum = luminosity[1]
        } else {
          bgLum = luminosity[1]
          chargeLum = luminosity[0]
        }
        partialHue = Math.floor(Math.random() * Math.floor(341))
        completeHue = Math.floor(Math.random() * Math.floor(361))
        backgroundColors = iwanthue(5, {
          colorSpace: [partialHue, partialHue + 20, 0, 40, bgLum[0], bgLum[1]],
          clustering: "force-vector",
        })
        chargeColors = iwanthue(5, {
          colorSpace: [0, 360, 50, 100, chargeLum[0], chargeLum[1]],
          clustering: "force-vector",
        })

        // compose heraldry
        composeHeraldry = {
          shield: { name: randomProperty(heraldries.shields) },
          field: backgroundColors[0],
          division: {
            name: randomProperty(heraldries.divisions),
            color: [
              backgroundColors[1],
              backgroundColors[2],
              backgroundColors[3],
            ],
          },
          ordinary: {
            name: randomProperty(heraldries.ordinaries),
            color: [chroma(backgroundColors[4]).set("hsl.h", completeHue)],
          },
          charge: {
            name: randomProperty(heraldries.charges),
            color: [
              chargeColors[0],
              chargeColors[1],
              // chargeColors[2],
              // chargeColors[3],
              // chargeColors[4],
            ],
            placement: randomProperty(heraldries.placements),
          },
        }

        //apply generation constraint
        if (Math.random() < 0.6) {
          delete composeHeraldry.ordinary

          if (
            composeHeraldry.charge.placement == "five" ||
            composeHeraldry.charge.placement == "four"
          ) {
            composeHeraldry.charge.placement == "one"
          }
        }
        if (
          composeHeraldry.charge.placement == "two" ||
          composeHeraldry.charge.placement == "twoVertical"
        ) {
          composeHeraldry.charge.color = [
            composeHeraldry.charge.color[0],
            composeHeraldry.charge.color[0],
          ]
        }
        //
        heraldriesArray.push(composeHeraldry)
      }
      return heraldriesArray
    },
  },
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Merriweather:700i&display=swap");
#app {
  font-family: "Merriweather", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  background: rgb(255, 255, 255);
  background: linear-gradient(
    176deg,
    rgba(255, 255, 255, 1) 0%,
    rgba(216, 225, 227, 1) 100%
  );
}
.heraldries-container {
  margin: auto;
}
.heraldries-container div {
  min-width: 180px;
  max-width: 200px;
  margin: 10px;
  display: inline-block;
}
</style>
