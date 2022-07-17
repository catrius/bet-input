<template>
  <form>
    <div class="amount-group group">
      <div class="grid amount-group-top">
        <AmountAdjustment class="min" value='MIN' v-model="amount" :adjustTo="MIN_AMOUNT" :disabled="placed"/>
        <div class="amount-input grid">
          <span class="yen-sign">¥</span>
          <input
            class="amount number-input"
            type="text"
            v-model="amount"
            :disabled="placed"
            @blur="handleAmountBlur"
          >
        </div>
        <AmountAdjustment
          class="half" value='1/2' v-model="amount" :adjustTo="Number(amount) / 2" :disabled="placed"
        />
        <AmountAdjustment
          class="max" value='MAX' v-model="amount" :adjustTo="MAX_AMOUNT" :disabled="placed"
        />
        <AmountAdjustment
          class="double" value='2X' v-model="amount" :adjustTo="Number(amount) * 2" :disabled="placed"
        />
      </div>
      <div class="grid amount-group-bottom">
        <AmountAdjustment
          value='+5' v-model="amount" :adjustTo="Number(amount) + 5" :isChip="true" :disabled="placed"
        />
        <AmountAdjustment
          value='+10' v-model="amount" :adjustTo="Number(amount) + 10" :isChip="true" :disabled="placed"
        />
        <AmountAdjustment
          value='+50' v-model="amount" :adjustTo="Number(amount) + 50" :isChip="true" :disabled="placed"
        />
        <AmountAdjustment
          value='+100' v-model="amount" :adjustTo="Number(amount) + 100" :isChip="true" :disabled="placed"
        />
        <AmountAdjustment
          value='+500' v-model="amount" :adjustTo="Number(amount) + 500" :isChip="true" :disabled="placed"
        />
        <AmountAdjustment
          class="amount-1k"
          value='+1k'
          v-model="amount"
          :adjustTo="Number(amount) + 1000"
          :isChip="true"
          :disabled="placed"
        />
      </div>
    </div>
    <div class="defuse-at-group group">
      <div class="grid defuse-at-group-top">
        <div class="defuse-at-text">DEFUSE AT</div>
        <DefuseAtAdjustment
          value='-'
          v-model="defuseAt"
          :adjustTo="defuseAt - 0.1"
          :disabled="placed"
        />
        <input
          class="number-input"
          type="text"
          v-model="defuseAt"
          :disabled="placed"
          @blur="handleDefuseAtBlur"
        >
        <DefuseAtAdjustment
          value='+'
          v-model="defuseAt"
          :adjustTo="Number(defuseAt) >= 2 ? Number(defuseAt) + 1 : Number(defuseAt) + 0.1"
          :disabled="placed"
        />
      </div>
      <div class="grid defuse-at-group-bottom">
        <input type="button" value='1.5x' @click="defuseAt = 1.5" :disabled="placed">
        <input type="button" value='2.0x' @click="defuseAt = 2" :disabled="placed">
        <input type="button" value='5.0x' @click="defuseAt = 5" :disabled="placed">
      </div>
    </div>
    <input id="bet-button" type="button" :value="placed ? 'BET PLACED' : 'BET'" @click="placed = true">
  </form>
</template>

<script>
import AmountAdjustment from '@/components/AmountAdjustment.vue';
import DefuseAtAdjustment from '@/components/DefuseAtAdjustment.vue';
import {
  MIN_AMOUNT, MAX_AMOUNT, MIN_DEFUSE_AT, MAX_DEFUSE_AT,
} from '@/constants';

export default {
  name: 'BetInput',
  components: {
    AmountAdjustment,
    DefuseAtAdjustment,
  },
  data() {
    return {
      MIN_AMOUNT,
      MAX_AMOUNT,
      MIN_DEFUSE_AT,
      MAX_DEFUSE_AT,
      amount: 10,
      defuseAt: 2,
      placed: false,
    };
  },
  computed: {},
  watch: {
    amount(newValue, oldValue) {
      if (!/^\d+$/.test(newValue) && newValue !== '') {
        this.amount = oldValue;
      }
    },
    defuseAt(newValue, oldValue) {
      if (!/^\d+\.?\d{0,2}$/.test(newValue) && newValue !== '') {
        this.defuseAt = oldValue;
      }
    },
  },
  methods: {
    handleAmountBlur() {
      if (this.amount > MAX_AMOUNT) {
        this.amount = MAX_AMOUNT;
      } else if (this.amount < MIN_AMOUNT) {
        this.amount = MIN_AMOUNT;
      }
    },
    handleDefuseAtBlur() {
      if (this.defuseAt > MAX_DEFUSE_AT) {
        this.defuseAt = MAX_DEFUSE_AT;
      } else if (this.defuseAt < MIN_DEFUSE_AT) {
        this.defuseAt = MIN_DEFUSE_AT;
      }
    },
  },
};
</script>

