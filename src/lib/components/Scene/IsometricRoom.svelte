<script lang="ts">
  import { Canvas } from '@threlte/core'
  import { T } from '@threlte/core'
  import { OrbitControls, Suspense } from '@threlte/extras'
  import Room from './Room.svelte'
  import Lights from './Lights.svelte'
  import { createEventDispatcher, onMount } from 'svelte'

  const dispatch = createEventDispatcher()
  
  let controls: any
  let zoom = 200
  let offsetY = 0

  onMount(() => {
    const handleResize = () => {
      const isMobile = window.innerWidth < 768
      // Lower zoom value = zoomed out (shows more)
      // Higher zoom value = zoomed in (shows less)
      zoom = isMobile ? 75 : 200
      
      // Move room up on mobile to center it better
      offsetY = isMobile ? 1.0 : 0
    }
    
    handleResize()
    window.addEventListener('resize', handleResize)
    
    return () => window.removeEventListener('resize', handleResize)
  })

  function handleSpecialAction(event: CustomEvent) {
    const { action } = event.detail
    if (action === 'reset_view') {
      controls?.reset()
    }
    // Forward the event to parent
    dispatch('specialAction', event.detail)
  }
</script>

<div style="width: 100vw; height: 100vh; background-color: #2a2a2a; position: relative;">
  <Canvas shadows>
    <T.OrthographicCamera
      makeDefault
      position={[17.557, 18, -27.461]}
      rotation={[54.7 * Math.PI / 180, 45 * Math.PI / 180, 0]}
      zoom={zoom}
    >
      <OrbitControls
        bind:ref={controls}
        enableZoom={true}
        enableDamping={true}
        dampingFactor={0.05}
        rotateSpeed={1.5}
      />
    </T.OrthographicCamera>

    <Suspense>
      <Lights />
      <T.Group position={[0, offsetY, 0]}>
        <Room
          on:objectClick
          on:specialAction={handleSpecialAction}
        />
      </T.Group>
    </Suspense>
  </Canvas>
</div>
