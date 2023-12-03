<template>
  <div>
    <section>
      <div class="calculator-history">
        <span class="calculator-history__text">{{ history }}</span>
      </div>
      <div class="calculator-expression">
        <div class="calculator-expression-container">
          <span class="calculator-expression__text">
            {{ expression || 0 }}
          </span>
        </div>
      </div>
      <div class="calculator-buttons">
        <div class="calculator-buttons__actions">
          <template
            v-for="button in ButtonConfig.actions"
            :key="button.value"
          >
            <BaseButton
              class="text-center"
              color="gray"
              :block="true"
              @click="clickHandle(button.value)"
            >
              <template v-if="button.icon">
                <div class="flex justify-center w-full">
                  <img
                    class="brightness-0"
                    :src="`../../assets/icons/${button.icon}`"
                  />
                </div>
              </template>
              <template v-else>
                <span class="text-[32px]">{{ button.label }}</span>
              </template>
            </BaseButton>
          </template>
        </div>
        <div class="calculator-buttons__numbers">
          <template
            v-for="button in ButtonConfig.numbers"
            :key="button.value"
          >
            <BaseButton
              :block="true"
              @click="clickHandle(button.value)"
            >
              <template v-if="button.icon">
                <div class="flex justify-center w-full">
                  <img
                    class="brightness-0"
                    :src="`../../assets/icons/${button.icon}`"
                  />
                </div>
              </template>
              <template v-else>
                <span class="text-[32px]">{{ button.label }}</span>
              </template>
            </BaseButton>
          </template>
        </div>
        <div class="calculator-buttons__operators">
          <template
            v-for="button in ButtonConfig.operators"
            :key="button.value"
          >
            <BaseButton
              color="blue"
              :block="true"
              @click="clickHandle(button.value)"
            >
              <span class="text-[32px]">{{ button.label }}</span>
            </BaseButton>
          </template>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import BaseButton from '../components/BaseButton.vue';
import ButtonConfig from '../components/BaseButtonConfig';

const expression = ref(null);
const result = ref(null);
const history = ref(null);

const clickHandle = (key) => {
  const lastChar = expression.value || ''.slice(-1);

  if (key === 'backspace') return removeLastCharacter();

  if (key === '=') return calculate();

  if (key === 'clear') return clearExpression();

  if (['+', '-', '%', '/', '*', '.', '+/-'].includes(key)) {
    if (!lastChar) return;
    if (['+', '-', '%', '/', '*', '.'].includes(lastChar)) {
      expression.value = expression.value.slice(0, -1);
    }
    return (expression.value += key);
  }
  expression.value += key;
};

const removeLastCharacter = () => {
  expression.value = expression.value.slice(0, -1);
};
const clearExpression = () => {
  expression.value = '';
  result.value = '';
};
const calculate = () => {
  history.value = expression.value;
  expression.value = eval(expression.value).toString();
};
</script>

<style lang="scss" scoped>
#calculator {
  display: flex;
  flex-direction: column;
  justify-content: end;
  min-height: 100vh;
}
.calculator {
  &-history {
    margin-right: 20px;
    text-align: right;
    &__text {
      color: black;
    }
  }
  &-expression {
    display: table;
    overflow: hidden;
    font-size: 30px;
    height: 32px;
    vertical-align: bottom;
    position: relative;
    table-layout: fixed;
    width: calc(100% - 40px);
    margin: 0 20px;
    z-index: 0;
    &-container {
      display: table-row;
      direction: ltr;
    }
    &__text {
      color: black;
      float: right;
      font-family: arial, sans-serif;
      white-space: nowrap;
    }
  }
  &-buttons {
    display: grid;
    padding: 20px;
    padding-bottom: 4.8rem;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
    gap: 10px;
    grid-auto-flow: row;
    grid-template-areas:
      'actions actions actions operators'
      'numbers numbers numbers operators'
      'numbers numbers numbers operators'
      'numbers numbers numbers operators'
      'special special special operators';
    &__actions {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-template-rows: 1fr;
      gap: 10px;
      grid-auto-flow: row;
      grid-template-areas: '. . .';
      grid-area: actions;
      align-items: center;
    }
    &__numbers {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-template-rows: 1fr 1fr 1fr;
      gap: 10px;
      grid-auto-flow: row;
      grid-template-areas:
        '. . .'
        '. . .'
        '. . .';
      grid-area: numbers;
      align-items: center;
    }
    &__special {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      grid-template-rows: 1fr;
      gap: 10px;
      grid-auto-flow: row;
      grid-template-areas: '. . .';
      grid-area: special;
      align-items: center;
    }
    &__operators {
      display: grid;
      grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
      grid-template-columns: 1fr;
      gap: 10px;
      grid-auto-flow: row;
      grid-template-areas:
        '.'
        '.'
        '.'
        '.'
        '.';
      grid-area: operators;
      align-items: center;
    }
  }
}
</style>
