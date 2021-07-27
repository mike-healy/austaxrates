<template>
  <div>
    <h1 class="text-3xl mb-4">2021/22 Australian Income Tax Calculator</h1>

    <form>
      <div class="p-6 pt-4 bg-gradient-to-tr from-green-600 to-green-400 text-black shadow-lg">
        <label for="income" class="block mb-2 text-xl">Income</label>
        <input v-model.number="income" type="number" name="income" id="income" step="1" min="0" placeholder="pre-tax income" class="bg-gray-200 text-gray-900 p-1 shadow:md">
      </div>

      <div class="mt-8 py-4 text-xl">
        <span class="inline-block w-36">Income</span>
        <output class="py-1 px-2 bg-yellow-200 text-gray-900">${{ income.toLocaleString() }}</output>
      </div>
      <div class="py-4 text-xl">
        <span class="inline-block w-36">Tax</span>
        <output class="py-1 px-2 bg-gray-900 text-gray-200">{{ round(tax()).toLocaleString() }}</output>
      </div>
      <div class="py-4 text-xl">
        <span class="inline-block w-36">Medicare Levy</span>
        <output class="py-1 px-2 bg-gray-900 text-gray-200">{{ round(medicare()).toLocaleString() }}</output>
        <small class="block text-sm">(estimate only)</small>

        <p
          v-show="income >= medicareData.from && income < medicareData.full"
          class="bg-gray-900 text-gray-300 mt-3 p-3 text-sm"
        >
          * Medicare levy is discounted for income under ${{ medicareData.full.toLocaleString() }}. This estimate does not account for that discount.
        </p>
      </div>

      <section class="p-6 py-2 border border-solid border-yellow-400">

        <div class="py-4 text-xl">
          <span class="inline-block text-yellow-200 w-36">Nett</span>
          <output class="py-2 px-4 bg-gradient-to-tr from-yellow-400 to-white text-gray-900">${{ round(nett()).toLocaleString() }}</output>
        </div>
        
        <div class="py-2">
          <p class="mb-2">
            <span class="inline-block text-right w-36 px-4 text-yellow-200">/ week</span>
            <output class="py-1 px-2 bg-gradient-to-tr from-yellow-300 to-yellow-200 text-gray-900">${{ round(nett()/52).toLocaleString() }}</output>
          </p>
          <p class="mb-2">
            <span class="inline-block text-right w-36 px-4 text-yellow-300">/ fortnight</span>
            <output class="py-1 px-2 bg-gradient-to-tr from-yellow-400 to-yellow-300 text-gray-900">${{ round(nett()/26).toLocaleString() }}</output>
          </p>
          <p class="mb-2">
            <span class="inline-block text-right w-36 px-4 text-yellow-400">/ month</span>
            <output class="py-1 px-2 bg-gradient-to-tr from-yellow-500 to-yellow-400 text-gray-900">${{ round(nett()/12).toLocaleString() }}</output>
          </p>
        </div>

      </section>

      <!-- Result rates -->
      <section class="p-4 mt-6 border border-solid border-gray-700 text-gray-300 shadow:md">
        <p class="mb-2"><span class="text-green-300">{{ round(( tax()+medicare() )/income * 100) }}% <span class="underline">overall</span></span> tax &amp; medicare levy combined rate.</p>
        <p>
          <span class="text-green-300">{{ currentIncomeBracket.rate*100 }}% top marginal rate</span>
          <span v-show="medicare() > 0">(excluding Medicare levy of {{ medicareData.rate*100 }}%)</span>.
        </p>
      </section>

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
