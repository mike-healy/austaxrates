<template>
  <div>
    <h1 class="text-2xl mb-8">2021/22 Australian Income Tax Rates</h1>
    <form>
      <label for="income" class="block">Income</label>
      <input v-model.number="income" type="number" name="income" id="income" step="1" placeholder="pre-tax income" class="bg-gray-200 text-gray-900 p-1 shadow:md">

      <h2 class="my-4 text-xl text-blue-300">${{ income.toLocaleString() }}</h2>

      <div class="py-4 text-xl">
        Tax <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ tax() }}</output>
      </div>
    </form>
  </div>
</template>

<script>
export default {
    /*
    0 – $18,200     Nil
    $18,201 – $45,000   19 cents for each $1 over $18,200
    $45,001 – $120,000  $5,092 plus 32.5 cents for each $1 over $45,000
    $120,001 – $180,000 $29,467 plus 37 cents for each $1 over $120,000
    $180,001 and over   $51,667 plus 45 cents for each $1 over $180,000

    Medicare: = i * 0.02
    Income -= Medicare 

    < $23,226 no medicare levy.
    It does reduce between that and 29033, but I don't know according to which formula.
    */

    data() {
      return {
        fnYearBrackets: [
          {top: 18200, prevTax: 0, rate: 0},
          {top: 45000, prevTax: 0, rate: 0.19},
          {top: 120_000, prevTax: 5092, rate: 0.325},
          {top: 180_000, prevTax: 29467, rate: 0.375},
          {top: null,  prevTax: 51667, rate: 0.45},
        ],
        
        income: 0,
      }
    },

    computed: {
      currentBracket: function() {
        
        let ourBracket = {}

        for (let i=0; i<this.fnYearBrackets.length; i++) {
          if (this.income <= this.fnYearBrackets[i].top || this.fnYearBrackets[i].top === null) {
            ourBracket = this.fnYearBrackets[i]
            break
          }
        }

        return ourBracket
      }
    },

    methods: {
      tax() {
        let tax = 0;
        return this.currentBracket.rate
      }
    }

}
</script>

<style>

</style>