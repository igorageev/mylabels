<template>
  <div id="modal" uk-modal>
    <div class="uk-modal-dialog uk-modal-body">
      <h2 class="uk-modal-title">Setup</h2>
      <form class="uk-grid-small" uk-grid>

        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="rowWidth">
            Row width
            <span class="uk-text-muted">(mm)</span>
          </label>
          <input
            v-model="layout.containerWidth"
            @input="setupLayout"
            id="rowWidth"
            class="uk-input"
            type="text"
            placeholder="194.5"
          >
        </div>
        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="labelPadding">
            Label padding
            <span class="uk-text-muted">(mm)</span>
          </label>
          <input
            v-model="layout.labelPadding"
            @input="setupLayout"
            id="labelPadding"
            class="uk-input"
            type="text"
            placeholder="1"
          >
        </div>

        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="labelWidth">
            Label width
            <span class="uk-text-muted">(mm)</span>
          </label>
          <input
            v-model="layout.labelWidth"
            @input="setupLayout"
            id="labelWidth"
            class="uk-input"
            type="text"
            placeholder="48.5"
          >
        </div>
        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="labelHeight">
            Label height
            <span class="uk-text-muted">(mm)</span>
          </label>
          <input
            v-model="layout.labelHeight"
            @input="setupLayout"
            id="labelHeight"
            class="uk-input"
            type="text"
            placeholder="48.5"
          >
        </div>

        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="topMargin">
            Top margin of the page
            <span class="uk-text-muted">(mm)</span>
          </label>
          <input
            v-model="layout.topMargin"
            @input="setupLayout"
            id="topMargin"
            class="uk-input"
            type="text"
            placeholder="22"
          >
        </div>
        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="leftMargin">
            Left margin of the page
            <span class="uk-text-muted">(mm)</span>
          </label>
          <input
            v-model="layout.leftMargin"
            @input="setupLayout"
            id="leftMargin"
            class="uk-input"
            type="text"
            placeholder="8"
          >
        </div>

        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="numbersize">
            ID font size
            <span class="uk-text-muted">(pt)</span>
          </label>
          <input
            v-model="layout.numberSize"
            @input="setupLayout"
            id="numbersize"
            class="uk-input"
            type="text"
            placeholder="22"
          >
        </div>
        <div class="uk-width-1-2@s">
          <label class="uk-form-label" for="textSize">
            Descriptions font size
            <span class="uk-text-muted">(pt)</span>
          </label>
          <input
            v-model="layout.textSize"
            @input="setupLayout"
            id="textSize"
            class="uk-input"
            type="text"
            placeholder="8"
          >
        </div>

        <div class="uk-width-1-2@s">
          <label>
            <input v-model="isBorder" class="uk-checkbox" @change="setupLayout" type="checkbox" checked> Show border
          </label>
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
  name: 'SetupModal',
  data: function () {
    return {
      layout: {
        containerWidth: 194.5,
        labelWidth: 48.5,
        labelHeight: 25.4,
        labelPadding: 1,
        numberSize: 22,
        textSize: 8,
        topMargin: 22,
        leftMargin: 8
      },
      isBorder: true,
      style: document.createElement('style')
    }
  },
  computed: {
    containerBorder: function () {
      return this.isBorder
        ? 'border-top-color: lightgray; border-left-color: lightgray;'
        : ''
    },
    labelBorder: function () {
      return this.isBorder
        ? 'border-bottom-color: lightgrey;border-right-color: lightgrey;'
        : ''
    },
    labelSize: function () {
      return this.layout.labelWidth
        ? 'width: ' + this.layout.labelWidth + 'mm; ' +
          'height: ' + this.layout.labelHeight + 'mm;'
        : ''
    },
    rowWidth: function () {
      return this.layout.containerWidth
        ? 'width: ' + this.layout.containerWidth + 'mm;'
        : ''
    },
    labelPadding: function () {
      return this.layout.labelPadding
        ? 'padding: ' + this.layout.labelPadding + 'mm;'
        : ''
    },
    paragraphPadding: function () {
      return this.layout.labelPadding
        ? 'padding-left: ' + this.layout.labelPadding + 'mm;'
        : ''
    }
  },
  created: function () {
    document.body.append(this.style)
    this.setupLayout()
  },
  methods: {
    /* Updates style injection for customizing layout */
    setupLayout: function () {
      this.style.innerText =
        '.container {' + this.rowWidth + '}' +
        '.border {' + this.containerBorder + '}' +
        '.label {' + this.labelPadding + this.labelSize + this.labelBorder + '}' +
        '.label h1 {font-size: ' + this.layout.numberSize + 'pt; line-height: ' + this.layout.numberSize + 'pt;}' +
        '.label p {font-size: ' + this.layout.textSize + 'pt; ' + this.paragraphPadding + '}' +
        '@media print { .container {margin: ' + this.layout.topMargin + 'mm ' + this.layout.leftMargin + 'mm;} }'
    }
  }
}
</script>
