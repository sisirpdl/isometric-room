# AI Development Guide - 3D Portfolio Project

## Project Overview
Interactive 3D isometric portfolio built with SvelteKit + Threlte (Three.js wrapper)

**Tech Stack:**
- SvelteKit
- Threlte (@threlte/core + @threlte/extras)
- Three.js
- TypeScript
- TailwindCSS

---

## Critical Decisions & Lessons Learned

### 1. INTERACTIVITY SETUP - CRITICAL ⚠️

**Problem:** Clicks on GLTF objects not working

**Root Causes:**
1. `interactivity()` plugin must be called INSIDE a Canvas child component
2. Must use `onclick` (lowercase) instead of `on:click` for Threlte
3. GLTF component needs `interactive` prop
4. Component mounting timing issues with conditional rendering
5. **IMPORTANT:** There is NO `<Interactivity />` component - it's a function!

**CORRECT Solution (Threlte v8/9):**
```svelte
<!-- Room.svelte - Component INSIDE Canvas -->
<script lang="ts">
  import { GLTF, interactivity } from '@threlte/extras'

  // Call at TOP LEVEL of script - NOT in onMount!
  // This must run synchronously when component initializes
  interactivity()
</script>

<GLTF
  url="/models/model.glb"
  interactive          <!-- Required prop -->
  onclick={handleClick}  <!-- Lowercase, not on:click -->
/>
```

