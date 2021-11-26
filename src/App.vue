<script>
import Drawer from './components/Drawer.vue'
import compact from 'lodash/compact'
import uniq from 'lodash/uniq'

export default {
  components: {
    Drawer
  },

  data() {
    return {
      text: '',
      isLocked: false
    }
  },

  computed: {
    rawItems() {
      return this.text.split(/\r?\n/)
    },

    items() {
      return uniq(compact(this.rawItems))
    },

    itemsLengthNotMatched() {
      return this.text.length > 0 && this.rawItems.length !== this.items.length
    }
  },

  methods: {
    onDrawStart() {
      this.isLocked = true
    },
  }
}
</script>

<template>
  <div class="container">
    <Drawer :items="items" @drawstart="onDrawStart" />
  </div>

  <div class="position-fixed end-0 bottom-0 p-3">
    <div class="btn-group dropup">
      <button type="button" class="btn btn-secondary dropdown-toggle" data-bs-toggle="dropdown" aria-expanded="false">
        匯入名單
      </button>
      <div class="dropdown-menu" style="min-width: 380px;">
        <div class="px-2">
          <textarea v-model="text" cols="30" rows="10" class="form-control" :disabled="isLocked">
          </textarea>
        </div>
        <div class="text-danger px-2 pt-1" v-if="itemsLengthNotMatched">
          請確認每一行的名單是否都是唯一的，並且不會有重複的名單。系統將自動過濾重複名單，以及去除空白的斷行資料。
        </div>
        <div class="text-muted px-2 pt-1" v-if="isLocked">
          抽籤已開始，名單已鎖定，如果需要重新開始，請重新整理瀏覽器。
        </div>
        <div class="text-muted px-2 pt-1" v-else>
          每筆資料請以換行分隔。
        </div>
      </div>
    </div>
  </div>
</template>

<style>
body, html {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: #ebebeb;
}
</style>
