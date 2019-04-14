<template>
  <div id="app">

    <div uk-sticky class="uk-sticky uk-sticky-fixed uk-active uk-sticky-below">
      <nav class="uk-navbar-container uk-margin uk-background-primary uk-light uk-navbar-transparent" uk-navbar>
        <div class="uk-navbar-left">
          <a class="uk-navbar-item uk-logo" href="#">{{ name }}</a>
        </div>
        <div class="uk-navbar-right">
          <div class="uk-navbar-item">
            <form action="javascript:void(0)">
              <div uk-form-custom>
                <input type="file" @change="changeData($event)" accept=".csv">
                <button class="uk-button uk-button-default" type="button" tabindex="-1">Open</button>
              </div>
              <button class="uk-button uk-button-default" type="button" uk-toggle="target: #modal">Setup</button>
              <button @click.prevent="print()" class="uk-button uk-button-default" type="button">Print</button>
            </form>
          </div>
        </div>
      </nav>
    </div>

    <div id="wellcome" class="container">
      <h1>Getting started</h1>
      <p>
        Welcome to MyLabels! This simple web-app helps you create collection of labels to organize your LEGO
        storage solution, that will allow you understand what you have and quickly find what you need.
      </p>
    </div>

    <div class="container" v-bind:class="{ border: isBorder }">
      <div v-for="item in items" :key="item.id" class="label">
        <h1>{{ item.id }}</h1>
        <p>{{ item.description }}</p>
        <div class="pic">
          <img v-for="picture in item.items.split(' ')" :key="picture.index" :src="imagePath + picture + '.png'" alt="">
        </div>
      </div>
    </div>

    <div id="help" class="container">
      <table class="uk-table uk-table-justify uk-table-divider">
        <thead>
          <tr>
            <th class="uk-width-small">Steps</th>
            <th>Descriptions</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Listing</td>
            <td>Download this <a href="data/examples.zip">archive</a> and edit csv files from it in any text editor. Each line of the file is a label record. Each record consists three fields, separated by commas: index number, description and images.</td>
          </tr>
          <tr>
            <td>Opening</td>
            <td>Open the result of the previous step in this application by clicking the [OPEN] button in navbar and selecting the csv file.</td>
          </tr>
          <tr>
            <td>Setting</td>
            <td>Click [SETUP] button to call the dialog window with the settings of the appearance.</td>
          </tr>
          <tr>
            <td>Printing</td>
            <td>Click [PRINT] button.</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div id="source" class="uk-position-fixed uk-position-medium uk-position-bottom-right">
      <a href="https://github.com/igorageev/mylabels" class="circle uk-button uk-button-primary uk-button-large" uk-icon="icon: github"></a>
    </div>

    <div id="modal" uk-modal>
      <div class="uk-modal-dialog uk-modal-body">
        <h2 class="uk-modal-title">Setup</h2>
        <form class="uk-grid-small" uk-grid>
          <div class="uk-width-1-2@s">
            <label class="uk-form-label" for="numbersize">
              ID font size
              <span class="uk-text-muted">(pt)</span>
            </label>
            <input v-model="layout.numberSize" @input="setupLayout" id="numbersize" class="uk-input" type="text"
              placeholder="22">
          </div>
          <div class="uk-width-1-2@s">
            <label class="uk-form-label" for="fontsize">
              Descriptions font size
              <span class="uk-text-muted">(pt)</span>
            </label>
            <input v-model="layout.textSize" @input="setupLayout" class="uk-input" type="text" placeholder="8">
          </div>

          <div class="uk-width-1-2@s">
            <label class="uk-form-label" for="numbersize">
              Top margin of the page
              <span class="uk-text-muted">(mm)</span>
            </label>
            <input v-model="layout.topMargin" @input="setupLayout" id="numbersize" class="uk-input" type="text"
              placeholder="22">
          </div>
          <div class="uk-width-1-2@s">
            <label class="uk-form-label" for="fontsize">
              Left margin of the page
              <span class="uk-text-muted">(mm)</span>
            </label>
            <input v-model="layout.leftMargin" @input="setupLayout" class="uk-input" type="text" placeholder="8">
          </div>

          <div class="uk-width-1-2@s">
            <label><input v-model="isBorder" class="uk-checkbox" type="checkbox" checked> Show border</label>
          </div>
        </form>
        <p class="uk-text-right">
          <button class="uk-button uk-button-primary uk-modal-close" type="button">Close</button>
        </p>
      </div>
    </div>

  </div>
</template>

