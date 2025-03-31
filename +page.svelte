<script lang="ts">
  import logo from './assets/bulogo.png';
  import { slide, fade } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';

  let currentDate = new Date();
  let day = currentDate.getDate();
  let modulesExpanded = false;
  let subjectsExpanded = false;
  let selectedModule: string | null = null;
  let selectedSubject: string | null = null;

  const calculationModules = [
    { id: 'structural', name: 'Structural Analysis', icon: 'M3.75 12h16.5m-16.5 3.75h16.5M3.75 19.5h16.5M5.625 4.5h12.75a1.875 1.875 0 010 3.75H5.625a1.875 1.875 0 010-3.75z' },
    { id: 'hydraulic', name: 'Hydraulic Calculations', icon: 'M12 6v12m-3-2.818l.879.659c1.171.879 3.07.879 4.242 0 1.172-.879 1.172-2.303 0-3.182C13.536 12.219 12.768 12 12 12c-.725 0-1.45-.22-2.003-.659-1.106-.879-1.106-2.303 0-3.182s2.9-.879 4.006 0l.415.33M21 12a9 9 0 11-18 0 9 9 0 0118 0z' }, 
    { id: 'geotechnical', name: 'Geotechnical Modeling', icon: 'M2.25 15.75l5.159-5.159a2.25 2.25 0 013.182 0l5.159 5.159m-1.5-1.5l1.409-1.409a2.25 2.25 0 013.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 001.5-1.5V6a1.5 1.5 0 00-1.5-1.5H3.75A1.5 1.5 0 002.25 6v12a1.5 1.5 0 001.5 1.5zm10.5-11.25h.008v.008h-.008V8.25zm.375 0a.375.375 0 11-.75 0 .375.375 0 01.75 0z' },
    { id: 'transportation', name: 'Transportation Planning', icon: 'M8.25 18.75a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h6m-9 0H3.375a1.125 1.125 0 01-1.125-1.125V14.25m17.25 4.5a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h1.125c.621 0 1.129-.504 1.09-1.124a17.902 17.902 0 00-3.213-9.193 2.056 2.056 0 00-1.58-.86H14.25M16.5 18.75h-2.25m0-11.177v-.958c0-.568-.422-1.048-.987-1.106a48.554 48.554 0 00-10.026 0 1.106 1.106 0 00-.987 1.106v7.635m12-6.677v6.677m0 4.5v-4.5m0 0h-12' },
    { id: 'environmental', name: 'Environmental Engineering', icon: 'M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z' },
    { id: 'construction', name: 'Construction Management', icon: 'M11.42 15.17L17.25 21A2.652 2.652 0 0021 17.25l-5.877-5.877M11.42 15.17l2.496-3.03c.317-.384.74-.626 1.208-.766M11.42 15.17l-4.655 5.653a2.548 2.548 0 11-3.586-3.586l6.837-5.63m5.108-.233c.55-.164 1.163-.188 1.743-.14a4.5 4.5 0 004.486-6.336l-3.276 3.277a3.004 3.004 0 01-2.25-2.25l3.276-3.276a4.5 4.5 0 00-6.336 4.486c.091 1.076-.071 2.264-.904 2.95l-.102.085m-1.745 1.437L5.909 7.5H4.5L2.25 3.75l1.5-1.5L7.5 4.5v1.409l4.26 4.26m-1.745 1.437l1.745-1.437m6.615 8.206L15.75 15.75M4.867 19.125h.008v.008h-.008v-.008z' },
    { id: 'materials', name: 'Materials Engineering', icon: 'M20.25 7.5l-.625 10.632a2.25 2.25 0 01-2.247 2.118H6.622a2.25 2.25 0 01-2.247-2.118L3.75 7.5M10 11.25h4M3.375 7.5h17.25c.621 0 1.125-.504 1.125-1.125v-1.5c0-.621-.504-1.125-1.125-1.125H3.375c-.621 0-1.125.504-1.125 1.125v1.5c0 .621.504 1.125 1.125 1.125z' },
    { id: 'surveying', name: 'Surveying and Mapping', icon: 'M9 6.75V15m6-6v8.25m.503 3.498l4.875-2.437c.381-.19.622-.58.622-1.006V4.82c0-.836-.88-1.38-1.628-1.006l-3.869 1.934c-.317.159-.69.159-1.006 0L9.503 3.252a1.125 1.125 0 00-1.006 0L3.622 5.689C3.24 5.88 3 6.27 3 6.695V19.18c0 .836.88 1.38 1.628 1.006l3.869-1.934c.317-.159.69-.159 1.006 0l4.994 2.497c.317.158.69.158 1.006 0z' }
  ];

  const subjects = [
    { id: 'statics', name: 'Statics', icon: 'M12 21v-8.25M15.75 21v-8.25M8.25 21v-8.25M3 9l9-6 9 6m-1.5 12V10.332A48.36 48.36 0 0012 9.75c-2.551 0-5.056.2-7.5.582V21M3 21h18M12 6.75h.008v.008H12V6.75z' },
    { id: 'geotech', name: 'Geotech', icon: 'M15 11.25l-3-3m0 0l-3 3m3-3v7.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z' }, 
    { id: 'transportation', name: 'Transportation', icon: 'M8.25 18.75a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h6m-9 0H3.375a1.125 1.125 0 01-1.125-1.125V14.25m17.25 4.5a1.5 1.5 0 01-3 0m3 0a1.5 1.5 0 00-3 0m3 0h1.125c.621 0 1.129-.504 1.09-1.124a17.902 17.902 0 00-3.213-9.193 2.056 2.056 0 00-1.58-.86H14.25M16.5 18.75h-2.25m0-11.177v-.958c0-.568-.422-1.048-.987-1.106a48.554 48.554 0 00-10.026 0 1.106 1.106 0 00-.987 1.106v7.635m12-6.677v6.677m0 4.5v-4.5m0 0h-12' }
  ];

  function toggleModulesExpand() {
    modulesExpanded = !modulesExpanded;
    if (!modulesExpanded) {
      selectedModule = null;
    }
  }

  function toggleSubjectsExpand() {
    subjectsExpanded = !subjectsExpanded;
    if (!subjectsExpanded) {
      selectedSubject = null;
    }
  }

  function selectModule(moduleId: string) {
    selectedModule = moduleId;
    // Here you would add logic to navigate to the module or load its content
    console.log(`Selected module: ${moduleId}`);
  }

  function selectSubject(subjectId: string) {
    selectedSubject = subjectId;
    // Here you would add logic to navigate to the subject or load its content
    console.log(`Selected subject: ${subjectId}`);
  }
