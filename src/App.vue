<template>
    <main>
      <div class="attribution">
        Challenge by <a href="https://www.frontendmentor.io/profile/ValdiANS" target="_blank">Frontend Mentor</a>.
        Coded by <a href="https://github.com/ValdiANS" target="_blank">Valdi</a>.
      </div>

      <h1>
        <span>Spli</span>
        <span>tter</span>
      </h1>

      <tip-calculator-app />
    </main>
</template>

<script>
import { onMounted } from 'vue';
import TipCalculatorApp from './components/TipCalculatorApp.vue';

export default {
  name: 'App',
  components: {
    TipCalculatorApp,
  },
  setup() {
    onMounted(() => {
      document.title = 'Tip Calculator App';
    });
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap');

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  background-color: hsl(185, 41%, 84%);
  font-family: 'Space Mono', 'Poppins', Arial;
}

main {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4em 0;
  padding: 40px 0 0 0;

  .tip-calculator-container {
    animation-name: moveInBottom;
    animation-duration: 1s;
    animation-timing-function: ease;
  }
}

h1 {
  text-transform: uppercase;
  font-weight: 700;
  font-size: 24px;
  color: hsl(186, 14%, 43%);
  letter-spacing: 0.2em;

  display: flex;
  flex-direction: column;

  width: fit-content;
  margin: 0 auto;
}

.attribution {
  text-align: center;
}

@media screen and (min-width: 430px) {
  main {
    padding: 40px;
  }
}

@keyframes moveInBottom {
  0% {
    transform: translateY(5rem);
    opacity: 0;
  }

  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
