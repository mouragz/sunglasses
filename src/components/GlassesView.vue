<template>
  <div class="glasses-container">
    <div class="glass-item" v-for="glass in glassesObj" :key="glass.id">
      <img alt="sunglasses" :src="glass.glass_variants[0].media[0].url" />
      <span class="glass-item-name">{{ glass.name }}</span>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "GlassesView",
  components: {},
  props: {
    configuration: {
      type: String,
      required: true,
      default: "spectacles-women",
    },
    colours: {
      type: Array,
      require: true,
      default: () => [],
    },
    shapes: {
      type: Array,
      require: true,
      default: () => [],
    },
  },
  data() {
    return {
      glassesObj: [],
      configuration_name: "spectacles-women",
      colours_filters: "",
      shapes_filters: "",
      number: 1,
      limit: 12,
    };
  },
  watch: {
    // It listens to the change in prop name
    configuration: function () {
      this.configuration_name = this.configuration;
      this.getGlasses();
    },
    colours: function () {
      this.colours_filters = this.colours;
      this.getGlasses();
    },
    shapes: function () {
      this.shapes_filters = this.shapes;
      this.getGlasses();
    },
  },
  mounted() {
    // Initially load some glasses.
    this.fetchData();
    this.scroll();
  },
  methods: {
    getGlasses(){
      this.number = 1;
      this.glassesObj= [];
      this.fetchData();
    },
    async fetchData() {
      await axios
        .get(
          `https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections/${this.configuration_name}/glasses?sort[type]=collection_relations_position&sort[order]=asc&filters[lens_variant_prescriptions][]=fashion&filters[lens_variant_types][]=classic&page[limit]=${this.limit}&page[number]=${this.number}&filters[frame_variant_home_trial_available]=true&${this.colours_filters}&${this.shapes_filters}&`
        )
        .then((response) => {
          if (response.data) {
            this.glassesObj.push(...response.data.glasses);
          }
        });
    },
    scroll() {
      window.onscroll = () => {
        let bottomOfWindow =
          Math.max(
            window.pageYOffset,
            document.documentElement.scrollTop,
            document.body.scrollTop
          ) +
            window.innerHeight >=
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          this.number++;
          this.fetchData();
        }
      };
    },
  },
};
</script>

<style scoped>
.glasses-container {
  width: 100%;
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  justify-items: center;
  align-items: flex-start;
}
.glasses-container > div {
  border-bottom: 1px solid #000;
  border-right: 1px solid #000;
  text-align: center;
}
.glass-item {
  position: relative;
  text-align: center;
}
.glass-item > img {
  width: 100%;
  height: auto;
}
.glass-item-name {
  font-style: normal;
  font-size: 20px;
  line-height: 24px;
  letter-spacing: 0.02em;
  text-transform: uppercase;
  position: absolute;
  top: 10%;
  left: 40%;
}
@media (max-width: 499.9px) and (min-width: 320px) {
  .glasses-container {
  width: 100%;
  display: grid;
  grid-template-columns: 50% 50% 50%;
  justify-items: center;
  align-items: flex-start;
}
}
</style>
