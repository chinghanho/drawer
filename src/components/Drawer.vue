<script>
import sample from 'lodash/sample'
import without from 'lodash/without'

export default {
  props: {
    items: {
      type: Array,
      default: () => []
    }
  },

  data() {
    return {
      isDebugMode: false,
      isStartedToDraw: false,
      selectedItems: [],
      activeItem: ''
    }
  },

  computed: {
    activeText() {
      return this.activeItem || '請點擊按鈕'
    },

    remainsItems() {
      return without(this.items, ...this.selectedItems)
    },

    debugItems() {
      return this.remainsItems.length === 0 ? '沒有剩餘的人名' : this.remainsItems.join('\n')
    },

    anyRamainItems() {
      return this.remainsItems.length > 0
    }
  },

  methods: {
    draw() {
      if (this.items.length === 0) {
        return
      }

      if (!this.isStartedToDraw) {
        this.$emit('drawstart')
        this.isStartedToDraw = true
      }

      this.activeItem = sample(this.remainsItems)
      this.selectedItems.push(this.activeItem)
    }
  }
}
</script>

<template>
  <div class="card" style="width: 360px;">
    <div class="card-body p-5">
      <div class="h1 text-center mb-4">
        {{ activeText }}
      </div>

      <div class="text-center mb-2">
        <div class="text-muted">
          還有 {{ remainsItems.length }} 人
        </div>
      </div>

      <div>
        <button class="btn btn-primary btn-lg d-block w-100" @click="draw" :disabled="!anyRamainItems">
          抽出答題者
        </button>
      </div>

      <div v-if="isDebugMode" class="pt-3 text-left">
        <div class="mb-1">剩餘名單：</div>
        <pre class="p-2 bg-light border rounded"><code>{{ debugItems }}</code></pre>
      </div>
    </div>
  </div>

  <div class="position-fixed start-0 bottom-0 p-3">
    <div class="form-check">
      <input class="form-check-input" type="checkbox" id="debug-mode" v-model="isDebugMode">
      <label class="form-check-label" for="debug-mode">
        debug 模式
      </label>
    </div>
  </div>
</template>
