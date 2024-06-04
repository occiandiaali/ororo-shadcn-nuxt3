<script setup lang="ts">
import { Search } from 'lucide-vue-next'
import { Input } from '@/components/ui/input'

const categories = [
    {
        id: 1,
        label: 'electronics',
        selected: true
    },
        {
        id: 2,
        label: 'kitchen',
        selected: false
    },
        {
        id: 3,
        label: 'perishables',
        selected: false
    },
        {
        id: 4,
        label: 'automobiles',
        selected: false
    },
        {
        id: 5,
        label: 'computers',
        selected: false
    },
        {
        id: 6,
        label: 'phones',
        selected: false
    },
        {
        id: 7,
        label: 'fashion',
        selected: false
    },
        {
        id: 8,
        label: 'household',
        selected: false
    },
        {
        id: 9,
        label: 'other',
        selected: false
    },
];

const location = useRouter();

</script>

<template>
 <!-- <div class="relative w-full max-w-sm items-center">
    <Input id="search" type="text" placeholder="Search..." class="pl-10" />
    <span class="absolute start-0 inset-y-0 flex items-center justify-center px-2">
      <Search class="size-6 text-muted-foreground" />
    </span>
  </div> -->
  <div>
  <!-- <h3>{{ location }}</h3> -->
  <pre>Search for: {{ location.currentRoute.value.query.q }}</pre>
  </div>
<div class="columns-2 md:columns-3">
<div v-for="category in categories" :key="category.id"
class="w-60 h-12 rounded-2xl bg-slate-200 flex justify-center pt-3 my-2"
>
{{ category.label }}
</div>
</div>
</template>