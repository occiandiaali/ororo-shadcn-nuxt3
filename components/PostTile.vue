<script setup lang="ts">
defineProps<{
    poster: string,
    title: string,
    price: string,
    category: string,
}>()
</script>

<template>
<div class="grid grid-rows-3 grid-flow-col gap-2 border-red-600">
<div class="row-span-2">
<img :src="poster" :alt="title" :style="{'width': '180px','height': '150px'}" />
</div>
<div class="row-span-2 col-span-3">
<h2 class="text-black font-bold text-xl">{{ title }}</h2>
<p class="text-slate-600 text-xs">{{ 
new Intl.NumberFormat('en-NG', {
          style: 'currency',
          currency: 'NGN'
        }).format(parseFloat(price))
}}</p>
<span class="text-slate-400">In: {{ category }}</span>
</div>
</div>
</template>

<style></style>