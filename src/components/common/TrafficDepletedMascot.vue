<template>
  <span class="traffic-depleted-mascot" aria-hidden="true">
    <img src="/images/traffic-depleted-mascot.png" alt="" />
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

.traffic-depleted-mascot img {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: contain;
}

@keyframes traffic-mascot-float {
  0%, 100% { transform: translateY(0) rotate(-1deg); }
  50% { transform: translateY(-2px) rotate(1deg); }
}

@media (prefers-reduced-motion: reduce) {
  .traffic-depleted-mascot { animation: none; }
}
</style>
