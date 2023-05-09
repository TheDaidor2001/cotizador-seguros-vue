<script setup>
import { ref, computed, watch } from 'vue';
import Header from './components/Header.vue';
import Button from './components/Button.vue'
import { calcularTotalPagar } from './helpers'


const cantidad = ref(0);
const meses = ref(6)
const total = ref(0)


watch([cantidad, meses], () => {
  total.value = calcularTotalPagar(cantidad.value, meses.value)
})

//Constantes
const MIN = 0;
const MAX = 20000;
const STEP = 100;

const formatearDinero = (numero) => {
  const dinero = new Intl.NumberFormat('es-ES', {
    style: "currency",
    currency: "EUR"
  })

  return dinero.format(numero);
}


const handleClickDecrementar = () => {
  if (cantidad.value <= MIN) {
    alert('Cantidad no válida');
    return;
  }

  cantidad.value = cantidad.value - STEP;
}

const handleClickIncrementar = () => {
  if (cantidad.value >= MAX) {
    alert('Cantidad no válida');
    return;
  }

  cantidad.value = cantidad.value + STEP;
}

const calcularMensual = computed(() => {
  return total.value / meses.value
})



</script>

<template>
  <div class="bg-white p-10 shadow max-w-lg my-20 mx-auto">
    <Header />

    <div class="flex justify-between my-6 ">
      <Button type="-" @fn="handleClickDecrementar" />
      <Button type="+" @fn="handleClickIncrementar" />
    </div>

    <input type="range" class="w-full mt-6 accent-lime-500 hover:accent-lime-600" :min="MIN" :max="MAX" :step="STEP"
      v-model.number="cantidad">
    <p class="text-center text-5xl text-indigo-600 font-extrabold my-10">{{ formatearDinero(cantidad) }}</p>

    <h2 class="text-center text-gray-500 text-2xl font-extrabold">
      Elige un <span class="text-indigo-600 underline">Plazo</span> a pagar.
    </h2>

    <select
      class="w-full b-white my-5 border-2 border-gray-300 p-2 rounded-lg text-center text-xl font-bold text-gray-500"
      v-model.number="meses">
      <option value="6">6 Meses</option>
      <option value="12">12 Meses</option>
      <option value="24">24 Meses</option>
    </select>

    <div v-if="total > 0" class="bg-gray-100 p-5 mb-5 space-y-4">
      <h2 class="text-center text-gray-500 text-2xl font-extrabold">
        Resumen <span class="text-indigo-600">de Pagos</span>
      </h2>
      <p class="text-center text-xl font-bold text-gray-500">{{ meses }} Meses</p>
      <p class="text-center text-xl font-bold text-gray-500">Total a pagar: <span class="text-indigo-600">{{ formatearDinero(total) }}</span></p>
      <p class="text-center text-xl font-bold text-gray-500">Mensualidad: <span class="text-indigo-600">{{ formatearDinero(calcularMensual) }}</span></p>
    </div>

    <div v-else>
      <h2 class="text-center text-3xl font-extrabold mt-5 text-indigo-600 capitalize">Averigua el costo de tu préstamo</h2>
    </div>
  </div>
</template>
