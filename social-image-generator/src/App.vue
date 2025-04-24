<script setup lang="ts">
import { ref, computed } from 'vue'
import defaultBg from './assets/bg.jpg'

const backgroundPosition = ref('right bottom')
const borderRadius = ref(15)
const picture = ref(defaultBg)
const text = ref('Vue.js Paris')
const subtitle = ref('')
const width = ref(640)

const containerStyle = computed(() => ({
  backgroundImage: `url(${picture.value})`,
  backgroundRepeat: 'no-repeat',
  borderRadius: `${borderRadius.value}px`,
  backgroundSize: 'cover',
  backgroundPosition: backgroundPosition.value,
}))

// const height = computed(() => width.value * (9 / 16))
const height = computed(() => width.value * 0.5)
const logoWidth = computed(() => width.value * 0.2)
const logoHeight = logoWidth
const fontSize = computed(() => 0.11 * width.value)
const titleSize = computed(() => fontSize.value + 'px')
const subtitleSize = computed(() => fontSize.value * (1 / 3) + 'px')

const canvasEl = ref<HTMLCanvasElement | null>(null)
const svgEl = ref<SVGSVGElement | null>(null)

// Should be doable by inlining the font Dosis (in index.html) in base64
// https://stackoverflow.com/questions/48105468/including-fonts-when-converting-svg-to-png
function _downloadImage() {
  const canvas = canvasEl.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return
  // @ts-expect-error: ok
  ctx.width = width.value
  // @ts-expect-error: ok
  ctx.height = height.value
  canvas.width = width.value
  canvas.height = height.value

  // saveSvgAsPng(svgEl.value, "diagram.png");
  // const svgData = new XMLSerializer().serializeToString(svgEl.value);
  // const svgBlob = new Blob([svgData], {
  //   type: "image/svg+xml;charset=utf-8",
  // });
  // const image = new Image();
  // image.onload = () => {
  //   ctx.clearRect(0, 0, ctx.width, ctx.height);
  //   ctx.drawImage(image, 0, 0, ctx.width, ctx.height);
  // };
  // const svgUrl = URL.createObjectURL(svgBlob);
  // image.src = svgUrl;
  // const linkEl = document.createElement("a");
  // linkEl.href = svgUrl;
  // linkEl.download = "image.svg";
  // linkEl.innerHTML = "Download SVG";
  // document.body.appendChild(linkEl);
}
</script>

<template>
  <div id="generated-background">
    <svg
      ref="svgEl"
      :style="containerStyle"
      :width="width"
      :height="height"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
    >
      <defs>
        <linearGradient id="bg-gradient" y2="1" x2="0">
          <stop stop-color="rgba(66, 184, 131, 0.9)" offset="0%" />
          <stop stop-color="rgba(53, 72, 94, 0.9)" offset="90%" />
        </linearGradient>
      </defs>

      <!-- <img :src="picture" width="100%" height="100%" /> -->

      <rect
        fill="url(#bg-gradient)"
        x="0"
        y="0"
        :rx="borderRadius"
        :ry="borderRadius"
        width="100%"
        height="100%"
      />

      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 196.32 170.02"
        :width="logoWidth"
        :height="logoHeight"
        :x="(width - logoWidth) / 2"
        :y="height / 2 - logoHeight * 0.9"
      >
        <title>Vue.js logo</title>
        <polygon
          fill="#fff"
          points="120.83 0 98.16 39.26 75.49 0 0 0 98.16 170.02 196.32 0 120.83 0"
        />
        <polygon
          fill="#35495e"
          points="120.83 0 98.16 39.26 75.49 0 39.26 0 98.16 102.01 157.06 0 120.83 0"
        />
      </svg>

      <text
        :x="width / 2"
        :y="(height + logoHeight) / 2"
        fill="white"
        text-anchor="middle"
        :style="{ fontFamily: 'Dosis', fontSize: titleSize }"
      >
        {{ text }}
      </text>
      <text
        :x="width / 2"
        :y="(height + logoHeight) / 2 + (fontSize * 2) / 3"
        fill="white"
        text-anchor="middle"
        :style="{ fontFamily: 'Dosis', fontSize: subtitleSize }"
      >
        {{ subtitle }}
      </text>
    </svg>
  </div>
  <canvas ref="canvasEl"></canvas>
</template>
