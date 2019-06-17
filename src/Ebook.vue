<template>
  <div class="ebook">
    <title-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggelTitleMenuShow"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar
    :ifTitleAndMenuShow="ifTitleAndMenuShow"
    :fontSizeList="fontSizeList"
    :defaultFontSize="defaultFontSize"
    @setFontSize="setFontSize"
    :themeList="themeList"
    :defaultTheme="defaultTheme"
    @setTheme="setTheme"
    ref="menuBar"></menu-bar>
  </div>
</template>

<script>
import Epub from 'epubjs'
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
const DOWNLOAD_URL = '/static/2018_Book_AgileProcessesInSoftwareEngine.epub'

export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      ifTitleAndMenuShow: false,
      fontSizeList: [
        {fontSize: 12},
        {fontSize: 14},
        {fontSize: 16},
        {fontSize: 18},
        {fontSize: 20},
        {fontSize: 22},
        {fontSize: 24}
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {'color': '#000', 'background': '#fff'}
          }
        },
        {
          name: 'eye',
          style: {
            body: {'color': '#000', 'background': '#ceeaba'}
          }
        },
        {
          name: 'night',
          style: {
            body: {'color': '#fff', 'background': '#000'}
          }
        },
        {
          name: 'gold',
          style: {
            body: {'color': '#000', 'background': 'rgb(241, 236, 226)'}
          }
        }
      ],
      defaultTheme: 0
    }
  },
  methods: {
    setTheme(index) {
      this.themes.select(this.themeList[index].name)
      this.defaultTheme = index
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style)
      })
    },
    toggelTitleMenuShow() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      if(!this.ifTitleAndMenuShow) {
        this.$refs.menuBar.hideSetting()
      }
    },
    // 电子书的解析和渲染
    showEpub() {
      // 生成 Book
      this.book = new Epub(DOWNLOAD_URL)
      // 通过 Book.renderTo 生成 Rendition
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 通过 Rendition.display 渲染电子书
      this.rendition.display()
      // 获取Theme对象
      this.themes = this.rendition.themes
      // 设置默认字体
      this.setFontSize(this.defaultFontSize)
      this.registerTheme()
      this.setTheme(this.defaultTheme)
    },
    prevPage() {
      if(this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      if(this.rendition) {
        this.rendition.next()
      }
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if(this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    }
  },
  mounted() {
    this.showEpub()
  }
}
</script>

<style lang='scss' scoped>
  @import 'assets/styles/global.scss';
  .ebook {
    position: relative;

    .read-wrapper {
      .mask {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 100;
        display: flex;
        width: 100%;
        height: 100%;
        .left {
          flex: 0 0 px2rem(100);
        }
        .center {
          flex: 1;
        }
        .right {
          flex: 0 0 px2rem(100);
        }
      }
    }

  }
</style>
