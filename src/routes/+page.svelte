<script lang="ts">
  import IsometricRoom from '$lib/components/Scene/IsometricRoom.svelte'
  import PageOverlay from '$lib/components/UI/PageOverlay.svelte'
  import LoadingScreen from '$lib/components/UI/LoadingScreen.svelte'
  import { currentPage } from '$lib/stores/navigation'

  // Special action handlers
  const SPECIAL_ACTION_HANDLERS: Record<string, () => void> = {
    open_book: () => {
      window.open('YOUR_BOOK_LINK_HERE', '_blank')
    },
    print_resume: () => {
      console.log('Attempting to print resume...')
      
      // Use a persistent iframe or create one if it doesn't exist
      let iframe = document.getElementById('print-iframe') as HTMLIFrameElement
      if (!iframe) {
        iframe = document.createElement('iframe')
        iframe.id = 'print-iframe'
        iframe.style.position = 'fixed'
        iframe.style.right = '0'
        iframe.style.bottom = '0'
        iframe.style.width = '1px' // Must have size to render
        iframe.style.height = '1px'
        iframe.style.opacity = '0.01'
        iframe.style.pointerEvents = 'none'
        iframe.style.border = '0'
        document.body.appendChild(iframe)
      }
      
      // Force reload if src is same
      iframe.src = '/resume.pdf'
      
      iframe.onload = () => {
        // Small delay to ensure PDF rendering
        setTimeout(() => {
          try {
            if (iframe.contentWindow) {
              iframe.contentWindow.focus()
              iframe.contentWindow.print()
            }
          } catch (e) {
            console.error('Printing failed', e)
            window.open('/resume.pdf', '_blank')
          }
        }, 1000)
      }
    },
    meow: () => {
      const audio = new Audio('/cat-sound.mp3')
      audio.play().catch((err) => console.error('Audio play failed:', err))
    }
  }

  function handleObjectClick(event: CustomEvent<{ page: string }>) {
    const { page } = event.detail
    console.log('Navigating to page:', page)
    $currentPage = page
  }

  function handleSpecialAction(event: CustomEvent<{ action: string; objectName: string }>) {
    const { action, objectName } = event.detail
    console.log('Special action triggered:', action, 'from:', objectName)

    const handler = SPECIAL_ACTION_HANDLERS[action]
    if (handler) {
      handler()
    } else {
      console.warn('Unknown special action:', action)
    }
  }
</script>

<svelte:head>
  <title>Portfolio</title>
  <meta name="description" content="3D Interactive Portfolio" />
</svelte:head>

<main class="w-full h-screen">
  <LoadingScreen />

  <!-- 3D Room - Always Mounted -->
  <IsometricRoom
    on:objectClick={handleObjectClick}
    on:specialAction={handleSpecialAction}
  />

  <!-- Content Overlay - Shows on top when page selected -->
  {#if $currentPage}
    <PageOverlay />
  {/if}
</main>