**WRONG Approaches (Don't use these):**
```svelte
❌ import { Interactivity } from '@threlte/extras'  // Doesn't exist!
❌ onMount(() => interactivity())  // Too late, causes errors
❌ on:click={handleClick}  // Wrong event name for Threlte
```

**Versions Tested:**
- @threlte/core: 8.3.1
- @threlte/extras: 9.7.1

**Resources:**
- [Threlte Interactivity Docs](https://threlte.xyz/docs/reference/extras/interactivity)
- [Handling Events Guide](https://github.com/threlte/threlte/blob/main/apps/docs/src/content/learn/basics/handling-events.mdx)

---

### 2. LOADING SCREEN ISSUES

**Problem:** Page stuck on loading screen

**Causes:**
1. Conditional rendering prevents component from mounting
2. Events not propagating properly
3. Model loads but progress events not firing

**Solution:**
```svelte
<!-- Use CSS hiding instead of conditional rendering -->
{#if !$currentPage}
  <div class:hidden={isLoading}>  <!-- Hidden but mounted -->
    <IsometricRoom on:progress on:loaded />
  </div>
{/if}

<!-- Add timeout fallback -->
let loadingTimeout: NodeJS.Timeout
$: if (isLoading && typeof window !== 'undefined') {
  loadingTimeout = setTimeout(() => {
    loadingProgress = 100
    handleLoadingComplete()
  }, 5000)
}
```

**Key Insight:** Component must be mounted (not conditionally removed) to receive events

---

### 3. CLICK DETECTION WITH WALLS

**Problem:** Walls/floor absorbing clicks meant for interactive objects

**Solution:** Check ALL intersections, not just the first one
```typescript
function handleClick(event: any) {
  const intersections = event.intersections || []
  const ignoreList = ['wall', 'floor', 'ceiling', 'room', 'base', 'ground', 'Cube']

  // Find first interactive object in ALL intersections
  for (const intersection of intersections) {
    const objName = intersection.object?.name || ''

    // Skip non-interactive objects
    if (!objName || ignoreList.some(ignore =>
      objName.toLowerCase().includes(ignore.toLowerCase())
    )) {
      continue
    }

    // Check if interactive
    if (interactiveObjects[objName] || specialActions[objName]) {
      foundObjectName = objName
      break
    }
  }
}
```

**Key Objects to Ignore:**
- Add wall/floor/ceiling object names from your Blender model to `ignoreList`

---

### 4. LIGHTING FOR ISOMETRIC ROOMS

**Best Practices:**
- Use `HemisphereLight` for natural ambient lighting (better for isometric than AmbientLight)
- Multiple `DirectionalLight` from different angles (no harsh shadows)
- Strategic `PointLight` placement for even coverage
- Higher intensities needed for good visibility

**Our Setup:**
```svelte
<!-- Sky/ground ambient -->
<T.HemisphereLight intensity={2.5} skyColor="#ffffff" groundColor="#444444" />

<!-- Main directional -->
<T.DirectionalLight position={[8, 12, 8]} intensity={3.5} castShadow />

<!-- Fill light opposite angle -->
<T.DirectionalLight position={[-5, 8, -5]} intensity={2.0} />

<!-- Point lights at corners -->
<T.PointLight position={[0, 6, 0]} intensity={2.5} />
<T.PointLight position={[-4, 4, 4]} intensity={1.8} />
<T.PointLight position={[4, 4, 4]} intensity={1.8} />
```

**Resources:**
- [Threlte Hemisphere Light](https://sbcode.net/threejs/hemisphere-light/)
- [Three.js Lighting Guide](https://blog.logrocket.com/how-to-use-lighting-and-renderers-in-three-js/)

---

### 5. OBJECT NAMING IN BLENDER

**Critical:** Object names in Blender MUST match exactly in code

**Our Mapping:**
```typescript
const interactiveObjects = {
  'screen': 'projects',      // Blender object name: "screen"
  'guitar': 'hobby',         // Blender object name: "guitar"
  'books': 'research',       // Blender object name: "books"
  'hitbox_experience': 'experience',
  'hitbox_tech': 'tech',
  'hitbox_education': 'education',
  'hitbox_projects': 'projects'
}
```

**Special Actions:**
```typescript
const specialActions = {
  'nataraj': 'open_book',    // Opens book link in new tab
  'printer': 'print_resume', // Triggers window.print()
  'cat': 'meow'              // Plays meow sound
}
```

---

### 6. EVENT FORWARDING IN SVELTE

**Pattern:** Events must be forwarded through component hierarchy

```svelte
<!-- Room.svelte - Emits events -->
dispatch('objectClick', { page })
dispatch('specialAction', { action, objectName })
dispatch('progress', e.detail)
dispatch('loaded')

<!-- IsometricRoom.svelte - Forwards events -->
<Room on:objectClick on:specialAction on:progress on:loaded />

<!-- +page.svelte - Handles events -->
<IsometricRoom
  on:objectClick={handleObjectClick}
  on:specialAction={handleSpecialAction}
  on:progress={handleModelProgress}
  on:loaded={handleModelLoaded}
/>
```

---

### 7. CAMERA SETUP FOR ISOMETRIC VIEW

**Use OrthographicCamera, not PerspectiveCamera**

```svelte
<T.OrthographicCamera
  makeDefault
  position={[17.557, 18, -27.461]}
  rotation={[54.7 * Math.PI / 180, 45 * Math.PI / 180, 0]}
  zoom={200}
>
  <OrbitControls enableZoom={true} />
</T.OrthographicCamera>
```

**Key Settings:**
- `makeDefault` - Makes this the active camera
- `rotation` - 54.7° and 45° for classic isometric view
- `zoom` - Adjust based on model size

---

### 8. PAGE NAVIGATION SYSTEM

**Store-based navigation:**
```typescript
// stores/navigation.js
import { writable } from 'svelte/store'
export const currentPage = writable(null)

// Usage in components
$currentPage = 'projects'  // Navigate to page
$currentPage = null        // Back to 3D room
```

**Page Overlay with Transitions:**
```svelte
<div
  class="fixed inset-0 z-50 bg-white overflow-y-auto"
  in:fly={{ y: 50, duration: 500 }}
  out:fly={{ y: 50, duration: 500 }}
>
  <svelte:component this={pages[$currentPage]} />
</div>
```

---

## Current Issues & Debugging

### Issue: Clicks Still Not Working

**Debug Steps:**
1. Check console for "Pointer entered" on hover (verifies interactivity plugin loaded)
2. If no "Pointer entered":
   - Verify `interactivity()` is called in `onMount()`
   - Check component is mounted (not conditionally hidden with `{#if}`)
3. Check console for "Click event received:" on click
4. Look at "All intersections:" array - are your object names there?
5. Verify object names in Blender match exactly (case-sensitive)

**Console Commands to Test:**
```javascript
// Check if currentPage store exists
import { currentPage } from './stores/navigation'
console.log($currentPage)

// Manually trigger navigation
currentPage.set('projects')
currentPage.set(null)  // Back to room
```

---

## Project Structure

```
my-portfolio/
├── static/
│   ├── models/
│   │   └── isometric_room_002.glb
│   └── sounds/
│       └── meow.mp3
├── src/
│   ├── lib/
│   │   ├── components/
│   │   │   ├── Scene/
│   │   │   │   ├── IsometricRoom.svelte  (Canvas wrapper)
│   │   │   │   ├── Room.svelte           (GLTF + interactivity)
│   │   │   │   └── Lights.svelte         (Light setup)
│   │   │   ├── UI/
│   │   │   │   ├── LoadingScreen.svelte
│   │   │   │   └── PageOverlay.svelte
│   │   │   └── Pages/
│   │   │       ├── Projects.svelte
│   │   │       ├── Experience.svelte
│   │   │       ├── Tech.svelte
│   │   │       ├── Education.svelte
│   │   │       ├── Hobby.svelte
│   │   │       └── Research.svelte
│   │   └── stores/
│   │       └── navigation.js
│   └── routes/
│       └── +page.svelte
```

---

## Common Errors & Fixes

### Error: "useDOM can only be used in child of Canvas"
**Cause:** `interactivity()` called outside Canvas context or before mount
**Fix:** Wrap in `onMount()` hook

### Error: Clicks not registering
**Possible causes:**
1. Missing `interactive` prop on GLTF
2. Using `on:click` instead of `onclick`
3. `interactivity()` not called
4. Component not mounted (conditionally rendered)
5. OrbitControls capturing all events

### Error: Loading screen stuck at 0%
**Cause:** IsometricRoom not mounted, can't emit events
**Fix:** Use CSS `hidden` class instead of `{#if}`

---

## Performance Notes

- **RequestAnimationFrame warnings:** Normal for complex 3D scenes on first load
- **First load:** ~1-2 seconds for GLTF parsing
- **Cached loads:** Nearly instant
- **Lighting impact:** 7 lights is reasonable, more may affect performance

---

## Current Status

**What's Working:**
- ✅ 3D scene renders
- ✅ Loading screen with timeout
- ✅ Page overlay system
- ✅ Navigation store
- ✅ Enhanced lighting
- ✅ All 6 page components created

**What's NOT Working:**
- ❌ Click interactivity (RED CUBE TEST: FAILED)
- ❌ Object clicks not registering
- ❌ `interactivity()` plugin not functioning

**Current Investigation:**
- Red test cube added at position [5, 2, 0]
- Test cube should show "TEST CUBE CLICKED!" but doesn't
- This confirms interactivity plugin itself is broken
- Need to verify Threlte version compatibility

---

## TODO Items

- [x] Phase 1: SvelteKit + Threlte setup
- [x] Phase 2: 3D scene with GLTF model
- [x] Phase 3: Page overlays (6 pages)
- [x] Phase 4: Loading screen
- [ ] **CRITICAL: Fix interactivity plugin**
- [ ] Remove test red cube once interactivity works
- [ ] Verify object names in Blender match code
- [ ] Add actual book link (replace YOUR_BOOK_LINK_HERE)
- [ ] Add meow.mp3 sound file to /static/sounds/
- [ ] Implement printer animation trigger
- [ ] Populate all page components with real content
- [ ] Add project images to /static/
- [ ] Test on mobile devices
- [ ] Optimize GLTF model if needed
- [ ] Add analytics
- [ ] SEO optimization

---

## Debugging Steps for Interactivity Issue

### Step 1: Verify Threlte Installation
```bash
npm list @threlte/core @threlte/extras
# Should show: @threlte/core@8.3.1, @threlte/extras@9.7.1
```

### Step 2: Check Console for Errors
Look for:
- "useDOM can only be used in child of Canvas" ❌ (means wrong placement)
- No errors ✅ (plugin loaded correctly)

### Step 3: Test Basic Mesh Click
Red cube at [5, 2, 0] should be visible and clickable.
- If cube visible but not clickable → interactivity() not working
- If cube not visible → rendering issue

### Step 4: Verify Object Names in Blender
Open `isometric_room_002.glb` in Blender:
```
Expected names (case-sensitive):
- screen (not Monitor, not SCREEN)
- guitar (not Guitar)
- books (not Books)
- hitbox_experience
- hitbox_tech
- hitbox_education
- hitbox_projects
- nataraj
- printer
- cat
```

### Step 5: Alternative Approach if interactivity() Fails
Try vanilla Three.js raycasting:
```typescript
import { useFrame } from '@threlte/core'
import * as THREE from 'three'

const raycaster = new THREE.Raycaster()
const mouse = new THREE.Vector2()

// Add click event to canvas manually
```

---

## Known Issues & Workarounds

### Issue: interactivity() Not Activating Clicks
**Symptoms:** Test cube and GLTF objects don't respond to clicks
**Tried Solutions:**
1. ❌ Calling in `onMount()`
2. ❌ Using `<Interactivity />` component (doesn't exist)
3. ✅ Calling at top level of Room.svelte script

**Status:** Still investigating

### Issue: Loading Screen Times Out Every Time
**Cause:** Model loads but `on:load` event not firing properly
**Workaround:** 3-second timeout forces completion
**TODO:** Hook into actual GLTF loading events properly

---

## Resources & References

- [Threlte Documentation](https://threlte.xyz/)
- [Three.js Docs](https://threejs.org/docs/)
- [SvelteKit Docs](https://kit.svelte.dev/)
- [3D Portfolio Example](https://github.com/sha5b/threlte.shahabverse/)
- [Isometric Three.js Example](https://github.com/jeremt/svelte-3d-portfolio)

---

**Last Updated:** December 2024
**Status:** Phase 4 complete, debugging click interactivity
