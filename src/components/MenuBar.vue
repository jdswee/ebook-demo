<template>
  <!-- 菜单栏 -->
  <div class="menu-bar">
    <transition name="slide-up">
      <div class="menu-wrapper"
      :class="{'hide-box-shadow': ifSettingShow || !ifTitleAndMenuShow}"
      v-show="ifTitleAndMenuShow">
        <div class="icon-wrapper">
          <span class="icon-menu icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon-progress icon"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting(1)">
          <span class="icon-bright icon"></span>
        </div>
        <div class="icon-wrapper" @click="showSetting(0)">
          <span class="icon-a icon">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up">
      <div class="setting-wrapper" v-show="ifSettingShow">
        <div class="setting-font-size" v-if="showTag === 0">
          <div class="preview"
            :style="{fontSize: fontSizeList[0].fontSize + 'px'}">A</div>
          <div class="select">
            <div class="select-wrapper"
              v-for="(item, index) in fontSizeList"
              :key="index"
              @click="setFontSize(item.fontSize)">
              <div class="line"></div>
              <div class="point-wrapper">
                <div class="point"
                  v-show="defaultFontSize === item.fontSize">
                  <div class="small-point"></div>
                </div>
              </div>
              <div class="line"></div>
            </div>
          </div>
          <div class="preview"
            :style="{fontSize: fontSizeList[fontSizeList.length - 1].fontSize + 'px'}">A</div>
        </div>
        <!-- 设置主题 -->
        <div class="setting-theme" v-if="showTag === 1">
          <div class="setting-theme-item"
            v-for="(item,index) in themeList"
            :key="index"
            @click="setTheme(index)">
            <div class="preview"
              :style="{background: item.style.body.background}"
              :class="{'no-border': item.style.body.background !== '#fff'}">
            </div>
            <div class="text" :class="{selected: index === defaultTheme}">{{ item.name }}</div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    ifTitleAndMenuShow: {
      type: Boolean,
      default: false
    },
    fontSizeList: Array,
    defaultFontSize: Number,
    themeList: Array,
    defaultTheme: Number
  },
  data() {
    return {
      ifSettingShow: false,
      showTag: 0
    }
  },
  methods: {
    showSetting(tag) {
      this.ifSettingShow = true
      this.showTag = tag
    },
    hideSetting() {
      this.ifSettingShow = false
    },
    setFontSize(fontSize) {
      this.$emit('setFontSize', fontSize)
    },
    setTheme(index) {
      this.$emit('setTheme', index)
    }
  }
}
</script>

<style lang='scss' scoped>
@import '../assets/styles/global';
.menu-bar {
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: px2rem(48);
    background: white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    .icon-wrapper {
      flex: 1;
      @include center;
    }
    &.hide-box-shadow {
      box-shadow: none;
    }
  }
  .setting-wrapper {
    position: absolute;
    bottom: px2rem(48);
    left: 0;
    width: 100%;
    height: px2rem(60);
    background: #fff;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    z-index: 101;
    .setting-font-size {
      display: flex;
      height: 100%;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select {
        display: flex;
        flex: 1;
        .select-wrapper {
          flex: 1;
          display: flex;
          align-items: center;
          &:first-child {
            .line {
              &:first-child {
                border-top: none;
              }
            }
          }
          &:last-child {
            .line {
              &:last-child {
                border-top: none;
              }
            }
          }
          .line {
            flex: 1;
            height: 0;
            border-top: px2rem(1) solid #ccc;
          }
          .point-wrapper {
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: px2rem(7);
            border-left: px2rem(1) solid #ccc;
            .point {
              position: absolute;
              left: px2rem(-10);
              top: px2rem(-8);
              width: px2rem(20);
              height: px2rem(20);
              background: white;
              border: px2rem(1) solid #ccc;
              border-radius: 50%;
              box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, .15);
              @include center;
              .small-point {
                width: px2rem(5);
                height: px2rem(5);
                border-radius: 50%;
                background: #000;
              }
            }
          }
        }
      }
    }
    .setting-theme {
      height: 100%;
      display: flex;
      .setting-theme-item {
        flex: 1;
        display: flex;
        flex-direction: column;
        padding: px2rem(5);
        box-sizing: border-box;
        .preview {
          flex: 1;
          border: px2rem(1) solid #ccc;
          box-sizing: border-box;
          &.no-border {
            border: none;
          }
        }
        .text {
          flex: 0 0 px2rem(20);
          font-size: px2rem(14);
          color: #ccc;
          text-align: center;
          &.selected {
            color: #333;
          }
        }
      }
    }
  }
}
</style>
