<script setup>
import { onMounted, ref, watch } from 'vue'
import * as THREE from 'three'

const canvas = ref(null)
const isDarkMode = ref(false)
const sceneRef = ref(null)
const rendererRef = ref(null)
const cameraRef = ref(null)
const segmentsRef = ref([])
const colorMeshesRef = ref([])

// Configuração
const CONFIG = {
  TUNNEL_WIDTH: 30,
  TUNNEL_HEIGHT: 20,
  SEGMENT_DEPTH: 8,
  NUM_SEGMENTS: 14,
  FLOOR_COLS: 6,
  WALL_ROWS: 4,
  MOVE_SPEED: 0.15,
  CAMERA_THRESHOLD: 10,
}

const NEON_COLORS = [
  0xffffff, 0xff5500
]

const createColorPlane = (group, pos, rot, width, height) => {
  const color = NEON_COLORS[Math.floor(Math.random() * NEON_COLORS.length)]
  const geometry = new THREE.PlaneGeometry(width - 0.4, height - 0.4)
  const material = new THREE.MeshBasicMaterial({
    color,
    side: THREE.DoubleSide,
    transparent: true,
    opacity: 0,
  })
  const mesh = new THREE.Mesh(geometry, material)
  mesh.position.copy(pos)
  mesh.rotation.copy(rot)
  mesh.userData = { targetOpacity: .8, currentOpacity: 0 }
  group.add(mesh)
  
  colorMeshesRef.value.push(mesh)
}

const createGridLines = (group, w, h, d, colWidth, rowHeight) => {
  const material = new THREE.LineBasicMaterial({ color: 0x343434, transparent: true, opacity: 0.5 })
  const geometry = new THREE.BufferGeometry()
  const vertices = []

  // Linhas longitudinais
  for (let i = 0; i <= CONFIG.FLOOR_COLS; i++) {
    const x = -w + (i * colWidth)
    vertices.push(x, -h, 0, x, -h, -d)
    vertices.push(x, h, 0, x, h, -d)
  }

  for (let i = 1; i < CONFIG.WALL_ROWS; i++) {
    const y = -h + (i * rowHeight)
    vertices.push(-w, y, 0, -w, y, -d)
    vertices.push(w, y, 0, w, y, -d)
  }

  // Linhas latitudinais
  vertices.push(-w, -h, 0, w, -h, 0)
  vertices.push(-w, h, 0, w, h, 0)
  vertices.push(-w, -h, 0, -w, h, 0)
  vertices.push(w, -h, 0, w, h, 0)

  geometry.setAttribute('position', new THREE.Float32BufferAttribute(vertices, 3))
  const lines = new THREE.LineSegments(geometry, material)
  group.add(lines)
}

const populateSegment = (group, w, h, d, colWidth, rowHeight) => {
  const addPlanes = (surfaces) => {
    surfaces.forEach(({ count, randomThreshold, positions }) => {
      let lastIdx = -999
      for (let i = 0; i < count; i++) {
        if (i > lastIdx + 1 && Math.random() > randomThreshold) {
          const { pos, rot, width, height } = positions(i, w, h, d, colWidth, rowHeight)
          createColorPlane(group, pos, rot, width, height)
          lastIdx = i
        }
      }
    })
  }

  addPlanes([
    {
      count: CONFIG.FLOOR_COLS,
      randomThreshold: 0.8,
      positions: (i, w, h, d, cw, rh) => ({
        pos: new THREE.Vector3(-w + i * cw + cw / 2, -h, -d / 2),
        rot: new THREE.Euler(-Math.PI / 2, 0, 0),
        width: cw,
        height: d,
      }),
    },
    {
      count: CONFIG.FLOOR_COLS,
      randomThreshold: 0.88,
      positions: (i, w, h, d, cw, rh) => ({
        pos: new THREE.Vector3(-w + i * cw + cw / 2, h, -d / 2),
        rot: new THREE.Euler(Math.PI / 2, 0, 0),
        width: cw,
        height: d,
      }),
    },
    {
      count: CONFIG.WALL_ROWS,
      randomThreshold: 0.8,
      positions: (i, w, h, d, cw, rh) => ({
        pos: new THREE.Vector3(-w, -h + i * rh + rh / 2, -d / 2),
        rot: new THREE.Euler(0, Math.PI / 2, 0),
        width: d,
        height: rh,
      }),
    },
    {
      count: CONFIG.WALL_ROWS,
      randomThreshold: 0.8,
      positions: (i, w, h, d, cw, rh) => ({
        pos: new THREE.Vector3(w, -h + i * rh + rh / 2, -d / 2),
        rot: new THREE.Euler(0, -Math.PI / 2, 0),
        width: d,
        height: rh,
      }),
    },
  ])
}

