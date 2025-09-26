<script>
  import { onMount } from 'svelte';

  // External prop
  export let menu_open;

  // Scroll + navbar state
  let y = 0;
  let pageHeight = 10;
  $: scrolled = y > pageHeight;

  // Dropdown state
  let dropdownOpen = false;   // desktop Cohort 1
  let dropdownOpensm = false; // mobile Cohort 1

  // Optional: close dropdowns with Escape
  const handleKeydown = (e) => {
    if (e.key === 'Escape') {
      dropdownOpen = false;
      dropdownOpensm = false;
    }
  };

  // Smooth scroll helpers (kept from your snippet)
  function findPos(obj) {
    let curtop = 0;
    if (obj?.offsetParent) {
      do {
        curtop += obj.offsetTop;
      } while (obj = obj.offsetParent);
      return curtop;
    }
    return 0;
  }

  const handleCyclo = (e) => {
    e.preventDefault();
    const button = e.target.closest('[data-target]');
    if (!button) return;
    const target = button.getAttribute('data-target');
    const offset = Number(button.getAttribute('data-offset') || 0);

    window.scroll({
      left: 0,
      top: findPos(document.getElementById(target)) - offset,
      behavior: 'smooth'
    });
  };

  // Desktop dropdown handlers
  const openDD = () => (dropdownOpen = true);
  const closeDD = () => (dropdownOpen = false);

  // Mobile dropdown toggle
  const toggleDDsm = () => (dropdownOpensm = !dropdownOpensm);

  // Menu items for Cohort 1 (edit as needed)
  const cohortMenu = [
    { label: 'Cohort 1', href: '/cohort1' },
  ];
</script>

<!-- Bind scrollY + Escape close -->
<svelte:window bind:scrollY={y} on:keydown={handleKeydown} />

<!-- Top Navbar (desktop + hamburger) -->
<div
  class="fixed space-x-4 md:space-x-0 min-h-fit w-screen sm:min-h-0 left-0
         flex sm:items-center ease-in-out transition-all transform duration-300
         z-50 py-2 {scrolled ? 'bg-white top-0 shadow-md' : 'bg-white text-black'}"
>
  <div class="w-full container mx-auto flex justify-between px-4">
    <!-- Logos -->
    <div class="md:px-2 m-0 font-medium gap-2 w-fit p-0 rounded-md flex items-center justify-center">
      <a href="/">
        <img
          src="{scrolled ? '/img/logo.svg' : '/img/logo.svg'}"
          alt="logo"
          class="items-center justify-center h-12"
        />
      </a>
    </div>

    <!-- Desktop links + hamburger -->
    <main class="flex items-start px-7 my-auto">
      <!-- Desktop nav -->
      <div style="font-family: Museo-Sans, sans-serif;" class="flex my-auto">
        <a href="/" class="md:flex cursor-pointer hidden px-4 py-3 md:py-1 uppercase text-sm hover:text-green-900">Home</a>
        <a href="/#about" class="md:flex page-scroll cursor-pointer hidden px-4 py-3 md:py-1 uppercase text-sm hover:text-green-900">About</a>
        <!-- <a href="/#Global_Inspirations" class="md:flex page-scroll hidden cursor-pointer px-4 py-3 md:py-1 uppercase text-sm hover:text-green-900">Inspirations</a> -->
        <a href="/#Contributions" class="md:flex page-scroll cursor-pointer hidden px-4 py-3 md:py-1 uppercase text-sm hover:text-green-900">Contributions</a>
        <a href="/#Program_Goals" class="md:flex page-scroll cursor-pointer hidden px-4 py-3 md:py-1 uppercase text-sm hover:text-green-900">Goals</a>
      </div>

      <!-- Mobile hamburger -->
      <div class="flex items-center justify-center bg-white my-auto md:hidden flex">
        <div
          on:click={() => menu_open = !menu_open}
          class="z-50 items-center justify-center my-auto absolute cursor-pointer top-3 right-4 w-10 h-10 text-xs transition duration-150 py-1 rounded bg-transparent block outline-none focus:outline-none"
          type="button"
          aria-label="Toggle menu"
          aria-expanded={menu_open}
        >
          <div class="block absolute w-6 h-0.5 rounded-sm bg-black top-5 duration-500 { menu_open ? 'opacity-0' : '' }"></div>
          <div class="block absolute w-6 h-0.5 rounded-sm bg-black top-6 duration-500 { menu_open ? 'transform rotate-45' : '' }"></div>
          <div class="block absolute w-6 h-0.5 rounded-sm bg-black top-7 duration-500 { menu_open ? 'transform -rotate-45' : '' }"></div>
          <div class="block absolute w-6 h-0.5 rounded-sm bg-black top-8 duration-500 { menu_open ? 'opacity-0' : '' }"></div>
        </div>
      </div>
    </main>
  </div>
