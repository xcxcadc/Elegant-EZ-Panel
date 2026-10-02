<template>
  <span class="traffic-depleted-mascot" aria-hidden="true">
    <svg viewBox="0 0 72 64" role="presentation">
      <path class="traffic-depleted-mascot__cloud" d="M18 43h35.5a10.5 10.5 0 0 0 1.2-20.93A15.5 15.5 0 0 0 25.1 19.6 11.5 11.5 0 0 0 18 43Z" />
      <path class="traffic-depleted-mascot__screen" d="M24 30.5h24a3.5 3.5 0 0 1 3.5 3.5v10a3.5 3.5 0 0 1-3.5 3.5H24a3.5 3.5 0 0 1-3.5-3.5V34a3.5 3.5 0 0 1 3.5-3.5Z" />
      <circle class="traffic-depleted-mascot__eye" cx="29" cy="39" r="1.65" />
      <circle class="traffic-depleted-mascot__eye" cx="43" cy="39" r="1.65" />
      <path class="traffic-depleted-mascot__smile" d="M33.5 42.5c1.5 1.8 4.5 1.8 6 0" />
      <path class="traffic-depleted-mascot__spark" d="M15 20v-5m-2.5 2.5h5M58 14v-4m-2 2h4" />
      <path class="traffic-depleted-mascot__drop" d="m58 35 3.5 4.5a4.5 4.5 0 1 1-7 0L58 35Z" />
    </svg>
  </span>
</template>

<script>
export default {
  name: 'TrafficDepletedMascot'
};
</script>

<style scoped>
.traffic-depleted-mascot {
  width: 42px;
  height: 42px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex: 0 0 auto;
  animation: traffic-mascot-float 2.8s ease-in-out infinite;
}

.traffic-depleted-mascot svg {
  width: 100%;
  height: 100%;
  overflow: visible;
}

.traffic-depleted-mascot__cloud {
  fill: rgba(var(--theme-color-rgb, 20, 153, 130), .12);
  stroke: var(--theme-color, #149982);
  stroke-width: 2.2;
  stroke-linejoin: round;
}

.traffic-depleted-mascot__screen {
  fill: var(--card-background, #fff);
  stroke: var(--theme-color, #149982);
  stroke-width: 2;
}

.traffic-depleted-mascot__eye {
  fill: var(--theme-color, #149982);
}

.traffic-depleted-mascot__smile,
.traffic-depleted-mascot__spark {
  fill: none;
  stroke: var(--theme-color, #149982);
  stroke-width: 1.8;
  stroke-linecap: round;
}

.traffic-depleted-mascot__drop {
  fill: #f4bd62;
  stroke: #d99634;
  stroke-width: 1.4;
}

@keyframes traffic-mascot-float {
  0%, 100% { transform: translateY(0) rotate(-1deg); }
  50% { transform: translateY(-2px) rotate(1deg); }
}

@media (prefers-reduced-motion: reduce) {
  .traffic-depleted-mascot { animation: none; }
}
</style>
