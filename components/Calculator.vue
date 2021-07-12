<template>
  <div>
    <h1 class="text-2xl mb-1">2021/22 Australian Income Tax Rates</h1>
    <p class="text-blue-200 italic mb-8">(Medicare levy not yet included)</p>

    <form>
      <label for="income" class="block">Income</label>
      <input v-model.number="income" type="number" name="income" id="income" step="1" min="0" placeholder="pre-tax income" class="bg-gray-200 text-gray-900 p-1 shadow:md">

      <h2 class="my-4 text-xl text-blue-300">${{ income.toLocaleString() }}</h2>

      <div class="py-4 text-xl">
        Tax <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ tax() }}</output>
      </div>
      <div class="py-4 text-xl">
        Medicare Levy Est. <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ medicare() }}</output>
        <p
          v-show="income >= medicareData.from && income < medicareData.full"
          class="text-yellow-400 mt-2 text-sm max-w-xs"
        >
          * Medicare levy is discounted for income under ${{ medicareData.full }}. This estimate does not account for that discount.
        </p>
      </div>
      <div class="py-4 text-xl">
        Nett <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ nett() }}</output>
      </div>
      <div class="py-4 text-xl">
        <p class="mb-1">Nett /week <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ nettWeek() }}</output></p>
        <p class="mb-1">Nett /month <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ round(nett()/12) }}</output></p>
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

    props: {
      finYear: {
        type: String,
        default: () => '2021-22'
      }
    },

    created: function() {
      this.fnYearBrackets = this.getBrackets(this.finYear).taxBrackets
      this.medicareData = this.getBrackets(this.finYear).medicare
    },

    data() {
      return {
        
        income: 52*1000,
      }
    },

    computed: {
      currentIncomeBracket: function() {
        for (let i=0; i<this.fnYearBrackets.length; i++) {
          if (this.income <= this.fnYearBrackets[i].to || this.fnYearBrackets[i].to === null) {
            return this.fnYearBrackets[i]
          }
        }
      }
    },

    methods: {
      round: a =>  Math.round(a*100)/100,

      tax() {
        const bracket = this.currentIncomeBracket
        return (this.income-bracket.from) * bracket.rate + bracket.accumulatedTax
      },

      medicare() {
        if (this.income <= this.medicareData.from) return 0
        return this.income * this.medicareData.rate
      },

      nett() {
        return this.income - this.tax() - this.medicare()
      },

      nettWeek() {
        return this.round(this.nett()/52)
      },

      getBrackets(financialYear) {
        switch(financialYear) {
          case '2021-22':
            return {
              medicare: {
                from: 23226, //levy starts to apply from this income, but at discounted rate
                full: 29033, //reaches full rate (below) from this income
                rate: 0.02
              },
              taxBrackets: [
                {from:      0, to:  18200, accumulatedTax:     0, rate:    0},
                {from:  18200, to:  45000, accumulatedTax:     0, rate: 0.19},
                {from:  45000, to: 120000, accumulatedTax:  5092, rate: 0.325},
                {from: 120000, to: 180000, accumulatedTax: 29467, rate: 0.375},
                {from: 180000, to:   null, accumulatedTax: 51667, rate: 0.45},
              ]
            }
          break;
        }
        
      },
    }

}
</script>

<style>

</style>