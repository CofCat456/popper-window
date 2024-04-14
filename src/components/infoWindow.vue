<script setup lang="ts">
import type { Instance } from '@popperjs/core'
import { createPopper } from '@popperjs/core'

const infoWindowRef = ref<HTMLDivElement | null>(null)
const popperInstance = shallowRef<Instance | null>(null)

function setupPopper(dom: HTMLElement) {
  if (dom === null || infoWindowRef.value === null)
    return

  popperInstance.value = createPopper(dom, infoWindowRef.value, {
    modifiers: [
      {
        name: 'offset',
        options: {
          offset: [0, 8],
        },
      },
    ],
  })
}

function show() {
  if (infoWindowRef.value === null || popperInstance.value === null)
    return

  infoWindowRef.value.setAttribute('data-show', '')

  // We need to tell Popper to update the tooltip position
  // after we show the window, otherwise it will be incorrect
  popperInstance.value.update()
}

function hide() {
  if (infoWindowRef.value === null)
    return

  infoWindowRef.value.removeAttribute('data-show')
}

defineExpose({ setupPopper, show, hide })
</script>

<template>
  <div id="infoWindow" ref="infoWindowRef">
    <div grid="~ place-items-center" h-8 w-28 rounded-8 bg-sky-300:40>
      <slot>
        InfoWindow
      </slot>
    </div>
  </div>
</template>

<style scoped>
#infoWindow {
  display: none;
}

#infoWindow[data-show] {
  display: block;
}
</style>
