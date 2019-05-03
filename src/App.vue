<template>
  <div id="app">
    <nav-bar name="MyLabels" v-on:dataReceived="changeList"></nav-bar>

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
import UIkit from 'uikit'
import Icons from 'uikit/dist/js/uikit-icons'
import initList from './data/init.json'
import NavBar from './components/NavBar.vue'
import SetupModal from './components/SetupModal.vue'

UIkit.use(Icons)

export default {
  name: 'App',
  components: {
    NavBar,
    SetupModal
  },
  data: function () {
    return {
      imagePath: 'data/images/',
      items: initList
    }
  },
  methods: {
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
