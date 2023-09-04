<template>
    <div id="calculator">
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
                        v-for="button in buttons.actions"
                        :key="button.value"
                    >
                        <BaseButton
                            color="gray"
                            :block="true"
                            @click="clickHandle(button.value)"
                        >
                            <BaseIcon v-if="button.icon">
                                <component :is="button.iconName"></component>
                            </BaseIcon>
                            <span v-else>{{ button.text }}</span>
                        </BaseButton>
                    </template>
                </div>
                <div class="calculator-buttons__numbers">
                    <template
                        v-for="button in buttons.numbers"
                        :key="button.value"
                    >
                        <BaseButton
                            :block="true"
                            @click="clickHandle(button.value)"
                        >
                            {{ button.text }}
                        </BaseButton>
                    </template>
                </div>
                <div class="calculator-buttons__special">
                    <template
                        v-for="button in buttons.special"
                        :key="button.value"
                    >
                        <BaseButton
                            :block="true"
                            @click="clickHandle(button.value)"
                        >
                            <BaseIcon v-if="button.icon">
                                <component :is="button.iconName"></component>
                            </BaseIcon>
                            <span v-else>{{ button.text }}</span>
                        </BaseButton>
                    </template>
                </div>
                <div class="calculator-buttons__operators">
                    <template
                        v-for="button in buttons.operators"
                        :key="button.value"
                    >
                        <BaseButton
                            color="blue"
                            :block="true"
                            @click="clickHandle(button.value)"
                        >
                            <BaseIcon v-if="button.icon">
                                <component :is="button.iconName"></component>
                            </BaseIcon>
                            <span v-else>{{ button.text }}</span>
                        </BaseButton>
                    </template>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import BaseButton from './BaseButton.vue';
import BaseIcon from './BaseIcon.vue';
import IconBackspace from './icons/iconBackspace.vue';
import IconDivide from './icons/IconDivide.vue';
import IconMinus from './icons/IconMinus.vue';
import IconMultiply from './icons/IconMultiply.vue';
import IconPlusMinus from './icons/IconPlusMinus.vue';
import IconPlus from './icons/IconPlus.vue';
import buttons from '../configs/ButtonsConfig';
export default {
    components: {
        BaseButton,
        BaseIcon,
        IconBackspace,
        IconDivide,
        IconMinus,
        IconMultiply,
        IconPlusMinus,
        IconPlus,
    },
    data() {
        return {
            buttons: buttons,
            expression: '',
            result: '',
            history: '',
        };
    },

    mounted() {},

    methods: {
        clickHandle(key) {
            const lastChar = this.expression.slice(-1);

            if (key === 'backspace') return this.removeLastCharacter();

            if (key === '=') return this.calculate();

            if (key === 'clear') return this.clearExpression();

            if (['+', '-', '%', '/', '*', '.', '+/-'].includes(key)) {
                if (!lastChar) return;
                if (['+', '-', '%', '/', '*', '.'].includes(lastChar)) {
                    this.expression = this.expression.slice(0, -1);
                }
                return (this.expression += key);
            }
            this.expression += key;
        },
        removeLastCharacter() {
            this.expression = this.expression.slice(0, -1);
        },
        clearExpression() {
            this.expression = '';
            this.result = '';
        },
        calculate() {
            this.history = this.expression;
            this.expression = eval(this.expression).toString();
        },
    },
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
