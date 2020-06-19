<template>
  <div id="app">
    <header>
      <h1>
        小黄鸭&nbsp;&nbsp;
        <span class="brand">©</span>
        中文提示计算器
      </h1>
    </header>
    <section class="calculator">
      <calculator-content></calculator-content>
    </section>
  </div>
</template>

<script>
import CalculatorContent from "components/CalculatorContent.vue";
export default {
  name: "app",
  components: {
    CalculatorContent
  }
};
</script>

<style>
</style>
