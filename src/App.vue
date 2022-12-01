<script setup lang="ts">
import { ref, watch } from 'vue';

const garbageIn = ref<string>('')
const garbageOut = ref<string>('')
const garbageOutArea = ref<HTMLTextAreaElement>()
const keyRegex = /([A-z0-9]+-[A-z0-9]+-[A-z0-9]+)/g

watch(garbageIn, (val) => {
  let matches = garbageIn.value.match(keyRegex)
  if(matches){
    garbageOut.value = matches.join('\n')
  }
})

function redeem() {
  if(garbageOutArea.value){
    const copyText = garbageOutArea.value
    copyText.select();
    copyText.setSelectionRange(0, 99999);
    navigator.clipboard.writeText(garbageOut.value);
    window.open('https://store.steampowered.com/account/registerkey', '_blank')
  }
}

</script>

<template>
  <div class="container f-col h-100 mx-1">
    <div class="container f-row f-1">
      <div class="m-1 w-100 container f-col">
        <h1>Garbage In</h1>
        <textarea v-model="garbageIn"></textarea>
      </div>
      <div class="m-1 w-100 container f-col">
        <h1>Keys Out</h1>
        <textarea ref="garbageOutArea" v-model="garbageOut" disabled></textarea>
      </div>
    </div>
    <button @click="redeem()">Redeem on Steam</button>
  </div>
</template>

<style scoped>
textarea {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  resize: none;
}

button {
  margin-bottom: 1rem;
}
</style>
