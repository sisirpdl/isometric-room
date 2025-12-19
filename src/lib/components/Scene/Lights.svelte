<script>
  import { T } from '@threlte/core'
  import { RectAreaLightUniformsLib } from 'three/examples/jsm/lights/RectAreaLightUniformsLib.js'
  
  // Required to make RectAreaLights work in Three.js
  RectAreaLightUniformsLib.init()
</script>

<!-- 1. NIGHT AMBIENT: Lowered slightly for more contrast -->
<T.AmbientLight
  intensity={2.7}
  color="#ffffff"
/>

<!-- 2. RIGHT WALL WASH: The "Main Character" light -->
<!-- ROTATION FIXED: Changed from -PI/2 (Left) to +PI/2 (Right) -->
<!-- Now effectively acting as a large window/LED panel on the Right Wall -->
<T.RectAreaLight
  position={[0.4, 2, 0]}
  width={8}
  height={12}
  intensity={2}
  color="#ffaa55"
  rotation={[0, Math.PI / 2, 0]}
/>

<!-- 3. DRAMATIC KEY LIGHT: Highlighting Right-Side Furniture -->
<!-- Shifted position to favor the right side of the room -->
<T.SpotLight
  position={[-2, 15, 5]}
  intensity={5.4}
  angle={0.6}
  penumbra={0.5}
  castShadow
  shadow-mapSize-width={2048}
  shadow-mapSize-height={2048}
  shadow-bias={-0.0001}
  color="#ffeecc"
/>
<!-- 4. DESK/MONITOR GLOW: Local light to make the workspace pop -->
<T.PointLight
  position={[0, 1.5, 0.2]}
  intensity={3.6}
  distance={5}
  decay={2}
  color="#aaccff"
/>