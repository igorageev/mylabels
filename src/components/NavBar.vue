<template>
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
          <button v-on:click="window.print()" class="uk-button uk-button-default" type="button">Print</button>
        </form>
      </div>
    </div>
  </nav>
</template>

<script>
  export default {
    name: '',
    props: {
      name: String
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
  @font-face {
    font-family: 'Lobster';
    src: url('../assets/fonts/lobster-regular-webfont.woff2') format('woff2'), url('../assets/fonts/lobster-regular-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }
  
  .uk-logo {
    font-family: 'Lobster';
    font-size: 2rem;
    color: rgba(255, 255, 255, .7);
  }
  
  nav button {
    margin-right: .25rem;
  }
</style>
