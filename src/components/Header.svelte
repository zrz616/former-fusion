<script>
  import { Blend, Badge, Sun, Moon, Github } from 'lucide-svelte';
  let isMenuOpen = false;
  let darkMode = false;
  import { visible } from '../stores/login';

  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }
</script>

<header class="bg-white shadow-md">
  <nav class="container mx-auto px-4 py-3 flex items-center justify-between">
    <div class="flex items-center">
      <a href="/" class="text-xl font-bold text-gray-800">
        <span class="bg-gradient-to-r from-amber-200 via-orange-500 to-amber-400 bg-clip-text text-transparent text-3xl uppercase">Svelte</span>
      </a>
    </div>
    
    <div class="hidden md:flex md:space-x-8 space-x-4">
      <a href="/" class="text-gray-600 hover:text-cyan-400 transition duration-300 font-sans">Home</a>
      <a href="/todo" class="text-gray-600 hover:text-cyan-400 transition duration-300 font-sans">Getting Started</a>
      <a href="/services" class="text-gray-600 hover:text-cyan-400 transition duration-300 font-sans">Build Guides</a>
      <a href="/contact" class="text-gray-600 hover:text-cyan-400 transition duration-300">Tier List</a>
    </div>

    <div class="flex items-center border-l border-slate-200 pl-6 space-x-4">
      {#if darkMode}
        <button 
          class="text-gray-600 hover:text-cyan-400 transition duration-300"
          on:click={() => darkMode = false}
          aria-label="Toggle light mode"
        >
          <Moon />
        </button>
      {:else}
        <button 
          class="text-gray-600 hover:text-cyan-400 transition duration-300"
          on:click={() => darkMode = true}
          aria-label="Toggle dark mode"
        >
          <Sun />
        </button>
      {/if}
      <button
        class="text-gray-600 hover:text-cyan-400 transition duration-300"
        on:click={() => visible.set(true)}
        aria-label="Login"
      >
        <Github />
      </button>
    </div>


    
    <button 
      class="md:hidden text-gray-600 hover:text-blue-600 transition duration-300"
      on:click={toggleMenu}
      aria-label="Toggle menu"
    >
      {#if isMenuOpen}
        <Badge class="h-6 w-6" />
      {:else}
        <Blend class="h-6 w-6" />
      {/if}
    </button>
  </nav>
  
  {#if isMenuOpen}
    <div class="md:hidden">
      <a href="/" class="block py-2 px-4 text-sm text-gray-600 hover:bg-gray-100">Home</a>
      <a href="/about" class="block py-2 px-4 text-sm text-gray-600 hover:bg-gray-100">About</a>
      <a href="/services" class="block py-2 px-4 text-sm text-gray-600 hover:bg-gray-100">Services</a>
      <a href="/contact" class="block py-2 px-4 text-sm text-gray-600 hover:bg-gray-100">Contact</a>
    </div>
  {/if}
</header>