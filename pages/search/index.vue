<script setup lang="ts">
const supabase = useSupabaseClient();
const results = ref<any>([])
const location = useRouter();

let showSpinner = ref(false);

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
<div v-if="!results.length" class="face">
  <div class="eye"></div>
  <div class="eye"></div>
	<div class="lips"></div>
</div>

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

