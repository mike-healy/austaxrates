<template>
  <div class="min-h-screen p-12 bg-gray-800 text-white">
    <div class="md:grid grid-flow-col gap-24">
      <main>
        <Calculator />
      </main>

      <aside class="pt-12">
        <div class="max-w-md p-4 bg-gray-900 rounded-md shadow-md">
          <h2 class="text-green-300 mb-2 text-xl">Please Note:</h2>
          <ul class="text-gray-300">
            <li class="py-2 my-2 border-b border-solid border-gray-700">Calculation based on simple income tax rates from ATO</li>
            <li class="py-2 my-2 border-b border-solid border-gray-700">Does not account for offsets, deductions, family tax benefits or other complications</li>
            <li class="py-2 my-2 border-b border-solid border-gray-700">Medicare levy is estimated at full rate when income is over the threshold. Low income earners may get a discount, reducing the levy</li>
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
