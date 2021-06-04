# dialog
A mini dialog base on Vue

```vue
<dig-dialog v-model="showTipsDialog"
            :show-cancel-button="false"
            :title="'标题'"
            :confirmText="'确认按钮'"
            :before-close="confirmTipsShow">
  <div class="tips">
   弹窗内容
  </div>
</dig-dialog>
```

```js
  confirmTipsShow (action, done) {
    if (action === 'confirm') {
      const success = true
      if (success) {
        done()
      } else {
        done(false)
      }
    } else {
      done()
    }
  }
```
