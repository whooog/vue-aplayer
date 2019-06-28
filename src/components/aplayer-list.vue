<template>
  <transition name="slide-v">
    <div
      class="aplayer-list"
      :style="listHeightStyle"
      ref="list"
      v-show="show"
    >
     <div class="aplayer-list-header">
       <span class="aplayer-list-tabs" :class="{active :cur==0}" @click="cur=0">播放列表</span>
       <span class="aplayer-list-tabs" :class="{active :cur==1}" @click="cur=1">历史纪录</span>
     </div>
      <ol
        ref="ol"
        style="height: 555px"
        v-show="cur==0">
        <div class="aplayer-list-detail">
          <span class="aplayer-list-totalMusic">总{{musicList.length}}首</span>
          <span class="aplayer-list-collection">
            <icon-button
              class="aplayer-icon-mode"
              icon="favorites-folder"
            />
            收藏全部
          </span>|
          <span class="aplayer-list-empty" @click="clean">
            <icon-button
              icon="clear"
              class="aplayer-icon-mode"
            />
            清空
          </span>
        </div>
        <li
          v-for="(aMusic, index) of musicList"
          :key="index"
          :class="{'aplayer-list-light': aMusic === currentMusic}"
          @click="$emit('selectsong', aMusic)"
        >
          <span class="aplayer-list-cur" :style="{background: theme}"></span>
          <span class="aplayer-list-index">{{ index + 1}}</span>
          <span class="aplayer-list-title">{{ aMusic.title || 'Untitled' }}</span>
          <span class="aplayer-list-author">{{ aMusic.artist || aMusic.author || 'Unknown' }}</span>
        </li>
      </ol>

      <ol v-show="cur==1">
        <li>fdfsdsfd</li>
      </ol>
    </div>
  </transition>
</template>

<script>
  import IconButton from './aplayer-iconbutton.vue'
  export default {
    data () {
      return {
        cur : 0
      }
    },
    components:{
      IconButton
    },
    props: {
      show: {
        type: Boolean,
        default: true,
      },
      currentMusic: Object,
      musicList: {
        type: Array,
        default () {
          return []
        }
      },
      playIndex: {
        type: Number,
        default: 0,
      },
      theme: String,
      listmaxheight: String,
    },
    computed: {
      listHeightStyle () {
        return {
          height: `600px`,
          maxHeight: this.listmaxheight || ''
        }
      }
    },
    methods: {
      clean () {
         this.musicList=''
      }
    }
  }
</script>

<style lang="scss">
  .aplayer-list-header {
    font-size: 0;
    height: 45px;
    line-height: 45px;
    background-color: #F4F4F6;
    .aplayer-list-tabs {
        color: #7D7E86;
        padding: 5px 20px;
        border: 1px solid #e8e4e4;
        font-size: 14px;

    }
    .active{
      background-color: #7D7E86;
      color: #fff;
      border: 1px solid #7D7E86;
    }
  }
  .aplayer-list {
    overflow: hidden;
    width: 580px;
    position: absolute;
    top: -600px;
    background: #f7f5f5;
    right: 0px;

    &.slide-v-enter-active,
    &.slide-v-leave-active {
      /*transition: height 500ms ease;*/
      will-change: height;
    }

    &.slide-v-enter,
    &.slide-v-leave-to {
      height: 0 !important;
    }

    ol {
      list-style-type: none;
      margin: 0;
      padding: 0;
      overflow-y: auto;

      &::-webkit-scrollbar {
        width: 5px;
      }

      &::-webkit-scrollbar-track {
        background-color: #f9f9f9;
      }

      &::-webkit-scrollbar-thumb {
        border-radius: 3px;
        background-color: #eee;
      }

      &::-webkit-scrollbar-thumb:hover {
        background-color: #ccc;
      }

      &:hover {
        li.aplayer-list-light:not(:hover) {
          background-color: inherit;
          transition: inherit;
        }
      }

      &:not(:hover) {
        li.aplayer-list-light {
          transition: background-color .6s ease;
        }
      }
      .aplayer-list-detail{
        border-top: 1px solid #e9e9e9;
        border-bottom: 1px solid #e9e9e9;
        height: 35px;
        line-height: 35px;
        background-color: #F9F9F9;
         span {
           font-size: 12px;
         }
        .aplayer-list-collection{
          margin-left: 375px;
          padding-right: 10px;

        }

        .aplayer-list-empty{
          padding-left:10px;

        }
        .aplayer-icon-mode{
          width: 14px;
          height: 14px;
        }
      }
      li {
        position: relative;
        height: 32px;
        line-height: 32px;
        padding: 0 15px;
        font-size: 12px;
        border-top: 1px solid #e9e9e9;
        cursor: pointer;
        transition: all 0.2s ease;
        overflow: hidden;
        margin: 0;
        text-align: start;
        display: flex;

        &:first-of-type {
          border-top: none;
        }

        &:hover {
          background: #efefef;
        }

        &.aplayer-list-light {
          background: #efefef;

          .aplayer-list-cur {
            display: inline-block;
          }
        }

        .aplayer-list-cur {
          display: none;
          width: 3px;
          height: 22px;
          position: absolute;
          left: 0;
          top: 5px;
          transition: background-color .3s;
        }
        .aplayer-list-index {
          color: #666;
          margin-right: 12px;
        }
        .aplayer-list-title {
          flex-grow: 1;
        }
        .aplayer-list-author {
          flex-shrink: 0;
          color: #666;
          float: right;
        }
      }
    }
  }
</style>