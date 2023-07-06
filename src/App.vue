<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <div @click="showPicker = !showPicker">切换pop</div>
    <!-- <vantMultSelect :showPicker="showPicker"></vantMultSelect> -->
    <abc :showPicker="showPicker"></abc>
  </div>
</template>

<script>
import "vant-mult-select/dist/vant-mult-select.css"
// import vantMultSelect from './components/vant-mult-select.vue';
import abc from 'vant-mult-select'
export default {
  name: 'App',
  components: {
    // vantMultSelect,
    abc
  },
  data() {
    return {
      showPicker: false
    }
  }
}
</script>

<style></style>
