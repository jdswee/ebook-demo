<template>
  <div>ebook</div>
</template>

<script>
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  methods: {
    // 电子书的解析和渲染
    showEpub () {
      // 生成 Book
      this.book = new Epub(DOWNLOAD_URL)
      console.log(this.book)
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
  @import 'assets/styles/global.scss';
</style>
