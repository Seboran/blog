<template>
  <div class="shake bouton text-sm text-gray-500 dark:text-white leading-5">
    <a class="hover:text-gray-700 dark:hover:text-gray-200" href="/contact"
      >Me contacter →</a
    >
  </div>
</template>

<style scoped>
.appear-enter-active,
.appear-leave-active {
  transition: opacity 0.5s ease;
  transition-delay: 0.225s;
}

.appear-enter-from,
.appear-leave-to {
  opacity: 0;
}

.shake:hover {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-2px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(4px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-8px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(8px, 0, 0);
  }
}
</style>
