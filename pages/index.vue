<template>
  <div class="h-screen p-12 bg-gray-800 text-white">
    <div class="md:grid grid-flow-col gap-12">
      <main class="order-2">
        <Calculator />
        
      </main>
      <aside class="order-1">
        <div class="max-w-md p-4 bg-gray-900 rounded-md shadow-md">
          <h3 class="text-green-300 mb-2">To Do</h3>
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
