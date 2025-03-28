<script lang="ts">
  import logo from './assets/bulogo.png';
  import { slide } from 'svelte/transition';

  let currentDate = new Date();
  let day = currentDate.getDate();
  let modulesExpanded = false;

  const calculationModules = [
    'Structural Analysis',
    'Hydraulic Calculations', 
    'Geotechnical Modeling',
    'Transportation Planning',
    'Environmental Engineering',
    'Construction Management',
    'Materials Engineering',
    'Surveying and Mapping'
  ];

  function toggleModulesExpand() {
    modulesExpanded = !modulesExpanded;
  }
</script>

<svelte:head>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700;800&family=Inter:wght@400;500;600&display=swap">
</svelte:head>

<div class="min-h-screen bg-[#f8f9fa] p-8 flex items-center justify-center">
  <div class="w-full max-w-6xl mx-auto grid grid-cols-3 gap-6">
    <!-- Left Panel -->
    <div class="bg-[#002b5c] text-white p-12 rounded-lg col-span-1 flex flex-col">
      <div class="mb-8">
        <div class="w-20 h-20 bg-white rounded-full flex items-center justify-center">
          <img src={logo} alt="Logo" class="w-16 h-16 rounded-full object-cover border-2 border-[#f08c00]" />
        </div>
      </div>

      <div class="mb-8">
        <h1 class="text-4xl font-extrabold font-montserrat leading-tight">CIVIL ENGINEERING SOLUTIONS</h1>
      </div>

      <p class="text-lg font-inter leading-relaxed">
        This design tool aid aims to provide assistance to both students and instructors for academic interactions during either synchronous or asynchronous classes.
      </p>
    </div>

    <div class="col-span-2 grid grid-cols-2 gap-6">
      <!-- Modules Card -->
      <div class="relative bg-[#8c9eff] text-white rounded-lg p-10 flex flex-col cursor-pointer shadow-md" on:click={toggleModulesExpand}>
        <h2 class="text-3xl font-bold font-montserrat">Modules</h2>
        <p class="text-md font-inter opacity-90">Access calculation modules and tools</p>

        <div class="mt-auto bg-white rounded-md p-4 flex justify-between items-center h-16 w-full">
          <span class="text-md text-gray-800 font-inter font-medium">8 calculation modules available</span>
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-800 transition-transform duration-300" class:rotate-180={modulesExpanded} viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
          </svg>
        </div>

        {#if modulesExpanded}
          <div transition:slide class="absolute w-full left-0 mt-2 space-y-2 bg-white/10 rounded-lg p-4">
            {#each calculationModules as module}
              <div class="text-md py-2 border-b border-white/10 last:border-b-0">{module}</div>
            {/each}
          </div>
        {/if}
      </div>

      <!-- Subjects Card -->
      <div class="bg-[#264653] text-white rounded-lg p-10 flex flex-col shadow-md">
        <h2 class="text-3xl font-bold font-montserrat mb-4">Subjects</h2>
        <p class="text-md font-inter opacity-90">Browse engineering knowledge base</p>

        <div class="mt-auto bg-white text-black rounded-md p-4 flex justify-between items-center h-16 w-full">
          <span class="text-md text-gray-800 font-inter font-medium">8 subject areas with detailed resources</span>
        </div>
      </div>

      <!-- Calendar Card -->
      <div class="bg-[#e76f51] text-white rounded-lg p-10 flex flex-col items-center justify-center shadow-md">
        <h2 class="text-3xl font-bold font-montserrat mb-4">Calendar</h2>
        <div class="w-28 h-28 bg-white rounded-md flex flex-col items-center justify-center relative">
          <span class="text-5xl font-extrabold text-gray-800 font-montserrat">{day}</span>
        </div>
      </div>

      <!-- Settings Card -->
      <div class="bg-[#b0bec5] text-white rounded-lg p-10 flex flex-col items-center justify-center shadow-md">
        <h2 class="text-3xl font-bold font-montserrat mb-4">Settings</h2>
        <div class="w-24 h-24">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="text-gray-800">
            <path fill-rule="evenodd" d="M12 0a12 12 0 110 24 12 12 0 010-24zm1 6a1 1 0 00-2 0v6.586L7.707 9.293a1 1 0 10-1.414 1.414l4 4a1 1 0 001.414 0l4-4a1 1 0 10-1.414-1.414L13 12.586V6z" clip-rule="evenodd" />
          </svg>
        </div>
      </div>
    </div>
  </div>
</div>
