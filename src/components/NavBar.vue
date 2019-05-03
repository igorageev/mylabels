<template>
  <div class="uk-active uk-position-top uk-position-fixed">
    <nav
      class="uk-navbar-container uk-margin uk-background-primary uk-light uk-navbar-transparent"
      uk-navbar>

      <div class="uk-navbar-left">
        <a class="uk-navbar-item uk-logo" href="#">{{ name }}</a>
      </div>

      <div class="uk-navbar-right">
        <div class="uk-navbar-item">
          <form action="javascript:void(0)">

            <div uk-form-custom>
              <input type="file" @change="parse($event)" accept=".csv">
              <button
                class="uk-button uk-button-default"
                type="button"
                tabindex="-1">
                Open
              </button>
            </div>

            <button
              uk-toggle="target: #modal"
              class="uk-button uk-button-default"
              type="button"
              >
              Setup
            </button>

            <button
              @click.prevent="print()"
              class="uk-button uk-button-default"
              type="button">
              Print
            </button>

          </form>
        </div>
      </div>

    </nav>
  </div>
</template>

<script>
import Papa from 'papaparse'

export default {
  name: 'NavBar',
  props: {
    name: String
  },
  methods: {
    /**
     * Get data from CSV file
     * @param {Object} event - Event Object of file input element
     */
    parse: function (event) {
      // Check for the various File API support
      if (window.FileReader && event.target.files) {
        // FileReader are supported
        var self = this
        Papa.parse(event.target.files[0], {
          header: true,
          complete: function (results) {
            self.$emit('fileParsed', results.data)
          }
        })
      } else {
        alert('FileReader are not supported in this browser.')
      }
    },
    /* No comment */
    print: function () {
      window.print()
    }
  }
}
</script>
