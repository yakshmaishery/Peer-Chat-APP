<script lang="ts">
   let Themes = ["light","dark","cupcake","bumblebee","emerald","corporate","synthwave","retro","cyberpunk","valentine","halloween","garden","forest","aqua","lofi","dracula","cmyk","autumn","business","acid","lemonade","night",'coffee',"winter",'dim',"nord","sunset","caramellatte","abyss","silk"]
</script>
<div class="dropdown">
   <div tabindex="0" role="button" class="btn" style="width: 100%;">
     Theme
     <svg
       width="12px"
       height="12px"
       class="inline-block h-2 w-2 fill-current opacity-60"
       xmlns="http://www.w3.org/2000/svg"
       viewBox="0 0 2048 2048">
       <path d="M1799 349l242 241-1017 1017L7 590l242-241 775 775 775-775z"></path>
     </svg>
   </div>
   <!-- svelte-ignore a11y_no_noninteractive_tabindex -->
   <ul tabindex="0" class="dropdown-content bg-base-300 rounded-box z-1 w-52 p-2 shadow-2xl" style="max-height: 30vh;overflow:auto">
      {#each Themes as item}
      <li>
        <input
          type="radio"
          name="theme-dropdown"
          class="theme-controller w-full btn btn-sm btn-block btn-ghost justify-start"
          aria-label={item}
          value={item} />
      </li>
      {/each}
   </ul>
 </div>