<template>
  <div class="max-w-xl mx-auto bg-white p-8 rounded-lg shadow-lg">
    <h2 class="text-2xl font-bold text-gray-800 mb-6 text-center">Payment Checkout</h2>

    <div v-if="isPaid" class="text-center text-green-600 font-semibold text-lg py-4">
      ✅ Payment Successful! Thank you for your purchase.
    </div>

    <div v-else>
      <p class="text-gray-600 mb-6 text-center">
        Click the button below to complete your payment.
      </p>

      <button
        @click="handlePayment"
        :disabled="isLoading"
        class="w-full bg-gradient-to-r from-purple-600 to-indigo-600 text-white font-bold py-3 px-6 rounded-lg shadow-md hover:from-purple-700 hover:to-indigo-700 transition duration-300 ease-in-out disabled:opacity-50 disabled:cursor-not-allowed flex items-center justify-center"
      >
        <template v-if="isLoading">
          🔄 Processing Payment...
        </template>
        <template v-else>
          Pay Now
        </template>
      </button>
    </div>

    <div v-if="error" class="text-center text-red-600 mt-4">
      ❌ Error: {{ error }}
    </div>
  </div>
</template>