</script>

<svelte:head>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700;800&family=Inter:wght@400;500;600&display=swap">
</svelte:head>

<div class="min-h-screen bg-[#f8f9fa] p-4 md:p-8 lg:p-12 flex items-center justify-center">
  <div class="w-full max-w-7xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-6 md:gap-8">
    <!-- Left Panel -->
    <div class="bg-[#002b5c] text-white p-8 md:p-12 rounded-lg flex flex-col shadow-xl">
      <div class="mb-6 flex items-center justify-center">
        <div class="w-24 h-24 bg-white rounded-full flex items-center justify-center shadow-lg transform hover:scale-105 transition-transform duration-300">
          <img src={logo || "/placeholder.svg"} alt="Logo" class="w-20 h-20 rounded-full object-cover border-4 border-[#f08c00]" />
        </div>
      </div>

      <h1 class="text-4xl md:text-5xl font-extrabold leading-tight text-center">CIVIL ENGINEERING SOLUTIONS</h1>

      <p class="text-lg md:text-xl font-inter leading-relaxed text-center mt-4">
        This design tool aid aims to assist both students and instructors during academic interactions in synchronous or asynchronous classes.
      </p>
    </div>

    <!-- Right Panel -->
    <div class="col-span-2 grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-8">
      <!-- Modules Card -->
      <div class="relative bg-[#8c9eff] text-white rounded-lg p-8 flex flex-col justify-center shadow-lg overflow-hidden group">
        <div class="cursor-pointer" on:click={toggleModulesExpand}>
          <h2 class="text-3xl font-bold group-hover:text-white transition-colors">Modules</h2>
          <p class="text-lg opacity-90 group-hover:opacity-100 transition-opacity">Access calculation modules and tools</p>

          <div class="mt-4 bg-white rounded-md p-4 text-gray-800 text-center transform group-hover:scale-105 transition-transform">
            <span class="text-xl font-medium">8 calculation modules available</span>
          </div>
        </div>

        {#if modulesExpanded}
          <div transition:slide={{ duration: 300, easing: quintOut }} class="mt-4 p-4 bg-white text-gray-800 rounded-lg">
            <ul class="space-y-2">
              {#each calculationModules as module}
                <li 
                  class="flex items-center p-2 rounded-md hover:bg-blue-50 cursor-pointer transition-colors {selectedModule === module.id ? 'bg-blue-100 border-l-4 border-blue-500' : ''}"
                  on:click={() => selectModule(module.id)}
                >
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2 text-blue-600">
                    <path stroke-linecap="round" stroke-linejoin="round" d={module.icon} />
                  </svg>
                  <span class="font-medium">{module.name}</span>
                </li>
              {/each}
            </ul>
          </div>
        {/if}
      </div>

      <!-- Subjects Card -->
      <div class="relative bg-[#264653] text-white rounded-lg p-8 flex flex-col justify-center shadow-lg overflow-hidden group">
        <div class="cursor-pointer" on:click={toggleSubjectsExpand}>
          <h2 class="text-3xl font-bold mb-4 group-hover:text-white transition-colors">Subjects</h2>
          <p class="text-lg opacity-90 group-hover:opacity-100 transition-opacity">Browse the engineering knowledge base</p>

          <div class="mt-4 bg-white rounded-md p-4 text-gray-800 text-center transform group-hover:scale-105 transition-transform">
            <span class="text-xl font-medium">3 subjects available</span>
          </div>
        </div>

        {#if subjectsExpanded}
          <div transition:slide={{ duration: 300, easing: quintOut }} class="mt-4 p-4 bg-white text-gray-800 rounded-lg">
            <ul class="space-y-2">
              {#each subjects as subject}
                <li 
                  class="flex items-center p-2 rounded-md hover:bg-teal-50 cursor-pointer transition-colors {selectedSubject === subject.id ? 'bg-teal-100 border-l-4 border-teal-500' : ''}"
                  on:click={() => selectSubject(subject.id)}
                >
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2 text-teal-600">
                    <path stroke-linecap="round" stroke-linejoin="round" d={subject.icon} />
                  </svg>
                  <span class="font-medium">{subject.name}</span>
                </li>
              {/each}
            </ul>
          </div>
        {/if}
      </div>

      <!-- Calendar Card -->
      <div class="bg-[#e76f51] text-white rounded-lg p-8 flex flex-col items-center justify-center shadow-lg transform hover:scale-105 transition-transform duration-300">
        <h2 class="text-3xl font-bold mb-6">Calendar</h2>
        <div class="w-28 h-28 bg-white rounded-md flex items-center justify-center shadow-inner">
          <span class="text-6xl font-extrabold text-gray-800">{day}</span>
        </div>
      </div>

      <!-- Settings Card -->
      <div class="bg-[#b0bec5] text-white rounded-lg p-8 flex flex-col items-center justify-center shadow-lg transform hover:scale-105 transition-transform duration-300">
        <h2 class="text-3xl font-bold mb-6">Settings</h2>
        <div class="w-20 h-20 bg-gray-800 rounded-full flex items-center justify-center shadow-lg">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="text-white w-12 h-12">
            <path fill-rule="evenodd" d="M11.078 2.25c-.917 0-1.699.663-1.85 1.567L9.05 4.889c-.02.12-.115.26-.297.348a7.493 7.493 0 00-.986.57c-.166.115-.334.126-.45.083L6.3 5.508a1.875 1.875 0 00-2.282.819l-.922 1.597a1.875 1.875 0 00.432 2.385l.84.692c.095.078.17.229.154.43a7.598 7.598 0 000 1.139c.015.2-.059.352-.153.43l-.841.692a1.875 1.875 0 00-.432 2.385l.922 1.597a1.875 1.875 0 002.282.818l1.019-.382c.115-.043.283-.031.45.082.312.214.641.405.985.57.182.088.277.228.297.35l.178 1.071c.151.904.933 1.567 1.85 1.567h1.844c.916 0 1.699-.663 1.85-1.567l.178-1.072c.02-.12.114-.26.297-.349.344-.165.673-.356.985-.57.167-.114.335-.125.45-.082l1.02.382a1.875 1.875 0 002.28-.819l.923-1.597a1.875 1.875 0 00-.432-2.385l-.84-.692c-.095-.078-.17-.229-.154-.43a7.614 7.614 0 000-1.139c-.016-.2.059-.352.153-.43l.84-.692c.708-.582.891-1.59.433-2.385l-.922-1.597a1.875 1.875 0 00-2.282-.818l-1.02.382c-.114.043-.282.031-.449-.083a7.49 7.49 0 00-.985-.57c-.183-.087-.277-.227-.297-.348l-.179-1.072a1.875 1.875 0 00-1.85-1.567h-1.843zM12 15.75a3.75 3.75 0 100-7.5 3.75 3.75 0 000 7.5z" clip-rule="evenodd" />
          </svg>
        </div>
      </div>
    </div>
  </div>
</div>

{#if selectedModule}
  <div transition:fade={{ duration: 200 }} class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-lg p-6 max-w-2xl w-full shadow-2xl" transition:slide={{ duration: 300, easing: quintOut }}>
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-2xl font-bold text-gray-800">
          {calculationModules.find(m => m.id === selectedModule)?.name}
        </h2>
        <button 
          class="p-2 rounded-full hover:bg-gray-100" 
          on:click={() => selectedModule = null}
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      <div class="p-4 bg-gray-50 rounded-lg">
        <p class="text-gray-700">Module content would go here. This is where you would implement the specific functionality for the {calculationModules.find(m => m.id === selectedModule)?.name} module.</p>
        
        <!-- Example module content - replace with actual functionality -->
        <div class="mt-4 grid grid-cols-1 md:grid-cols-2 gap-4">
          <div class="bg-white p-4 rounded-lg shadow">
            <h3 class="font-semibold text-lg mb-2">Input Parameters</h3>
            <div class="space-y-2">
              <div>
                <label class="block text-sm font-medium text-gray-700">Parameter 1</label>
                <input type="number" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500" />
              </div>
              <div>
                <label class="block text-sm font-medium text-gray-700">Parameter 2</label>
                <input type="number" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500" />
              </div>
            </div>
            <button class="mt-4 bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700 transition-colors">Calculate</button>
          </div>
          <div class="bg-white p-4 rounded-lg shadow">
            <h3 class="font-semibold text-lg mb-2">Results</h3>
            <div class="h-40 bg-gray-100 rounded-md flex items-center justify-center">
              <p class="text-gray-500">Results will appear here</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
{/if}

{#if selectedSubject}
  <div transition:fade={{ duration: 200 }} class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-lg p-6 max-w-2xl w-full shadow-2xl" transition:slide={{ duration: 300, easing: quintOut }}>
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-2xl font-bold text-gray-800">
          {subjects.find(s => s.id === selectedSubject)?.name}
        </h2>
        <button 
          class="p-2 rounded-full hover:bg-gray-100" 
          on:click={() => selectedSubject = null}
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      <div class="p-4 bg-gray-50 rounded-lg">
        <p class="text-gray-700">Subject content would go here. This is where you would display information about the {subjects.find(s => s.id === selectedSubject)?.name} subject.</p>
        
        <!-- Example subject content - replace with actual content -->
        <div class="mt-4">
          <h3 class="font-semibold text-lg mb-2">Topics</h3>
          <ul class="list-disc pl-5 space-y-1">
            <li>Introduction to {subjects.find(s => s.id === selectedSubject)?.name}</li>
            <li>Fundamental Principles</li>
            <li>Advanced Concepts</li>
            <li>Practical Applications</li>
            <li>Case Studies</li>
          </ul>
          
          <div class="mt-4 p-4 bg-blue-50 rounded-lg border border-blue-200">
            <h4 class="font-medium text-blue-800">Learning Resources</h4>
            <div class="mt-2 grid grid-cols-1 md:grid-cols-2 gap-2">
              <a href="#" class="flex items-center p-2 bg-white rounded-md hover:bg-blue-50">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2 text-blue-600">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.042A8.967 8.967 0 006 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 016 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 016-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0018 18a8.967 8.967 0 00-6 2.292m0-14.25v14.25" />
                </svg>
                <span>Textbook</span>
              </a>
              <a href="#" class="flex items-center p-2 bg-white rounded-md hover:bg-blue-50">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2 text-blue-600">
                  <path stroke-linecap="round" d="M15.75 10.5l4.72-4.72a.75.75 0 011.28.53v11.38a.75.75 0 01-1.28.53l-4.72-4.72M4.5 18.75h9a2.25 2.25 0 002.25-2.25v-9a2.25 2.25 0 00-2.25-2.25h-9A2.25 2.25 0 002.25 7.5v9a2.25 2.25 0 002.25 2.25z" />
                </svg>
                <span>Video Lectures</span>
              </a>
              <a href="#" class="flex items-center p-2 bg-white rounded-md hover:bg-blue-50">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2 text-blue-600">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 002.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 00-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 00.75-.75 2.25 2.25 0 00-.1-.664m-5.8 0A2.251 2.251 0 0113.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25zM6.75 12h.008v.008H6.75V12zm0 3h.008v.008H6.75V15zm0 3h.008v.008H6.75V18z" />
                </svg>
                <span>Practice Problems</span>
              </a>
              <a href="#" class="flex items-center p-2 bg-white rounded-md hover:bg-blue-50">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 mr-2 text-blue-600">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z" />
                </svg>
                <span>Help & FAQ</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
{/if}