<template>
  <div class="aplayer-volume-wrap">
    <icon-button
      :class="`aplayer-icon-${volumeIcon}`"
      :icon="volumeIcon"
      @click.native="$emit('togglemute')"
      title="音量"
    />
    <div
      class="aplayer-volume-bar-wrap"
      @mousedown="onBarMouseDown"
    >
      <div class="aplayer-volume-bar" ref="bar">
        <div
          class="aplayer-volume"
          :style="{
            width: muted ? 0 : `${Math.trunc(volume * 100)}%`,
            background: theme
          }"
        >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import IconButton from './aplayer-iconbutton.vue'
  import {getElementViewLeft} from '../utils'

  const barWidth = 90

  export default {
    components: {
      IconButton,
    },
    props: ['volume', 'muted', 'theme'],
    computed: {
      volumeIcon () {
        if (this.muted || this.volume <= 0) return 'volume-off'
        if (this.volume >= 1) return 'volume-up'
        return 'volume-down'
      },
    },
    methods: {
      adjustVolume (e) {
        let percentage = (barWidth - e.clientX + getElementViewLeft(this.$refs.bar)) / barWidth
        percentage = percentage > 0 ? percentage : 0
        percentage = percentage < 1 ? percentage : 1
        this.$emit('setvolume', percentage)
      },
      onBarMouseDown () {
        document.addEventListener('mousemove', this.onDocumentMouseMove)
        document.addEventListener('mouseup', this.onDocumentMouseUp)
      },
      onDocumentMouseMove (e) {
        let percentage = (barWidth - e.clientX + getElementViewLeft(this.$refs.bar)) / barWidth

        percentage = 1 - percentage
        percentage = percentage < 0 ? 0 : percentage
        percentage = percentage > 1 ? 1 : percentage

        this.$emit('setvolume', percentage)
      },
      onDocumentMouseUp (e) {
        document.removeEventListener('mouseup', this.onDocumentMouseUp)
        document.removeEventListener('mousemove', this.onDocumentMouseMove)

        let percentage = (barWidth - e.clientX + getElementViewLeft(this.$refs.bar)) / barWidth

        /**
         * 0    1
         *
         * 0.4  0.6
         *
         * 0.5  0.95
         *
         * 0.6  0.4
         *
         * 1    0
         */
        percentage = 1 - percentage
        percentage = percentage > 0 ? percentage : 0
        percentage = percentage < 1 ? percentage : 1

        this.$emit('setvolume', percentage)
      }
    }
  }
</script>

<style lang="scss">

  .aplayer-volume-wrap {
    position: relative;
    cursor: pointer;
    z-index: 0;

    &:hover .aplayer-volume-bar-wrap {
      display: block;
    }

    .aplayer-volume-bar-wrap {
      position: absolute;
      bottom: 8px;
      left: 35px;
      width: 90px;
      z-index: -1;
      &::after {
        content: '';
        position: absolute;
        bottom: -16px;
        left: 0;
        right: 0;
        width: 90px;
        /*background-color: #fff;*/
        /*box-shadow: 0 0 2px 0 rgba(0, 0, 0, 0.07), 0 0 5px 0 rgba(0, 0, 0, 0.1);*/
      }

      .aplayer-volume-bar {
        position: absolute;
        bottom: 0;
        left: 11px;
        width: 90px;
        height: 5px;
        background: #aaa;
        border-radius: 2.5px;
        overflow: hidden;
        z-index: 1;

        .aplayer-volume {
          position: absolute;
          height: 5px;
          bottom: 0;
          left: 0;
          right: 0;
          transition: width 0.1s ease, background-color .3s;
          will-change: width;
        }
      }
    }
  }
</style>