<style scoped lang="scss">

$border-color: #344053;
$text-color-primary: white;
$text-color-secondary: #5BBBCC;
$form-background-color: #0A141E;
$group-background-color: #1D2234;
$button-background-color: #23344E;
$bet-button-background-color: linear-gradient(90deg, #2d83d8, #34bfde);
$button-hover-border-color: #3C6595;
$defuse-at-color: #6A778C;

form {
  background-color: $form-background-color;
  padding: 10px;
  display: inline-grid;
  column-gap: 8px;

  input[type="button"] {
    background-color: $button-background-color;
    color: $text-color-primary;
    border: 1px solid $button-background-color;
    border-radius: 5px;

    &:disabled {
      background: $group-background-color;
      color: #2E4062 !important;
    }
  }

  .number-input {
    background: transparent;
    color: $text-color-primary;
    border: none;
    text-align: center;
    font-size: 20px;
    font-weight: bold;

    &:focus-visible {
      outline: none;
    }
  }

  .group {
    border: 1px solid $border-color;
    background-color: $group-background-color;
    border-radius: 10px;
  }

  .grid {
    display: grid;
    column-gap: 3px;
    row-gap: 3px;
  }

  .hidden {
    visibility: hidden;
  }
}

.amount-group {
  .amount-group-top {
    grid-template-columns: repeat(5, 50px);
    grid-template-rows: repeat(2, 30px);
    padding: 10px 10px 8px 10px;
    border-bottom: 1px solid $border-color;

    input[type="button"] {
      &:hover {
        border-color: $button-hover-border-color;
      }
    }

    .amount-input {
      grid-column: 2 / span 3;
      grid-row: 1 / span 2;
      grid-template-columns: 30% 40%;

      .yen-sign {
        font-size: 20px;
        font-weight: bold;
        color: #B2CDE7;
        align-self: center;
      }
    }
  }

  .amount-group-bottom {
    grid-template-columns: repeat(5, 50px);
    grid-template-rows: repeat(1, 30px);
    padding: 8px 10px 10px 10px;

    input[type="button"] {
      color: $text-color-secondary;
      font-family: monospace;

      &:hover {
        background: $bet-button-background-color;
        color: $text-color-primary;
      }
    }
  }
}

.defuse-at-group {
  .defuse-at-group-top {
    grid-template-columns: 30px 90px 30px;
    grid-template-rows: repeat(2, 30px);
    padding: 10px 10px 8px 10px;
    border-bottom: 1px solid $border-color;

    .defuse-at-text {
      color: $defuse-at-color;
    }

    input[type="button"] {
      &:hover {
        border-color: $button-hover-border-color;
      }
    }
  }

  .defuse-at-group-bottom {
    grid-template-columns: repeat(3, 50px);
    grid-template-rows: repeat(1, 30px);
    padding: 8px 10px 10px 10px;

    input[type="button"] {
      color: $text-color-secondary;
      font-family: monospace;

      &:hover {
        background: $bet-button-background-color;
        color: $text-color-primary;
      }
    }
  }
}

#bet-button {
  background: $bet-button-background-color;
  border: 1px solid $bet-button-background-color;
  border-radius: 10px !important;
  font-weight: bold;
  font-size: 20px;

  &:hover {
    border-color: $button-hover-border-color;
  }
}

@media (min-width: 768px) {
  form {
    grid-template-columns: fit-content(40%) fit-content(40%) 180px;
  }

  .amount-group {
    .amount-group-bottom {
        .amount-1k {
          display: none;
        }
      }
    }
  .defuse-at-group {
    .defuse-at-group-top {
      .defuse-at-text {
        grid-column: 1 / span 3;
      }
    }
  }
}

@media (max-width: 767px) {
  form {
    grid-template-rows: auto auto 100px;
    row-gap: 10px;
  }

  .amount-group {
    .amount-group-top {
      grid-template-columns: repeat(6, 50px);

      .amount-input {
        grid-column: 2 / span 4;
        grid-row: 1 / span 2;
      }
    }

    .amount-group-bottom {
      grid-template-columns: repeat(6, 50px);
    }
  }

  .defuse-at-group {
    .defuse-at-group-top {
      grid-template-columns: 156px 30px 89px 30px;
      grid-template-rows: 30px;
      border-bottom: none;

      .defuse-at-text {
        align-self: center;
      }
    }

    .defuse-at-group-bottom {
      display: none;
    }
  }
}

</style>
