<script setup lang="ts">
import InfoWindow from '~/components/infoWindow.vue'

const buttonRef = ref<HTMLButtonElement | null>(null)
const button2Ref = ref<HTMLButtonElement | null>(null)
const infoWindowRef = ref<InstanceType<typeof InfoWindow> | null>(null)

const title = ref('')

function setup(dom: HTMLElement | null) {
  if (dom === null || infoWindowRef.value === null)
    return

  title.value = dom.textContent || ''

  infoWindowRef.value.setupPopper(dom)
  infoWindowRef.value.show()
  dom.addEventListener('blur', () => {
    infoWindowRef.value!.hide()
  })
}
</script>

<template>
  <button ref="buttonRef" type="button" @click="setup(buttonRef)">
    button 1
  </button>
  <button ref="button2Ref" type="button" @click="setup(button2Ref)">
    button 2
  </button>
  <InfoWindow ref="infoWindowRef">
    {{ title }}
  </InfoWindow>
</template>
