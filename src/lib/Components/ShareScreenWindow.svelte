<script lang="ts">
   export let videodata:HTMLVideoElement
   import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();
</script>
<div id="MainScreenshare">
   <div style="display: flex;flex-direction:column">
      <button class="btn" on:click={()=>dispatch('ShareScreen')}>Share</button>
      <button class="btn" on:click={()=>videodata.requestFullscreen()}>Fullscreen</button>
   </div>
      <!-- svelte-ignore a11y_media_has_caption -->
      <!-- svelte-ignore element_invalid_self_closing_tag -->
      <video bind:this={videodata} controls id="videotag"/>
</div>

<style>
   #MainScreenshare{
      height: 100%;
      width: 100%;
      display: flex;
      /* padding: 10px; */
      /* background-color: yellowgreen; */
   }
   #videotag{
      background-color: blue;
      display: flex;
      justify-self: center;
      max-height: max-content;
      max-width: fit-content;
      /* width: 100vw; */
      /* height: 90vh; */
   }
</style>