<script>
import UIkit from 'uikit'
import Papa from 'papaparse'
import Icons from 'uikit/dist/js/uikit-icons'
import initList from './data/init.json'

UIkit.use(Icons)

var style = document.createElement('style')
document.body.append(style)

export default {
  name: 'App',
  data: function () {
    return {
      name: 'MyLabels',
      imagePath: 'data/images/',
      layout: {
        numberSize: 22,
        textSize: 8,
        topMargin: 22,
        leftMargin: 8
      },
      theme: 'border',
      isBorder: true,
      items: initList
    }
  },
  methods: {
    changeData: function (event) {
      // Check for the various File API support.
      if (window.FileReader && event.target.files) {
        var self = this
        // FileReader are supported.
        Papa.parse(event.target.files[0], {
          header: true,
          complete: function (results) {
            self.items = results.data
          }
        })
      } else {
        alert('FileReader are not supported in this browser.')
      }
    },
    setupLayout: function () {
      var lineHeightHead = this.layout.numberSize > 15 ? Math.floor(this.layout.numberSize * 0.85) : Math.floor(this.layout
        .numberSize * 1.2)
      var lineHeightText = this.layout.numberSize > 15 ? Math.floor(this.layout.textSize * 0.85) : Math.floor(this.layout
        .textSize * 1.2)
      style.innerText =
        '.label h1 {font-size: ' + this.layout.numberSize + 'pt; line-height: ' + lineHeightHead + 'pt;}' +
        '.label p {font-size: ' + this.layout.textSize + 'pt; line-height: ' + lineHeightText + 'pt;}' +
        '@media print { .container {margin: ' + this.layout.topMargin + 'mm ' + this.layout.leftMargin + 'mm;} }'
    },
    print: function () {
      window.print()
    }
  }
}

</script>

<style lang="less">
  @import "../node_modules/uikit/src/less/uikit.less";
  @import "./assets/less/theme.less";

  @font-face {
    font-family: 'Lobster';
    src: url('./assets/fonts/lobster-regular-webfont.woff2') format('woff2'), url('./assets/fonts/lobster-regular-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }

  @font-face {
    font-family: 'Roboto Condensed';
    src: url('./assets/fonts/robotocondensed-regular-webfont.woff2') format('woff2'),
      url('./assets/fonts/robotocondensed-regular-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }

  @font-face {
    font-family: 'Roboto Condensed';
    src: url('./assets/fonts/robotocondensed-bold-webfont.woff2') format('woff2'),
      url('./assets/fonts/robotocondensed-bold-webfont.woff') format('woff');
    font-weight: bold;
    font-style: bold;
  }

  .uk-logo {
    font-family: 'Lobster';
    font-size: 2rem;
    color: rgba(255, 255, 255, .7);
  }

  nav .uk-button {
    margin-right: .25rem;
  }

  html,
  body {
    width: 100%;
    margin: 0;
    padding: 0;
    font-family: Arial, Helvetica, sans-serif;
  }

  .container {
    width: 194.5mm;
    margin: 10mm auto;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
    border-top: 1pt dotted transparent;
    border-left: 1pt dotted transparent;
  }

  .border {
    border-top-color: lightgray;
    border-left-color: lightgray;
  }

  .label {
    width: 48.5mm;
    height: 25.4mm;
    padding: 1mm;
    position: relative;
    display: flex;
    box-sizing: border-box;
    border-bottom: 1pt dotted transparent;
    border-right: 1pt dotted transparent;
    overflow: hidden;
  }

  .border .label {
    border-bottom-color: lightgrey;
    border-right-color: lightgrey;
  }

  .label h1,
  .label p {
    margin: 0;
  }

  .label h1 {
    font-size: 22pt;
    font-family: 'Roboto Condensed';
    font-weight: bold;
    line-height: 26pt;
    z-index: 2;
    height: 2rem;
    display: flex;
    align-items: flex-end;
  }

  .label p {
    height: 1.8rem;
    padding: 0 0 0 2mm;
    font-size: 8pt;
    line-height: 10pt;
    z-index: 1;
    display: flex;
    align-items: flex-end;
  }

  .pic {
    position: absolute;
    width: 100%;
    height: 15mm;
    bottom: 0;
    left: 0;
    z-index: 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .pic img {
    height: 90%;
    width: auto;
    margin: 0 4pt;
  }

  .circle {
    border-radius: 50%;
    padding: 0;
    width: 55px;
  }

  @media print {

    .uk-sticky,
    .uk-sticky-placeholder,
    #help,
    #source,
    #wellcome {
      display: none !important;
    }

    .container {
      margin: 22mm 8mm;
    }
  }

</style>
