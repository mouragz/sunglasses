<template>
  <Header
    @send-configuration-name-event="sendConfigurationName"
    @send-colors-name-event="sendColorsName"
    @send-shapes-name-event="sendShapesName"
  />
  <GlassesView
    :configuration="configuration_name"
    :colours="colours_filters"
    :shapes="shapes_filters"
  ></GlassesView>
</template>

<script>
import Header from "./Header.vue";
import GlassesView from "./GlassesView.vue";
export default {
  title: "MainView",
  components: { Header, GlassesView },
  data() {
    return {
      configuration_name: "",
      colours_filters: "",
      shapes_filters: "",
    };
  },
  methods: {
    sendConfigurationName: function (configuration_name) {
      this.configuration_name = configuration_name;
    },
    sendColorsName: function (colours_filters) {
      this.colours_filters = colours_filters;
    },
    sendShapesName: function (shapes_filters) {
      this.shapes_filters = shapes_filters;
    }
  },
};
</script>

<style scoped></style>
