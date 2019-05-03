<template>
  <div id="app">
    <nav-bar name="MyLabels" v-on:fileParsed="changeList"></nav-bar>

    <div class="container border">
      <div v-for="item in items" :key="item.id" class="label">
        <h1>{{ item.id }}</h1>
        <p>{{ item.description }}</p>
        <div class="pic">
          <img
            v-for="picture in item.items.split(' ')"
            :key="picture.index"
            :src="imagePath + picture + '.png'"
            alt
          >
        </div>
      </div>
    </div>

    <setup-modal></setup-modal>
  </div>
</template>

<script>
import UIkit from 'uikit' // Front-end framework
import Icons from 'uikit/dist/js/uikit-icons' // Icon library
import initList from './data/init.json' // Initial parts list
// Components
import NavBar from './components/NavBar.vue' // For user interaction
import SetupModal from './components/SetupModal.vue' // For setting layout

UIkit.use(Icons) // Loads the Icon plugin

export default {
  name: 'App',
  components: {
    NavBar,
    SetupModal
  },
  data: function () {
    return {
      items: initList, // List of parts
      imagePath: 'data/images/' // Directory with pictures of parts
    }
  },
  methods: {
    /**
     * Updates list of parts
     * @param {Array} newList - List of parts
     */
    changeList: function (newList) {
      this.items = newList
    }
  }
}
</script>

<style lang="less">
@import "../node_modules/uikit/src/less/uikit.less";
@import "./assets/less/theme.less";
@import "./assets/less/mylabels.less";
</style>
