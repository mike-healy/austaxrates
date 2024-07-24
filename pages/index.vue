<template>
  <div class="min-h-screen p-6 md:p-12 bg-gray-800 text-white">
    <div class="max-w-7xl mx-auto md:grid grid-cols-3 gap-12 lg:gap-24">
      <main class="col-span-2">
        <Calculator />
      </main>

      <aside class="pt-12">
        <div class="max-w-md p-4 bg-gray-900 rounded-sm shadow-md">
          <h2 class="text-green-300 mb-2 text-xl">Please Note</h2>
          <ul class="text-gray-300">
            <li class="py-3 border-b border-solid border-gray-700">Calculation based on simple income tax rates from ATO</li>
            <li class="py-3 border-b border-solid border-gray-700">Does not account for offsets, deductions, family tax benefits or other complications</li>
            <li class="py-3">Medicare levy is estimated at full rate when income is over the threshold. Low income earners may get a discount, reducing the levy</li>
          </ul>
        </div>
      </aside>
    </div> <!-- /grid -->
  </div>
</template>

<script>
export default {

}
</script>
