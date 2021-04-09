<template>
  <div id="app">
    <div class="app-inner">
      <template v-for="(num, i) in nums">
        <div class="fraction-block" :key="i + '_nums'">
          <input
            type="number"
            v-model.number.trim="num.numerator"
            v-mask="[/[1-9]/, /[1-9]/]"
          />
          <hr />
          <input
            type="number"
            v-model.number.trim="num.denominator"
            v-mask="[/[1-9]/, /[1-9]/]"
          />
          <button class="delete" v-if="i > 1" @click="deleteElem(i)">
            &times;
          </button>
        </div>
        <div class="sign" :key="i + '_sign'" v-if="i + 1 !== nums.length">+</div>
      </template>
      <div class="sign">=</div>
      <div class="result">{{ sum }}</div>
    </div>
    <button
      class="add-elem"
      @click="addElem"
      :disabled="nums.length >= maxElems"
    >
      Add new element
    </button>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    nums: [
      { numerator: 1, denominator: 1 },
      { numerator: 1, denominator: 1 },
    ],
    maxElems: 5,
  }),
  computed: {
    sum() {
      return this.nums.reduce((acc, currVal) => {
        return acc + currVal.numerator / currVal.denominator;
      }, 0);
    },
  },
  methods: {
    addElem() {
      if (this.nums.length >= this.maxElems) return;
      this.nums.push({ numerator: 1, denominator: 1 });
    },
    deleteElem(i) {
      this.nums.splice(i, 1);
    },
  },
};
</script>

<style>
.app-inner {
  display: flex;
  align-items: center;
  font-size: 24px;
  font-family: Arial, Helvetica, sans-serif;
}

.fraction-block {
  display: flex;
  flex-direction: column;
  width: 60px;
  position: relative;
}

.fraction-block .delete {
  position: absolute;
  left: 0;
  width: 100%;
  bottom: -25px;
}

.fraction-block input {
  padding: 10px;
}

.fraction-block hr {
  border-top: 1px solid #000;
  border-bottom: 1px solid #000;
  width: 100%;
}

.sign {
  font-size: 40px;
  margin: 0 10px;
}

.add-elem {
  margin-top: 30px;
}
</style>
