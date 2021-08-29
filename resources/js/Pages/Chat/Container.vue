<template>
    <app-layout title="chat">
        <template #header>
         
              <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                  <chat-room-selection v-if="currentRoom.id"
                                      :rooms="chatRooms"
                                      :currentRooms="currentRoom"
                                      v-on:roomChanged="setRooms($event)"
                  ></chat-room-selection>
                  
              </h2>
              
          
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <message-container :messagess="messages"/>
                    <input-message :room="currentRoom" v-on:messagesent="getMessages()"/>
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import AppLayout from '@/Layouts/AppLayout.vue'
    import InputMessage from './InputMessage.vue'
    import MessageContainer from './MessageContainer.vue'
import ChatRoomSelection from './ChatRoomSelection.vue'



    export default {
        components: {
            AppLayout,
            InputMessage,
            MessageContainer,
                ChatRoomSelection,
              
        },
        data() {
          return {
            messages:[],
            chatRooms:[],
            currentRoom:[],
            userIdOld:0,
          }
        },
        watch:{
          currentRoom(val, oldVal) {
            if(oldVal.id ){
              this.disconnect(oldVal);
            }
            this.connect();
          }
        },
        methods: {
          connect(){
            if(this.currentRoom.id){
                let vm=this;
                this.getMessages();
                window.Echo.private("chat."+this.currentRoom.id).
                listen('.message.new',e=>{
                  vm.getMessages();
                });
            }
          },
          disconnect(room){
            window.Echo.leave("chat."+room.id);
          },
          async getRooms(){
            try {
              var res=await axios.get('/chat/rooms');
              this.chatRooms=res.data;
              this.setRooms(res.data[0]);
              
            } catch (error) {
              console.log(error);
            }
          
          },
          async setRooms(room){
            this.currentRoom=room;
            
          },
          async getMessages(){
            try {
              var res =await axios.get("/chat/rooms/"+this.currentRoom.id+"/messages");
              this.messages=res.data;
              
            } catch (error) {
              console.log(error)
            }
          }
        },
        async created() {
          await this.getRooms();
        },

    }
</script>
