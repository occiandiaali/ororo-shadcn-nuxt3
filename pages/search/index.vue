<script setup lang="ts">
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

const supabase = useSupabaseClient();
const results = ref<any>([])
const location = useRouter();

const logPath = async (v:any) => {
    const {data: filterPosts, error} = await supabase
    .from('nuxtshadcnposts')
    .select()
    .eq('category', v);
    if (error) {
        console.log(error)
    }
    console.log('Filtered: ', filterPosts)
   // results.value = filterPosts
   // return results.value;
    return filterPosts;
}

let searchTerm = ref<any>(null);

watch(() => location.currentRoute.value.query, async () => {
    //let searchTerm = ref<any>(location.currentRoute.value.query.q);
    searchTerm.value = location.currentRoute.value.query.q
    const {data: filterPosts, error} = await supabase
    .from('nuxtshadcnposts')
    .select()
    //.textSearch('title', searchTerm.value)  // single item search
    .or(`category.eq.${searchTerm.value}, title.eq.${searchTerm.value}`);
    if (error) {
        console.log(error)
    }
    console.log('Term: ', searchTerm.value);
    console.log('Filtered: ', filterPosts);
    results.value = filterPosts;
    searchTerm.value = '';
})


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
  <!-- <pre>Search for: {{ location.currentRoute.value.query.q }}</pre> -->
 
  </div>
<!-- <div class="columns-2 md:columns-3">
<div v-for="category in categories" :key="category.id"
class="w-60 h-12 rounded-2xl bg-slate-200 flex justify-center pt-3 my-2"
>
{{ category.label }}
</div>
</div> -->

<!-- <div> Searched: <pre>{{ location.currentRoute.value.query.q }}</pre></div> -->
<div v-if="!results.length" class="face">
  <div class="eye"></div>
  <div class="eye"></div>
	<div class="lips"></div>
</div>
<!-- <div><pre>{{ logPath(location.currentRoute.value.query.q) }}</pre></div> -->
<!-- <div v-else><pre>{{ JSON.stringify(results, null, 2) }}</pre></div> -->
<div v-else>
<div v-for="result in results" :key="result.id">
<post-tile 
:title="result.title"
:poster="result.poster"
:price="result.price"
:category="result.category"
/>
</div>
</div>
</template>

<style>
* {
    box-sizing: border-box;
}

.face {
    display: flex;
    justify-content: space-between;
    align-self: center;
    margin-top: 15%;
    width: 150px;
    height: 200px;
    padding: 30px;
	position: relative;
    background: tomato;
    transition: .3s ease-in-out;
}

.face:hover {
    background: green;
}
.lips {
	width: 100px; 
	height: 10px; position: absolute; bottom: 35%;
	background: white; border-radius: 50%; border-top:3px;left:25px
}

.eye {
    background: white;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    transform-origin: 50%;
    -webkit-transform-origin: 50%;
    animation: blink 4s infinite;
    -webkit-animation: blink 4s infinite;
}

@keyframes blink {
    0%, 100% {
        transform: scale(1, .05);
    }
    5%,
    95% {
        transform: scale(1, 1);
    }
}

@-webkit-keyframes blink {
    0%, 100% {
        transform: scale(1, .05);
    }
    5%,
    95% {
        transform: scale(1, 1);
    }
}
</style>

