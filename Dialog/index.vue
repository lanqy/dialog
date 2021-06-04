<template>
  <div v-if="show">
    <div role="dialog"
         class="dialog animated"
         :style="{ ZIndex: Zindex }">
      <div class="dialog__header"
           slot="header"
           v-if="title">{{title}}</div>
      <div class="dialog__content">
        <slot></slot>
      </div>
      <div class="dialog__footer dialog__footer--buttons"
           v-if="showConfirmButton">
        <div class="button dialog__cancel"
             @click="handleAction('cannel')"
             v-if="showCannelButton">
          <span>{{cancelText}}</span>
        </div>
        <div class="button dialog__confirm"
             @click="handleAction('confirm')">
          <span :style="{ color: confirmColor}">{{confirmText}}</span>
        </div>
      </div>
    </div>
    <div class="overlay"
         :style="{ ZIndex: Zindex - 1 }"
         v-if="closeOnClickOverlay"
         @click="handleAction('cannel')"></div>
    <div class="overlay"
         :style="{ ZIndex: Zindex - 1 }"
         v-else></div>
  </div>
</template>

<script>

export default {
  name: 'dig-dialog',
  props: {
    Zindex: {
      type: Number,
      default: 9999
    },
    value: {},
    title: String,
    width: {
      type: String,
      default: '85%'
    },
    beforeClose: Function,
    showCannelButton: {
      type: Boolean,
      default: true
    },
    showConfirmButton: {
      type: Boolean,
      default: true
    },
    closeOnClickOverlay: {
      type: Boolean,
      default: false
    },
    callback: Function,
    transition: {
      type: String
    },
    cancelText: {
      type: String,
      default: '取消'
    },
    confirmText: {
      type: String,
      default: '确认'
    },
    confirmColor: {
      type: String,
      default: '#1A1A1A'
    }
  },
  data () {
    return {
      show: false
    }
  },
  watch: {
    value (newVal, oldVal) {
      this.show = newVal
    },
    show (val) {
      this.$emit('input', val)
    }
  },
  mounted () {
    this.show = this.value
  },
  created () {
  },
  methods: {
    close () {
      this.show = false
    },
    handleAction (action) {
      this.$emit(action)
      if (this.beforeClose) {
        this.beforeClose(action, state => {
          if (state !== false) {
            this.onClose(action)
          }
        })
      } else {
        this.onClose(action)
      }
    },
    onClose (action) {
      this.close()

      if (this.callback) {
        this.callback(action)
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.dialog__wrapper {
  width: 100%;
}
.overlay {
  position: fixed;
  z-index: 99;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
}

.dialog {
  position: fixed;
  top: 50%;
  left: 50%;
  font-size: 32rpx;
  background-color: #fff;
  border-radius: 8rpx;
  -webkit-transform: translate3d(-50%, -50%, 0);
  transform: translate3d(-50%, -50%, 0);
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
  -webkit-transition: 0.3s;
  transition: 0.3s;
  z-index: 100;
  padding: 42rpx;
}

.dialog__header {
  text-align: center;
  font-size: 34rpx;
  font-weight: 500;
  color: #1a1a1a;
}

.dialog__footer {
  overflow: hidden;
  -webkit-user-select: none;
  user-select: none;
  margin-top: 42rpx;
}

.dialog__footer--buttons {
  display: -webkit-box;
  display: -webkit-flex;
  display: flex;
}

.dialog__footer--buttons .button {
  width: 258rpx;
  height: 86rpx;
  line-height: 86rpx;
  border: none;
  background: #ffd100;
  color: #1a1a1a;
  flex: 1;
  justify-content: center;
  text-align: center;
}
.dialog .button {
  border: 0;
  border-radius: 0;
}
.dialog .dialog__cancel {
  border: none;
  background: #f5f5f5;
  color: #737373;
  margin-right: 22rpx;
}
.dialog .button::after {
  border-radius: 0;
}
</style>>
