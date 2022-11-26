<template>
  <div class="main-header">
    <div class="sunglasses-header-btns">
      <a class="sunglasses-header-button">
        <span class="">MENU</span>
        <CollectionsMenu
          :showMenu="showMenu"
          @configuration-event="setConfigurationName"
        ></CollectionsMenu>
      </a>
    </div>
    <div class="sunglasses-header-logo">
      <img
        alt="sunglasses"
        class="logo"
        src="../assets/logo.png"
        width="75"
        height="50"
      />
    </div>
  </div>

  <div class="header container">
    <div class="category border">
      {{ configurationName.replace("-", " ") }}
    </div>
    <div class="menu-button border">
      <a class="dropdown">
        <span class="">COLOUR</span>
        <div class="dropdown-content">
          <ul class="dropdown-list">
            <li
              v-for="(colour, index) in coloures"
              :key="colour.id"
              @click="selectColour(colour, index)"
            >
              <span :class="`${colour.selected ? 'colour-item' : ''}`">{{
                colour.color_name
              }}</span>
            </li>
          </ul>
        </div>
      </a>
      <a class="dropdown">
        <span class="">SHAPE</span>
        <div class="dropdown-content">
          <ul class="dropdown-list">
            <li
              v-for="(shape, index) in shapes"
              :key="shape.id"
              @click="selectShape(shape, index)"
            >
              <span :class="`${shape.selected ? 'shape-item' : ''}`">{{
                shape.shape_name
              }}</span>
            </li>
          </ul>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
import CollectionsMenu from "./CollectionsMenu.vue";
// import { isMobile } from "./utils/helpers.js";

export default {
  title: "Header",
  components: { CollectionsMenu },
  data() {
    return {
      showMenu: false,
      configurationName: "spectacles-women",
      selectedColors: [],
      selectedShapes: [],
      coloursFilters: "",
      shapesFilters: "",
      coloures: [
        { color_name: "black", selected: false },
        { color_name: "tortoise", selected: false },
        { color_name: "coloured", selected: false },
        { color_name: "crystal", selected: false },
        { color_name: "dark", selected: false },
        { color_name: "bright", selected: false },
      ],
      shapes: [
        { shape_name: "square", selected: false },
        { shape_name: "rectangle", selected: false },
        { shape_name: "round", selected: false },
        { shape_name: "cat-eye", selected: false },
      ],
    };
  },
  // computed: {
  //   isMobile,
  // },
  methods: {
    setConfigurationName: function (gender) {
      this.configurationName = gender.configuration_name;
      this.$emit("send-configuration-name-event", this.configurationName);
    },
    selectColour: function (colour, index) {
      if (this.coloures[index].selected) {
        this.coloures[index].selected = false;
        let i = this.selectedColors.indexOf(colour);
        if (i !== -1) {
          this.selectedColors.splice(i, 1);
        }
      } else {
        this.coloures[index].selected = true;
        this.selectedColors.push(colour);
      }
      this.constructColourFilter();
    },
    selectShape: function (shape, index) {
      if (this.shapes[index].selected) {
        this.shapes[index].selected = false;
        let i = this.selectedShapes.indexOf(shape);
        if (i !== -1) {
          this.selectedShapes.splice(i, 1);
        }
      } else {
        this.shapes[index].selected = true;
        this.selectedShapes.push(shape);
      }
      this.constructShapeFilter();
    },
    constructColourFilter: function () {
      this.coloursFilters = " ";
      this.selectedColors.forEach(
        (color) =>
          (this.coloursFilters += `filters[glass_variant_frame_variant_colour_tag_configuration_names][]=${color.color_name}&`)
      );
      this.$emit("send-colors-name-event", this.coloursFilters);
    },
    constructShapeFilter: function () {
      this.shapesFilters = " ";
      this.selectedShapes.forEach(
        (shape) =>
          (this.shapesFilters += `filters[glass_variant_frame_variant_frame_tag_configuration_names][]=${shape.shape_name}&`)
      );
      this.$emit("send-shapes-name-event", this.shapesFilters);
    },
  },
};
</script>

<style>
.shape-item,
.colour-item {
  position: relative;
}
.shape-item:after,
.colour-item:after {
  content: "";
  position: absolute;
  left: 0;
  top: 100%;
  width: 0;
  height: 1px;
  background-color: black;
  width: 100%;
}
ul {
  list-style-type: none;
  padding-inline-start: 0px;
}
.dropdown {
  position: relative;
  display: inline-block;
}
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  border: 1px solid #000;
  z-index: 1;
}

.dropdown:hover .dropdown-content {
  display: block;
}

.dropdown-list > li:hover {
  color: white;
  background-color: black;
}

.sunglasses-header-button:hover .menu-container {
  left: 0;
}
.main-header {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #000;
  background-color: #fff;
  box-sizing: border-box;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #000;
  text-align: center;
}
.header > div {
  width: 33.33%;
}
.category {
  font-weight: bold;
  font-size: 25px;
  margin-inline-start: auto;
  text-transform: uppercase;
}
.cat {
  font-weight: bold;
  font-size: 25px;
  border-right: 1px solid #000;
}
.sunglasses-header-button {
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  text-align: center;
  width: 100px;
  font-style: normal;
  font-weight: 400;
  font-size: 16px;
}
.sunglasses-header-button:hover {
  text-decoration: underline;
}
.sunglasses-header-logo {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
}
.sunglasses-header-btns {
  display: flex;
  height: 100%;
}
.menu-button {
  display: flex;
}
.border {
  border-left: 1px solid #000;
}
.menu-button > a {
  border-right: 1px solid #000;
  justify-content: center;
  align-items: center;
  text-align: center;
  cursor: pointer;
  width: 100px;
  font-weight: 400;
  font-size: 16px;
  line-height: 40px;
}
@media (max-width: 815px) and (min-width: 498px) {
  .category {
    font-size: 20px;
  }
}
@media (max-width: 499.9px) and (min-width: 320px) {
  .sunglasses-header-button {
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    text-align: center;
    width: 100px;
    font-style: normal;
    font-weight: 400;
    font-size: 12px;
  }
  .sunglasses-header-logo {
    margin: 0 15%;
  }
  .category {
    font-size: 16px;
    margin-inline-start: 0;
  }
  .header > div {
    width: 50%;
  }
}
</style>
