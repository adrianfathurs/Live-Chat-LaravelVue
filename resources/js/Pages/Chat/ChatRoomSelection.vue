<template>
  <div class="grid grid-cols-2">
    <div class="font-bold text-xl">

      {{currentRooms.name}} Chat Messages
    </div>
    <div>
      <select v-model="selected"
              @change="$emit('roomChanged',selected)"
              class="float-right">
              <option
                v-for="(room,index) in rooms" :key="index"
                :value="room"
              >
              {{room.name}}
              </option>
      </select>
    </div>
    
  </div>
</template>
<script>
export default {
  props:['rooms','currentRooms'],
  data() {
    return {
      selected:''
    }
  },
  
}
</script>