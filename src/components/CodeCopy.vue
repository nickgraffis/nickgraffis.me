<template>
  <textarea ref="source" class="hidden"> </textarea>
</template>

<script setup lang="ts">
import { useClipboard } from '@vueuse/core'
const flashMessage = inject<(message: string) => void>('updateNoticeMessage')
const source = ref()
onMounted(() => {
  document.querySelectorAll('.markdown-it-code-copy').forEach((item: Element) => {
    item.addEventListener('click', (event: Event) => {
      let code = item.getAttribute('data-clipboard-text') || ''
      // We remove the special charecter placeholders and replace them with their original value
      code = code?.replace(/<SINGLE-QUOTE>/g, '\'').replace(/<DOUBLE-QUOTE>/g, '"').replace(/<VUE-SETUP>/g, 'setup')
      source.value = code
      const { copy, copied, isSupported } = useClipboard({ source })
      if (isSupported) { copy() }
      else {
        flashMessage && flashMessage(`
          <div class="text-primary font-semibold text-xl flex flex-col items-center space-y-4">
            <p>
              Could not copy to clipboard!
            </p>
          </div>
        `)
      }
      if (copied) {
        flashMessage && flashMessage(`
          <div class="text-primary font-semibold text-xl flex flex-col items-center space-y-4">
            <p>
              Copied to clipboard!
            </p>
          </div>
        `)
      }
    })
  })
})
</script>
