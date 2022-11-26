<template>
  <div>
    <div class="menu-container" :style="{ 'left: -200px': showMenuList }">
      <ul class="menu-active list">
        <li
          class="menu-active-level-one"
          v-for="(item, key) in groupedCollections"
          :key="item.id"
        >
          {{ key }}
          <svg width="11" height="16" viewBox="0 0 11 16" fill="none">
            <path
              d="M1 1L9 8.2L1 15.4"
              stroke="black"
              stroke-opacity="0.85"
              stroke-width="1.5"
            ></path>
          </svg>
          <ul class="menu-active-list list">
            <li
              class="menu-active-level-two"
              v-for="(gender, index) in item"
              :key="gender.id"
              @click="selectCollection(gender)"
            >
              {{ item[index].label }}
            </li>
          </ul>
        </li>
        <li class="menu-active-level-one">HOME TRY ON</li>
        <li class="menu-active-level-one">PAIR FOR PAIR</li>
      </ul>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import { groupBy } from "./utils/helpers.js";
export default {
  title: "CollectionsMenu",
  data() {
    return {
      showMenuList: false,
      collections: [],
      groupedCollections: [],
    };
  },
  mounted: function () {
    axios
      .get(
        "https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections"
      )
      .then((response) => {
        if (response.data) {
          this.collections = response.data.collections;
          this.getFiltteredCollections();
        }
      });
  },
  methods: {
    groupBy,
    selectCollection: function (gender) {
      this.$emit("configuration-event", gender);
    },
    getFiltteredCollections: function () {
      const newCollections = this.collections.map((item) => ({
        ...item,
        label: item.name.split(" ")[1],
        collection: item.name.split(" ")[0],
      }));
      this.groupedCollections = this.groupBy(newCollections, "collection");
    },
  },
};
</script>
<style>
.menu-container,
.menu-container.show {
  position: fixed;
  width: 200px;
  height: calc(100vh - 52px);
  top: 52px;
  left: -200px;
  z-index: 1024;
  transition: left 0.5s ease-in-out;
}
.menu-container:before {
  content: "";
  background: #fff;
  border-right: 1px solid #000;
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: -1;
  left: 0;
  top: 0;
}
.list {
  margin: 0;
  padding: 0;
  list-style: none;
  position: static;
}
.list li {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #000;
  padding: 10px;
  position: static;
  text-transform: Uppercase;
}
.menu-active-list {
  position: absolute;
  width: 200px;
  height: 100%;
  top: 0;
  left: -200px;
  background: #fff;
  z-index: -2;
  border-right: 1px solid #000;
  transition: left 0.5s ease-in-out;
}
.list li:hover > .menu-active-list {
  left: 200px;
}
.menu-active-level-two:hover {
  background-color: black;
  color: white;
}
</style>
