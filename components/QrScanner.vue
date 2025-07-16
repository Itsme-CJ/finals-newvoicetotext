<template>
  <div>
    <div id="reader" style="width: 500px;"></div>
    <p v-if="scanResult">Scanned Result: {{ scanResult }}</p>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { Html5QrcodeScanner } from 'html5-qrcode'

const scanResult = ref('')

onMounted(() => {
  const scanner = new Html5QrcodeScanner('reader', {
    fps: 10,
    qrbox: { width: 250, height: 250 }
  })

scanner.render(
  (decodedText) => {
    scanResult.value = decodedText;

    // Open in new tab to allow coming back easily
    if (decodedText.startsWith('http://') || decodedText.startsWith('https://')) {
      window.open(decodedText, '_blank');
    } else if (decodedText.startsWith('/')) {
      // Optional: handle internal route (like /auth/signinpage)
      router.push(decodedText);
    }

    scanner.clear();
  },
  (error) => {
    console.error('Scan error:', error);
  }
);
})
</script>

