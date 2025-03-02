<script lang="ts">
   export let Window = ""
   export let CurrentUser = ""
   export let AnotherUser = ""
   export let IsConnected = false

   //  Imports
   import ThemeControllerDropdown from "./ThemeControllerDropdown.svelte";
   import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

   function ChangeWindow(params:string) {
      Window = params
   }
   function ConnectwithUserFirst() {
        dispatch('ConnectwithUserFirst');
   }
</script>
<div class="navbar bg-base-100 shadow-sm">
   <div class="navbar-start">
      <ThemeControllerDropdown/>
     <div class="dropdown">
       <div tabindex="0" role="button" class="btn btn-ghost btn-circle" hidden={!IsConnected}>
         <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block h-5 w-5 stroke-current"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path> </svg>
       </div>
       <ul
         tabindex="0"
         class="menu menu-sm dropdown-content bg-base-100 rounded-box z-1 mt-3 w-52 p-2 shadow">
         <!-- <li><a>Homepage</a></li>
         <li><a>Portfolio</a></li>
         <li><a>About</a></li> -->
         <li>
            <button class="btn" on:click={()=>ChangeWindow("screen")}>Share Screen</button>
         </li>
         <li>
            <button class="btn" on:click={()=>ChangeWindow("chat")}>Chat</button>
         </li>
         <li>
            <button class="btn" on:click={()=>dispatch('LeaveConnection')}>Leave</button>
         </li>
       </ul>
     </div>
   </div>
   <div class="navbar-center">
     <a class="btn btn-ghost text-xl" on:click={()=>navigator.clipboard.writeText(CurrentUser)}>Personal Chat ({CurrentUser})</a>
   </div>
   <div class="navbar-end">
      <input type="text" placeholder="Search by another ID" class="input input-bordered w-24 md:w-auto" bind:value={AnotherUser} disabled={IsConnected} />
     <button class="btn btn-ghost btn-circle" on:click={ConnectwithUserFirst} disabled={IsConnected?true:(AnotherUser == "")?true:false}>
       <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /> </svg>
     </button>
   </div>
 </div>