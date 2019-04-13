<template>
  <div id="modal" uk-modal>
    <div class="uk-modal-dialog uk-modal-body">
      <h2 class="uk-modal-title">Setup</h2>
      <form class="uk-grid-small" uk-grid>
        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="numbersize">Numbers font size</label>
          <input v-model="layout.numberSize" @input="setupLayout" id="numbersize" class="uk-input" type="text"
            placeholder="22">
        </div>
        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="fontsize">Descriptions font size</label>
          <input v-model="layout.textSize" @input="setupLayout" class="uk-input" type="text" placeholder="8">
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
</template>

<script>
  export default {
    data: function () {
      return {
        layout: {
          numberSize: 22,
          textSize: 8
        },
        theme: 'border',
        isBorder: true
      }
    },
    methods: {
      changeData: function (event) {
        // Check for the various File API support.
        if (window.FileReader && event.target.files) {
          var self = this;
          // FileReader are supported.
          Papa.parse(event.target.files[0], {
            header: true,
            complete: function (results) {
              self.items = results.data;
            }
          });
        } else {
          alert('FileReader are not supported in this browser.');
        }
      },
      setupLayout() {
        var lineHeightHead = this.layout.numberSize > 15 ? Math.floor(this.layout.numberSize * 0.85) : Math.floor(this.layout.numberSize * 1.2)
        var lineHeightText = this.layout.numberSize > 15 ? Math.floor(this.layout.textSize * 0.85) : Math.floor(this.layout.textSize * 1.2)
        style.innerText =
          '.label h1 {font-size: ' + this.layout.numberSize + 'pt; line-height: ' + lineHeightHead + 'pt;}' +
          '.label p {font-size: ' + this.layout.textSize + 'pt; line-height: ' + lineHeightText + 'pt;}'
      }
    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

</style>
