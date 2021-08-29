<template>
  <div class="h-96 w-full">
    <div class="h-full p-2 flex flex-col-reverse overflow-scroll">
      <div v-for="(pesan,index) in messagess" :key="index">
        <message-item  class="ml-4" :itemMessage="pesan"></message-item>
      </div>
    </div>
  </div>
</template>
<script>
import messageItem from './MessageItem.vue'

export default {
  components: { messageItem },
  props:["messagess"]
}
</script>