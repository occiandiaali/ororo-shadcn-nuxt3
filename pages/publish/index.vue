<script setup lang="ts">
const supabase = useSupabaseClient();

const title = ref('');
const description = ref('');
const price = ref('');
const category = ref('');
const video_url = ref('');
const poster_url = ref('');
//const views = ref('');
const duration = ref('');

let uploading = ref(false);
const isLoading = ref(false);
const formFeedback = ref(null)
const success = ref(false);

const replaceFileExtension = (str) => 
  str.replace(/\.[^/.]+$/, ".jpg")

const handleFileUpload = async (file) => {
    const cloudUrl = `https://api.cloudinary.com/v1_1/daaziue36/video/upload/`;
    const videoData = new FormData();
    videoData.append('file', file);
    videoData.append('folder', 'ororo-shadcn-nuxt');
    videoData.append('upload_preset', 'unnew-sveltekit-preset');

    try {
        uploading.value = true;
        const uploadResult = await fetch(cloudUrl, {
            method: 'POST',
            body: videoData
        });
        const result = await uploadResult.json();
        console.log('Upload: ', result)
        const posterfile = replaceFileExtension(result.secure_url);
        video_url.value = result.secure_url;
        poster_url.value = posterfile;
    } catch (error) {
        console.log('Error: ', error)
    } finally {
        uploading.value = false;
        videoData = null;
    }
}

const submitForm = async () => {
    isLoading.value = true;
    formFeedback.value = null;
if (
    !title.value.trim() || !price.value
    || !category.value.trim() || !video_url.value.trim()) {
        formFeedback.value = 'Form is incomplete..';
        isLoading.value = false;
        return;
    }
    const {error} = await supabase.from('nuxtshadcnposts')
    .insert({
        title: title.value,
        description: description.value,
        price: price.value,
        category: category.value,
        video: video_url.value,
        poster: poster_url.value,
        duration: duration.value
    })

    if (error) {
        console.log(error)
    }
    success.value = true;
    isLoading.value = false;
    formFeedback.value = 'Success!';
    title.value = '';
    description.value = '';
    price.value = '';
    category.value = '';
    video_url.value = '';
    poster_url.value = '';
    duration.value = '';
    setTimeout(() => {
        formFeedback.value = null;
    }, 2000)
  

}
</script>

<template>
<div>
   <div class="flex items-center justify-center h-screen bg-black">
           <div
        class="text-center text-white p-6 border-2 border-white w-full max-w-md"
      >
      <h1 class="text-2xl mb-3">Publish a new item</h1>
        <form @submit.prevent="submitEmail" class="flex flex-col">
          <label class="mb-4 text-left text-stone-400">
            Title
            <input
              placeholder="Name of the item.."
              v-model="title"
              type="text"
              class="bg-gray-800 text-white w-full px-3 py-2 mt-2"
            />
          </label>
         <label class="mb-4 text-left text-stone-400">
            Description
            <textarea
              maxlength="150"
              placeholder="Brief description of the item.."
              v-model="description"
              class="bg-gray-800 text-white w-full px-3 py-2 mt-2"
            />
          </label>
        <label class="mb-4 text-left text-stone-400">
            Price
            <input
              placeholder="Asking price.."
              v-model="price"
              type="number"
              class="bg-gray-800 text-white w-full px-3 py-2 mt-2"
            />
          </label>
        <label class="mb-4 text-left text-stone-400">
            Duration
            <input
              placeholder="List for how many days?"
              v-model="duration"
              type="number"
              class="bg-gray-800 text-white w-full px-3 py-2 mt-2"
            />
          </label>
          <label class="mb-4 text-left text-stone-400" for="category-select">Choose a category:

<select name="categories" id="category-select" v-model="category">
  <option value="" class="text-black">--Please select a category--</option>
  <option value="electronics">Electronics</option>
  <option value="automobiles">Automobiles</option>
  <option value="phones">Phones</option>
  <option value="computers">Computers</option>
  <option value="fashion">Fashion</option>
  <option value="household">Household</option>
  <option value="other">Other</option>
</select>
</label>

<label class="mb-4 text-left text-stone-400" for="video">Choose a video

<input class="mb-4 text-left" type="file" id="video" name="video" accept="video/*"
@change="(e) => handleFileUpload(e.target.files[0])"
 />
 <p class="text-white p-1">{{ uploading ? 'Uploading. Please, wait..' : null }}</p>
</label>
          <button 
          type="submit"
          class="bg-white text-black px-6 py-2 mt-4"
          @click.prevent="submitForm"
          >{{ isLoading ? 'Wait..' : 'Publish' }}</button>
          <p>{{ formFeedback }}</p>
          </form>
      </div>
   </div>
</div>
</template>

<style>
#category-select {
    font-size: 0.9rem;
    padding: 2px 5px;
}
</style>