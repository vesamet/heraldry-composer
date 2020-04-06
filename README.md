
![Heraldry Composer logo](https://github.com/vesamet/heraldry-composer/blob/master/screenshot.png "Heraldry Composer logo")

Heraldry Composer is a Vuejs component for generating & displaying medieval heraldries of all kinds.
It's vector based (svg), fully responsive, customizable, and as no dependencies.

Visit https://vesamet.github.io/heraldry-composer/ to see it in action.

## Installation
1. Install the package
```
yarn add @bit/vesamet.olgorian-slates.heraldry-composer
```

1. Import and register the Heraldry component

```js
import Heraldry from "@bit/vesamet.olgorian-slates.heraldry-composer"

export default {
  components: {
    Heraldry,
  },
}
```

2. Import the "ruleset" where all the elements available for the heraldries are defined

```js
import { heraldries } from "@bit/vesamet.olgorian-slates.heraldry-composer/heraldries"

computed: {
    heraldries() {
      return heraldries
    },
},
```
##Usage

1. Define the component in your vue template

```js
<Heraldry :heraldry="heraldry" :heraldries="heraldries" />
```

2. Define your heraldry

From the element displayed on [the github page](https://vesamet.github.io/heraldry-composer/), choose your desired shield type, divison, ordinary and charge, and colors.
Your heraldry definition should have the same format as the following example:

```js
heraldry: {
        shield: {
          name: "default",
        },
        field: "#8cc4d5",
        division: {
          name: "halfDiagonalRight",
          color: ["#27d0ea", "#8bd2d9", "#42bcdb"],
          //^up to 4 colors are required depending on the division type
        },
        ordinary: { name: "arrowTop", color: ["lightblue"] },
        charge: {
          name: "stampedEye",
          color: ["#861500", "#861500"],
          //^up to 5 colors are required depending onthe placement type
          placement: "one",
        },
      },
```
**A complete example can be found in [this playground](https://bit.dev/vesamet/olgorian-slates/heraldry-composer)**

Note that charges with a placement of 4 will always have the two first

## Adding your own shield type, division, ordinaries or charge

**For divisions, ordinaries and charges:**

1. Create your element using your favorite vector editor. Make sure you set the canvas to 250 width by 300 height. Then export it as svg
2. (Optionnal) Minify your svg.

- Load your file in this [fantastic tool](https://jakearchibald.github.io/svgomg/)
- Download the minified version of your svg.

3. Open your svg with a text editor and copy enverything that is **inside** the svg tags,
   then paste it in the "ruleset", like so:

```js
divisions: {
    //...
    cloakedRight: {
        value: [`<path d="M0 0l250 300H0z"/><path d="M250 0L0 300h250z"/>`],
    },
}
```

You may split the element in multiple strings so that each path will have it's own color:

```js
{
     value: [`<path d="M0 0l250 300H0z"/>` + `<path d="M250 0L0 300h250z"/>`],
}
```

**For shield types:**

Simply define a css [clip-path polygon](https://bennettfeely.com/clippy/) and add it in the "ruleset". Just make sure the units are in percentage (%).

For complex forms, I suggest you do the following:

1. Define your element just like the first step for divisions, ordinaries or charges.
2. visit this [online tool](https://betravis.github.io/shape-tools/path-to-polygon/), upload your svg, use the following settings :
   divideXBy: 2.5
   divideYBy: 3
   units: %
   precision: 9 or 10
   , and retrieve the code at the bottom of the page.
3. copy the path in the corresponding section of the "ruleset", like so:

```
wankel: {
    value: [
    `polygon(0.000% 11.684%, 0.000% 62.073%, 2.148% 73.192%, 7.812% 81.832%, 15.820% 88.299%, 25.000% 92.900%, 34.180% 95.944%, 42.187% 97.738%, 50.000% 98.803%, 57.812% 97.738%, 65.820% 95.944%, 75.000% 92.900%, 84.180% 88.299%, 92.188% 81.832%, 97.852% 73.192%, 100.000% 62.073%, 100.000% 11.684%, 50.000% -0.118%, 0.000% 11.684%)`,
    ],
},
```

## FAQ

**Why isn't the component accepting svg files as props instead?**

For portability reasons, and also to reduce the load time of the components when used in bulk.
Since svg's xml is very lightweight, I find it very practical to simply store it as a string in a javascript object.

**My project is not based on vue-cli, how can I use your component?**

Since Vuejs is only 60K, I suggest you use it along with http-vue-loader.
Start with this template, then follow the installation/usage instruction.

In your project header add the following:
```
<head>
  <script src="https://unpkg.com/vue"></script>
  <script src="https://unpkg.com/http-vue-loader"></script>
</head>
```
Then, anywhere in your project, add a vue instance
```
  <div id="heraldry">
    <Heraldry />
  </div>

  <script>
    new window.Vue({
      el: '#heraldry',

      components: {
        'Heraldry': window.httpVueLoader('/js/path/to/Heraldry.vue')
      },
    })
  </script>
</body>
</html>
```
