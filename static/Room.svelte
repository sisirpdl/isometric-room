<script lang="ts">
  import { T, useTask } from '@threlte/core'
  import { useGltf, useGltfAnimations, interactivity } from '@threlte/extras'
  import { createEventDispatcher } from 'svelte'
  import { Vector3, Color, type Mesh, type Object3D, MeshStandardMaterial, LoopRepeat } from 'three'

  const dispatch = createEventDispatcher()

  // Configuration: Map object names to page routes
  const INTERACTIVE_OBJECTS: Record<string, string> = {
    screen: 'projects',
    guitar: 'hobby',
    books: 'research',
    nataraj: 'research',
    hitbox_experience: 'experience',
    hitbox_tech: 'tech',
    hitbox_education: 'education',
    hitbox_projects: 'projects'
  }

  // Configuration: Map object names to special actions
  const SPECIAL_ACTIONS: Record<string, string> = {
    nataraj: 'rotate_nataraj',
    'printer': 'print_resume', // Will match any object with 'printer' in name
    cat: 'meow',
    welcomemat: 'reset_view',
    chair: 'rotate_chair',
    bag: 'rotate_bag',
    guitar: 'rotate_guitar',
    cup: 'rotate_cup'
  }

  // Configure interactivity plugin with filter to prevent clicking non-interactive objects
  interactivity({
    filter: (hits) => hits.filter((hit) => hit.object.userData.isInteractive)
  })

  // Load 3D model
  const gltf = useGltf('/models/isometric_room_002.glb')
  const { actions } = useGltfAnimations(gltf)

  let interactiveMeshes: Mesh[] = []

  // Helper: Tag all mesh children of an object as interactive
  function tagObjectAsInteractive(object: Object3D, userData: Record<string, any>) {
    // If this is a hitbox, we want to make IT interactive, but make the VISUAL object glow
    const isHitbox = object.name.toLowerCase().startsWith('hitbox')
    let glowTarget = object

    if (isHitbox && userData.glow) {
      // Try to find the visual object (remove 'hitbox_' prefix)
      const visualName = object.name.replace(/^hitbox_?/i, '')
      // We need to search in the scene again
      // Note: This is a bit inefficient but runs only once at startup
      let visualObject = object.parent?.getObjectByName(visualName)
      
      // If not found in parent, try global search (using our findObject helper logic if we had access to it here)
      // Since we don't have easy access to findObject here without passing it or scope, 
      // let's assume the visual object is a sibling or we can find it via the scene root if we had it.
      // Actually, we can just use the userData to store the visual target name and resolve it later?
      // Or better: Pass the scene or findObject to this function.
      
      // For now, let's try to find it in the same scene graph
      if (!visualObject && object.parent) {
         visualObject = object.parent.getObjectByName(visualName)
      }
      
      if (visualObject) {
        glowTarget = visualObject
        // Make sure the hitbox itself is invisible but clickable
        object.traverse((child) => {
          if ((child as Mesh).isMesh) {
            (child as Mesh).visible = false // Hide hitbox
          }
        })
      } else {
        console.warn(`Could not find visual target '${visualName}' for hitbox '${object.name}'`)
      }
    }

    // Tag the interactive object (the hitbox or the object itself)
    object.traverse((child) => {
      if ((child as Mesh).isMesh) {
        const mesh = child as Mesh
        mesh.userData.isInteractive = true
        Object.assign(mesh.userData, { ...userData, rootObject: object })
      }
    })

    // Setup glow on the glow target
    if (userData.glow) {
      glowTarget.traverse((child) => {
        if ((child as Mesh).isMesh) {
          const mesh = child as Mesh
          // We don't tag the visual mesh as interactive if it's separate from the hitbox
          // But we do add it to the glow list
          
          const materials = Array.isArray(mesh.material) ? mesh.material : [mesh.material]
          let hasEmissive = false

          materials.forEach((m) => {
            const mat = m as MeshStandardMaterial
            if (mat.emissive) {
              mat.emissive = new Color(0xffd700) // Warm Gold glow
              mat.emissiveIntensity = 0
              hasEmissive = true
            }
          })

          if (hasEmissive) {
            interactiveMeshes.push(mesh)
          }
        }
      })
    }
  }

  // Glow Animation Loop
  useTask((delta) => {
    const time = performance.now() / 1000
    // 3 second cycle
    const cycle = time % 3
    
    // Pulse logic:
    // We want a "heartbeat" or "breath" every 3 seconds.
    // Let's say the pulse lasts 1.5 seconds, then 1.5 seconds of rest.
    let intensity = 0
    
    if (cycle < 1.5) {
      // Sine wave from 0 to PI (0 -> 1 -> 0)
      // (cycle / 1.5) goes from 0 to 1
      intensity = Math.sin((cycle / 1.5) * Math.PI) * 0.3 // Max intensity 0.3
    }

    interactiveMeshes.forEach((mesh) => {
      const materials = Array.isArray(mesh.material) ? mesh.material : [mesh.material]
      materials.forEach((m) => {
        const mat = m as MeshStandardMaterial
        if (mat && mat.emissiveIntensity !== undefined) {
          mat.emissiveIntensity = intensity
        }
      })
    })
  })

  // Reactive: Tag interactive objects when model loads
  $: if ($gltf) {
    // Helper to find object with loose matching
    const findObject = (name: string) => {
      // 1. Try exact match
      let found = $gltf.scene.getObjectByName(name)
      
      // 2. Try case-insensitive match
      if (!found) {
        $gltf.scene.traverse((child) => {
          if (!found && child.name.toLowerCase() === name.toLowerCase()) {
            found = child
          }
        })
      }

      // 3. Try partial match (useful if name is like "Printer_Mesh" or "printer.001")
      if (!found) {
        $gltf.scene.traverse((child) => {
          if (!found && child.name.toLowerCase().includes(name.toLowerCase())) {
            found = child
          }
        })
      }
      
      return found
    }

    // Tag page navigation objects
    Object.entries(INTERACTIVE_OBJECTS).forEach(([name, page]) => {
      const object = findObject(name)
      if (object) {
        // Guitar and books look bad with glow, so we disable it
        const shouldGlow = name !== 'guitar' && name !== 'books'
        tagObjectAsInteractive(object, { targetPage: page, glow: shouldGlow })
      } else {
        console.warn(`Interactive object not found: ${name}`)
      }
    })

    // Tag special action objects
    Object.entries(SPECIAL_ACTIONS).forEach(([name, action]) => {
      let object = findObject(name)

      // Fallback: If searching for "printer", try to find the FIRST object with "printer" or "print" in the name
      if (!object && name === 'printer') {
        $gltf.scene.traverse((child) => {
          if (!object && (child.name.toLowerCase().includes('printer') || child.name.toLowerCase().includes('print'))) {
            console.log(`Fallback found for printer: ${child.name}`)
            object = child
            // Important: Don't break out, just set object once and the !object check will prevent duplicates
          }
        })
      }

      if (object) {
        console.log(`Found special object: ${object.name} (searched for ${name}), tagging...`)
        // Only printer should glow from special actions list
        const shouldGlow = object.name.toLowerCase().includes('printer') ||
                          object.name.toLowerCase().includes('print')
        tagObjectAsInteractive(object, { specialAction: action, objectName: name, glow: shouldGlow })
      } else {
        console.warn(`Special action object not found: ${name}`)
      }
    })
    
    // Debug: Log all object names to help troubleshooting
    if ($gltf.nodes) {
      const allKeys = Object.keys($gltf.nodes)
      console.log('Scene loaded. Total objects:', allKeys.length)
      console.log('Objects containing "print", "001", "cube":', 
        allKeys.filter(k => {
          const lower = k.toLowerCase()
          return lower.includes('print') || lower.includes('001') || lower.includes('cube')
        })
      )
    }
  } else {
    // Clear interactive meshes if model unloads/reloads
    interactiveMeshes = []
  }

  // Helper: Rotate around world Y axis (floor plane)
  function animateRotation(object: Object3D, duration: number = 1000, onComplete?: () => void) {
    const startTime = performance.now()
    let lastProgress = 0
    const axis = new Vector3(0, 1, 0) // World Up (Floor Plane Normal)

    function update(currentTime: number) {
      const elapsed = currentTime - startTime
      const progress = Math.min(elapsed / duration, 1)
      
      // Easing (easeInOutQuad)
      const ease = progress < 0.5 ? 2 * progress * progress : 1 - Math.pow(-2 * progress + 2, 2) / 2
      
      const delta = (ease - lastProgress) * Math.PI * 2
      object.rotateOnWorldAxis(axis, delta)
      lastProgress = ease

      if (progress < 1) {
        requestAnimationFrame(update)
      } else {
        if (onComplete) onComplete()
      }
    }
    
    requestAnimationFrame(update)
  }

  // Handle click events on interactive objects
  function handleClick(event: any) {
    const { targetPage, specialAction, objectName, rootObject } = event.object.userData
    
    // Use the root object for animations if available, otherwise fallback to the clicked mesh
    const targetObject = rootObject || event.object

    const performNavigation = () => {
      if (targetPage) {
        console.log('Navigating to:', targetPage)
        dispatch('objectClick', { page: targetPage })
      }
    }

    if (specialAction) {
      console.log('Triggering action:', specialAction, 'from:', objectName)
      
      // Handle animations locally
      if (specialAction === 'print_resume' && $actions) {
        // Prevent multiple clicks while animation is playing
        if (targetObject.userData.isPrinting) {
          console.log('Printer animation already in progress, ignoring click')
          return
        }
        targetObject.userData.isPrinting = true

        console.log('Available animations:', Object.keys($actions))

        // Find animation that contains 'printer', 'print', or 'Cube_0.040'
        const printerAnim = Object.keys($actions).find(key => {
          const k = key.toLowerCase()
          return k.includes('printer') || k.includes('print') || (k.includes('cube') && k.includes('040'))
        })

        if (printerAnim) {
          console.log('Playing animation:', printerAnim)
          const action = $actions[printerAnim]

          if (action) {
            action.reset()
            // Play once then stop
            action.setLoop(LoopRepeat, 1)
            action.clampWhenFinished = true
            action.play()

            // Get mixer from the action itself to avoid store issues
            const mixer = action.getMixer()

            // Wait for animation to finish before triggering the action
            const onFinished = (e: any) => {
              // Check if the finished action is the one we played
              if (e.action === action) {
                console.log('Printer animation finished, triggering resume...')
                mixer.removeEventListener('finished', onFinished)
                targetObject.userData.isPrinting = false
                dispatch('specialAction', { action: specialAction, objectName })
              }
            }
            mixer.addEventListener('finished', onFinished)
          } else {
             // Should not happen if printerAnim is found, but safety first
             targetObject.userData.isPrinting = false
             dispatch('specialAction', { action: specialAction, objectName })
          }
        } else {
          console.warn('Printer animation not found')
          // If no animation, trigger immediately
          targetObject.userData.isPrinting = false
          dispatch('specialAction', { action: specialAction, objectName })
        }
        performNavigation()
      } else if (specialAction.startsWith('rotate_')) {
        // Rotate object 360 degrees on World Y axis (floor plane)
        
        // Prevent multiple rotations if already rotating
        if (targetObject.userData.isRotating) return
        targetObject.userData.isRotating = true

        const onFinish = () => {
          targetObject.userData.isRotating = false
          performNavigation()
        }

        // If there is a target page, wait for rotation to finish before navigating
        if (targetPage) {
          animateRotation(targetObject, 1000, onFinish)
        } else {
          animateRotation(targetObject, 1000, () => {
            targetObject.userData.isRotating = false
          })
          performNavigation()
        }
        dispatch('specialAction', { action: specialAction, objectName })
      } else {
        performNavigation()
        dispatch('specialAction', { action: specialAction, objectName })
      }
    } else {
      // No special action, just navigate immediately
      performNavigation()
    }
  }

  // Cursor management
  const setCursor = (cursor: string) => () => {
    document.body.style.cursor = cursor
  }
</script>

{#if $gltf}
  <T
    is={$gltf.scene}
    position={[0, -1.2, 0]}
    rotation={[0, (Math.PI / 2) + 0.25, 0]}
    onclick={handleClick}
    onpointerenter={setCursor('pointer')}
    onpointerleave={setCursor('auto')}
  />
{/if}