const createSegment = (zPos) => {
  const group = new THREE.Group()
  group.position.z = zPos

  const w = CONFIG.TUNNEL_WIDTH / 2
  const h = CONFIG.TUNNEL_HEIGHT / 2
  const d = CONFIG.SEGMENT_DEPTH
  const colWidth = CONFIG.TUNNEL_WIDTH / CONFIG.FLOOR_COLS
  const rowHeight = CONFIG.TUNNEL_HEIGHT / CONFIG.WALL_ROWS

  createGridLines(group, w, h, d, colWidth, rowHeight)
  populateSegment(group, w, h, d, colWidth, rowHeight)

  return group
}

onMounted(() => {
  if (!canvas.value) return

  const scene = new THREE.Scene()
  sceneRef.value = scene

  const { innerWidth: width, innerHeight: height } = window
  const camera = new THREE.PerspectiveCamera(70, width / height, 0.1, 1000)
  camera.position.set(0, 0, 0)
  cameraRef.value = camera

  const renderer = new THREE.WebGLRenderer({
    canvas: canvas.value,
    antialias: true,
    alpha: false,
    powerPreference: 'high-performance',
  })
  renderer.setSize(width, height)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  rendererRef.value = renderer

  const segments = Array.from({ length: CONFIG.NUM_SEGMENTS }, (_, i) =>
    createSegment(-i * CONFIG.SEGMENT_DEPTH)
  )
  segments.forEach(seg => scene.add(seg))
  segmentsRef.value = segments

  let frameId
  const animate = () => {
    frameId = requestAnimationFrame(animate)
    if (!cameraRef.value || !rendererRef.value) return

    cameraRef.value.position.z -= CONFIG.MOVE_SPEED

    // Animar opacidade das cores
    colorMeshesRef.value.forEach((mesh) => {
      const userData = mesh.userData
      if (userData.currentOpacity < userData.targetOpacity) {
        userData.currentOpacity += 0.01
        mesh.material.opacity = Math.min(userData.currentOpacity, userData.targetOpacity)
      }
    })

    segmentsRef.value.forEach((segment) => {
      if (segment.position.z > cameraRef.value.position.z + CONFIG.CAMERA_THRESHOLD) {
        const minZ = Math.min(...segmentsRef.value.map(s => s.position.z))
        segment.position.z = minZ - CONFIG.SEGMENT_DEPTH
        
        // Reset opacidade das cores quando segmento é reciclado
        segment.traverse((child) => {
          if (child instanceof THREE.Mesh && child.userData.targetOpacity) {
            child.userData.currentOpacity = 0
            child.material.opacity = 0
          }
        })
      }
    })

    rendererRef.value.render(scene, cameraRef.value)
  }
  animate()

  const handleResize = () => {
    const { innerWidth: w, innerHeight: h } = window
    camera.aspect = w / h
    camera.updateProjectionMatrix()
    renderer.setSize(w, h)
  }
  window.addEventListener('resize', handleResize)

  return () => {
    window.removeEventListener('resize', handleResize)
    cancelAnimationFrame(frameId)
    renderer.dispose()
  }
})

watch(isDarkMode, () => {
  if (!sceneRef.value) return

  const bgHex = isDarkMode.value ? 0x050505 : 0xffffff
  const lineHex = isDarkMode.value ? 0x555555 : 0xb0b0b0
  const lineOp = isDarkMode.value ? 0.35 : 0.5

  sceneRef.value.background = new THREE.Color(bgHex)
  sceneRef.value.traverse((obj) => {
    if (obj instanceof THREE.LineSegments) {
      obj.material.color.setHex(lineHex)
      obj.material.opacity = lineOp
      obj.material.needsUpdate = true
    }
  })
})
</script>

<template>
  <canvas ref="canvas" class="fixed inset-0 -z-10"></canvas>
</template>
