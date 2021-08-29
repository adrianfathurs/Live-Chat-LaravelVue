<template>
 <div>
     <p><span class=" font-bold text-base text-gray-800 leading-tight">{{itemMessage.user.name}} :</span>  {{itemMessage.message}}</p>
 </div>
  
</template>
<script>
export default {
  props:["itemMessage"],
  data() {
    return {
     
      
    }
  },
  
}
</script>