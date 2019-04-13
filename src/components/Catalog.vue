<template>
  <div class="container" v-bind:class="{ border: isBorder }">
    <div v-for="item in items" class="label">
      <h1>{{ item.id }}</h1>
      <p>{{ item.description }}</p>
      <div class="pic">
        <!-- <img v-for="picture in item.items.split(' ')" :src="imagePath + picture + '.png'" alt=""> -->
      </div>
    </div>
  </div>
</template>

<script>
  import Papa from 'papaparse'
  export default {
    name: '',
    props: {
      name: String
    },
    data: function () {
      return {
        imagePath: 'items/',
        items: [{
            id: '01',
            description: 'Plate 1x1',
            items: '3024'
          },
          {
            id: '02',
            description: 'Plate 1x2',
            items: '15573 3023'
          },
          {
            id: '03',
            description: 'Plate 1x3',
            items: '3623'
          },
          {
            id: '04',
            description: 'Plate 1x4',
            items: '3710'
          },
          {
            id: '05',
            description: 'Brick 1x1',
            items: '3005'
          },
          {
            id: '06',
            description: 'Brick 1x2',
            items: '3004'
          },
          {
            id: '07',
            description: 'Brick 1x3',
            items: '3622'
          },
          {
            id: '08',
            description: 'Brick 1x4',
            items: '3010'
          },
          {
            id: '09',
            description: 'Plate 2x2 with 1 stud',
            items: '87580'
          },
          {
            id: '10',
            description: 'Plate 2x2',
            items: '3022'
          },
          {
            id: '11',
            description: 'Plate 2x3',
            items: '3021'
          },
          {
            id: '12',
            description: 'Plate 2x4',
            items: '8200'
          }
        ]
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
        var lineHeightHead = this.layout.numberSize > 15 ? Math.floor(this.layout.numberSize * 0.85) : Math.floor(this.layout
          .numberSize * 1.2);
        var lineHeightText = this.layout.numberSize > 15 ? Math.floor(this.layout.textSize * 0.85) : Math.floor(this.layout
          .textSize * 1.2);
        style.innerText =
          '.label h1 {font-size: ' + this.layout.numberSize + 'pt; line-height: ' + lineHeightHead + 'pt;}' +
          '.label p {font-size: ' + this.layout.textSize + 'pt; line-height: ' + lineHeightText + 'pt;}';
      }
    }
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
  .container {
    width: 194.5mm;
    margin: 23mm auto;
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
.label h1, .label p {
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

</style>
