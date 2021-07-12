<template>
  <div class="h-screen p-12 bg-gray-800 text-white">
    <div class="md:grid grid-flow-col gap-12">
      <main class="order-2">
        <Calculator />
        
      </main>
      <aside class="order-1">
        <div class="max-w-md p-4 bg-gray-900 rounded-md shadow-md">
          <h2 class="text-green-300 mb-2 text-xl">Please Note:</h2>
          <ul class="pl-8 text-gray-300">
            <li class="my-4 border-b border-solid border-gray-700">Calculation based on simple income tax rates from ATO</li>
            <li class="my-4 border-b border-solid border-gray-700">Does not account for offsets, deductions, family tax benefits or other complications</li>
            <li class="my-4 border-b border-solid border-gray-700">Medicare levy is estimated at full rate when income is over the threshold. Low income earners may get a discount, reducing the levy</li>
          </ul>

          <h3 class="mt-12 text-green-300 mb-2">To Do</h3>
          <ul>
            <li class="mb-2">
              Select tax year
            </li>
            <li class="mb-2">
              Multiple calculator instances to compare with
            </li>
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
