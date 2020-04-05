<template>
  <div class="shield-container">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 250 300"
      :clip-path="shield"
    >
      <rect :fill="heraldry.field" width="100%" height="100%" />
      <g
        v-html="division"
        width="100%"
        height="100%"
        class="shield-element"
      ></g>
      <g
        v-html="ordinary"
        width="100%"
        height="100%"
        class="shield-element"
      ></g>
      <g
        v-html="heraldries.effects.shieldOverlay"
        width="100%"
        height="100%"
        class="shield-element"
      ></g>
      <g
        v-html="charge"
        fill="red"
        width="100%"
        height="100%"
        class="shield-element"
      ></g>
      <g
        v-if="error"
        width="100%"
        height="100%"
        v-html="heraldries.errorHeraldry"
        class="shield-element"
      ></g>
    </svg>
  </div>
</template>

<script>
import { heraldries } from "@/components/olgorianSlates/heraldryComposer/heraldries"
export default {
  data() {
    return {
      division: null,
      ordinary: null,
      charge: null,
      error: false,
    }
  },
  props: {
    heraldry: {
      type: Object,
    },
  },
  computed: {
    heraldries() {
      return heraldries
    },
    shield() {
      if (this.heraldry.shield) {
        return heraldries.shields[this.heraldry.shield?.name].value
      } else {
        return ""
      }
    },
  },
  methods: {
    defineHeraldry() {
      //define division
      this.defineElement({ arrayName: "divisions", element: "division" })
      //define ordinary
      this.defineElement({ arrayName: "ordinaries", element: "ordinary" })
      //define charges
      this.defineElement({ arrayName: "charges", element: "charge" })
    },
    defineElement(p) {
      //retrieve array of element's svg strings
      let elements = this.heraldries[p.arrayName][
        this.heraldry[p.element]?.name
      ]

      if (elements) {
        //apply placement rule
        let placement = this.heraldries.placements[
          this.heraldry[p.element]?.placement
        ]
        if (placement?.length) {
          for (var i = 2; i <= placement.length; i++) {
            elements.value = Array.from(
              { length: placement.length },
              () => elements.value[0]
            )
            this.heraldry[p.element].color = Array.from(
              { length: placement.length },
              () => this.heraldry[p.element].color[0] || "black"
            )
          }
        }
        //define and concat element(s)
        let svg = ""
        for (let i = 0; i < elements.value.length; i++) {
          let color = this.heraldry[p.element].color[i] || color[0] || "black"
          svg = svg.concat(
            `<g
            fill="${color}"
            style="transform-origin: center;"
            ${placement ? placement[i] : ""}
            >
            ${elements.value[i]}
            </g>`
          )
        }
        this[p.element] = svg
      }
    },
  },
  mounted() {
    this.defineHeraldry()
  },
}
</script>

<style scoped>
.shield-element {
  transform-origin: center;
  transform-box: fill-box;
}
</style>
