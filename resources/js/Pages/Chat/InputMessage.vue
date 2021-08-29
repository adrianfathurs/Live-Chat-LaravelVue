<template>
  <div class="relative h-10 m-3">
    <div style="border-top:1px solid #e6e6e6;" class="grid grid-cols-6 outline-none">
      <input type="text"
            v-model="message"
            @keyup.enter="sendMessage()"
            placeholder="say something ..."
            class="col-span-5  focus:ring focus:border-blue-200 p-1 m-2"
      >
        <button
          class=" bg-blue-500 hover:bg-green-700 p-1 mt-1 rounded text-white"
          @click="sendMessage()"
        >
          send
        </button>
    </div>
  </div>
</template>
<script>
import Input from '../../Jetstream/Input.vue'
export default {
  component:{
    Input
  },
  props:['room'],
  data() {
    return {
      message:''
    }
  },
  methods: {
    sendMessage(){
      if(this.message == ' '){
        return;
      }
      axios.post('/chat/rooms/'+this.room.id+'/message',{
        message:this.message
      }).then(response=>{
        if(response.status == 201){
          this.message='';
          this.$emit("messagesent");
        }
      }).catch(error=>{
        console.log(error);
      })
    }
  },
}
</script>