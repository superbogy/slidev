<script setup lang="ts">
import type { ComponentCustomProps, Slots } from 'vue'
import { h, watchEffect } from 'vue'
import _configs from '/@slidev/configs'
import { slideScale, windowSize } from '../state'
import { isPrintMode } from '../logic/nav'
import { themeVars } from '../env'
import PrintContainer from './PrintContainer.vue'

const width = _configs.canvasWidth
const height = Math.round(width / _configs.aspectRatio) + 1

function vStyle<Props>(props: Props, { slots }: { slots: Slots }) {
  if (slots.default)
    return h('style', slots.default())
}

watchEffect(() => {
  if (isPrintMode)
    document.body.parentNode.classList.add('print')
  else
    document.body.parentNode.classList.remove('print')
})
</script>

<template>
  <vStyle>
    @page { size: {{ width }}px {{ height }}px; margin: 0px; }
  </vStyle>
  <div id="page-root" class="grid grid-cols-[1fr,max-content]" :style="themeVars">
    <PrintContainer
      class="w-full h-full"
      :style="{ background: 'var(--slidev-slide-container-background, black)' }"
      :width="windowSize.width.value"
    />
  </div>
</template>

<style lang="postcss">
html.print,
html.print body,
html.print #app,
html.print #page-root {
  height: auto;
  overflow: auto;
}

html.print * {
  -webkit-print-color-adjust: exact;
}
html.print {
  width: 100%;
  height: 100%;
  overflow: visible;
}
html.print body {
  margin: 0 auto;
  border: 0;
  padding: 0;
  float: none;
  overflow: visible;
}
</style>
