<script lang="ts">
  import { fade } from 'svelte/transition'
  import { useProgress } from '@threlte/extras'

  const { progress, active } = useProgress()
</script>

{#if $active}
  <div
    class="fixed inset-0 bg-black flex flex-col items-center justify-center z-50"
    out:fade={{ duration: 800 }}
  >
    <div class="text-center space-y-8 relative">
      <!-- Decorative blur behind -->
      <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-64 h-64 bg-white/5 rounded-full blur-3xl pointer-events-none"></div>

      <h1 class="text-white text-6xl md:text-8xl font-bold tracking-tighter relative z-10">
        Portfolio
      </h1>
      
      <div class="flex flex-col items-center gap-4 relative z-10">
        <div class="w-48 h-1 bg-white/10 rounded-full overflow-hidden">
          <div
            class="h-full bg-white transition-all duration-300 ease-out"
            style="width: {$progress * 100}%"
          ></div>
        </div>
        
        <div class="text-white/40 font-mono text-xs font-bold tracking-widest">
          {Math.round($progress * 100)}%
        </div>
      </div>
    </div>
  </div>
{/if}
