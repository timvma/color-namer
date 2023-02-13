<script setup>

import TheHeader from './components/TheHeader.vue'

</script>
<template>

  <TheHeader />
  <Transition>
    <RouterView />
  </Transition>
</template>