</div>

<!-- Slide-in Mobile Menu -->
<div
  class="min-h-screen h-20 p-8 sm:py-3 bg-white left-0 top-0
         flex items-center ease-in-out transition-all transform duration-300
         -translate-x-full fixed z-50 {menu_open ? 'translate-x-0' : ''}"
  aria-hidden={!menu_open}
>
  <div class="flex bg-white top-4 absolute">
    <button
      on:click={() => menu_open = !menu_open}
      class="z-50 bg-white cursor-pointer fixed right-5 w-10 h-10 text-xl transition duration-150 leading-none px-2 py-1 rounded bg-transparent block outline-none focus:outline-none"
      type="button"
      aria-label="Close menu"
    >
      <span class="block absolute w-6 h-1 rounded-sm bg-black top-3 duration-500 { menu_open ? 'opacity-0' : '' }"></span>
      <span class="block absolute w-6 h-1 rounded-sm bg-black duration-500 { menu_open ? 'transform rotate-45' : '' }"></span>
      <span class="block absolute w-6 h-1 rounded-sm bg-black duration-500 { menu_open ? 'transform -rotate-45' : '' }"></span>
      <span class="block absolute w-6 h-1 rounded-sm bg-black bottom-2.5 duration-500 { menu_open ? 'opacity-0' : '' }"></span>
    </button>
  </div>

  <div class="w-fit container mx-auto sm:items-center sm:justify-between">
    <div class="mb-8 gap-2 font-medium sm:bg-transparent p-5 sm:p-0 rounded-md flex items-center justify-center">
      <img src="/img/logo.svg" alt="logo" class="md:h-14 h-12" />
    </div>

    <nav>
      <ul class="md:text-sm gap-2 font-data">
        <li on:click={() => menu_open = !menu_open} class="px-5 py-2 transform hover:bg-gray-400 transition duration-500 md:hidden flex">
          <a href="/" class="page-scroll cursor-pointer font-bold">Home</a>
        </li>
        <li on:click={() => menu_open = !menu_open} class="px-5 py-2 transform hover:bg-gray-400 transition duration-500 md:hidden flex">
          <a href="/#about" class="page-scroll cursor-pointer font-bold">About</a>
        </li>
        <!-- <li on:click={() => menu_open = !menu_open} class="px-5 py-2 transform hover:bg-gray-400 transition duration-500 md:hidden flex">
          <a href="/#Global_Inspirations" class="page-scroll cursor-pointer font-bold">Inspirations</a>
        </li> -->
        <li on:click={() => menu_open = !menu_open} class="px-5 py-2 transform hover:bg-gray-400 transition duration-500 md:hidden flex">
          <a href="/#Contributions" class="page-scroll cursor-pointer font-bold">Contributions</a>
        </li>


        <li on:click={() => menu_open = !menu_open} class="px-5 py-2 transform hover:bg-gray-400 transition duration-500 md:hidden flex">
          <a href="/#Program_Goals" class="page-scroll cursor-pointer font-bold">Goals</a>
        </li>

      </ul>
    </nav>
  </div>
</div>
