<script lang="ts">
   export let UserMessage = ""
   export let LogMessages:{type:string;message:string}[] = []
   import { createEventDispatcher } from 'svelte';

   const dispatch = createEventDispatcher();
   const copyText = (data:string) => {
      navigator.clipboard.writeText(data);
   }
</script>
<div id="MainScreenshare">
   <div class="chatwindow" id="chatwindow">
      {#each LogMessages as item}
         <div class={item.type=="Sender"?"chat chat-end":"chat chat-start"}>
            <div class="chat-header">
               <button class="" on:click={()=> {copyText(item.message)}}>📋</button>
            </div>
            <pre class="chat-bubble chat-bubble-success" style="overflow-x: auto;">{item.message}</pre>
         </div>
      {/each}
   </div>
   <div class="join chattxtdiv">
      <textarea placeholder="Message" class="bg-base-100 textarea-neutral" rows="1" bind:value={UserMessage}></textarea>
      <button class="btn btn-neutral join-item" on:click={()=>dispatch('SendMessage')}>Send</button>
   </div>
</div>
<style>
   #MainScreenshare{
      height: 100%;
      width: 100%;
      /* display: flex; */
      /* padding: 10px; */
      /* background-color: yellowgreen; */
      display: grid;
      grid-template-rows: auto max-content;
   }
   .chattxtdiv{
      width: 100%;
      display: grid;
      grid-template-columns: auto max-content;
      padding: 10px;
   }
   .chatwindow{
      max-height: 100%;
      overflow: auto;
   }
   textarea{
      border: 1px solid;
      border-radius: 10px 0px 0px 10px;
   }
</style>