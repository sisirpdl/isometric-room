<script lang="ts">
  import { fade, fly } from 'svelte/transition'
  import { cubicOut } from 'svelte/easing'
  import { currentPage } from '$lib/stores/navigation'
  import Projects from '$lib/components/Pages/Projects.svelte'
  import Experience from '$lib/components/Pages/Experience.svelte'
  import Tech from '$lib/components/Pages/Tech.svelte'
  import Education from '$lib/components/Pages/Education.svelte'
  import Hobby from '$lib/components/Pages/Hobby.svelte'
  import Research from '$lib/components/Pages/Research.svelte'

  const pages = {
    projects: Projects,
    experience: Experience,
    tech: Tech,
    education: Education,
    hobby: Hobby,
    research: Research
  }

  function handleClose() {
    $currentPage = null
  }

  function handleBackdropClick(e: MouseEvent) {
    if (e.target === e.currentTarget) {
      handleClose()
    }
  }
</script>

<!-- Backdrop - Dims the 3D scene -->
<div
  class="fixed inset-0 z-50 flex items-center justify-center p-6 md:p-10 lg:p-12"
  style="background: rgba(0, 0, 0, 0.5); backdrop-filter: blur(16px);"
  in:fade={{ duration: 400, easing: cubicOut }}
  out:fade={{ duration: 300 }}
  on:click={handleBackdropClick}
  role="button"
  tabindex="0"
  on:keydown={(e) => e.key === 'Escape' && handleClose()}
>
  <!-- Light Glassmorphic Modal -->
  <div
    class="relative w-full max-w-5xl max-h-[90vh] flex flex-col overflow-hidden"
    style="
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(40px) saturate(180%);
      -webkit-backdrop-filter: blur(40px) saturate(180%);
      border-radius: 24px;
      border: 1px solid rgba(255, 255, 255, 0.8);
      box-shadow:
        0 32px 64px -12px rgba(0, 0, 0, 0.2),
        0 0 0 1px rgba(255, 255, 255, 0.5) inset;
    "
    in:fly={{ y: 60, duration: 600, delay: 150, easing: cubicOut }}
    out:fly={{ y: 40, duration: 400, easing: cubicOut }}
  >
    <!-- Close Button -->
    <button
      on:click={handleClose}
      class="absolute top-6 right-6 z-20 w-11 h-11 flex items-center justify-center transition-all duration-500 rounded-full hover:rotate-180 group"
      style="
        background: rgba(0, 0, 0, 0.04);
        border: 1px solid rgba(0, 0, 0, 0.06);
      "
      aria-label="Close"
    >
      <svg
        width="18"
        height="18"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="1.5"
        stroke-linecap="round"
        class="text-gray-500 group-hover:text-gray-900 transition-all duration-300"
      >
        <line x1="18" y1="6" x2="6" y2="18" />
        <line x1="6" y1="6" x2="18" y2="18" />
      </svg>
    </button>

    <!-- Scrollable Content -->
    <div class="flex-1 overflow-y-auto custom-scrollbar overscroll-contain">
      <div 
        class="w-full max-w-4xl mx-auto px-12 py-10 md:px-24 md:py-14 lg:px-32 lg:py-16"
        style="padding: 2.5rem 3rem;"
      >
        {#if $currentPage}
          <svelte:component this={pages[$currentPage as keyof typeof pages]} />
        {/if}
      </div>
    </div>
  </div>
</div>

<style>
  /* Light Scrollbar */
  .custom-scrollbar::-webkit-scrollbar {
    width: 8px;
  }

  .custom-scrollbar::-webkit-scrollbar-track {
    background: rgba(0, 0, 0, 0.02);
    border-radius: 4px;
  }

  .custom-scrollbar::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.15);
    border-radius: 4px;
    border: 2px solid rgba(255, 255, 255, 0.3);
    transition: background 0.3s ease;
  }

  .custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background: rgba(0, 0, 0, 0.25);
  }

  /* Firefox scrollbar */
  .custom-scrollbar {
    scrollbar-width: thin;
    scrollbar-color: rgba(0, 0, 0, 0.15) rgba(0, 0, 0, 0.02);
  }
</style>
