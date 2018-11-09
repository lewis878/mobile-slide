<template>
  <div id="app">
    <slide :slides="slides"></slide>
  </div>
</template>

<script>
  import {jsonp} from "./js/utils"

  import slide from './components/slide'

  export default {
    name: 'App',
    components: {
      slide
    },
    data() {
      return {
        slides: []
      }
    },
    created() {
      this.getSlideData()
    },
    methods: {
      getSlideData() {
        var url = `https://c.y.qq.com/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg?g_tk=5381&uin=0&format=json&inCharset=utf-8&outCharset=utf-8&notice=0&platform=h5&needNewCode=1&_=1541671098352`
        jsonp({
          url,
          param: 'jsonpCallback'
        }).then((res) => {
          this.slides = res.data.slider
        })
      }
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }

  body {
    background: #f5f5f5;
  }
</style>
