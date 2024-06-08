<script setup>
import { Skeleton } from '@/components/ui/skeleton';
import { Button } from '@/components/ui/button'

import {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerFooter,
  DrawerHeader,
  DrawerTitle,
  DrawerTrigger,
} from '@/components/ui/drawer'
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import {
  Dialog,
  DialogClose,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'


const supabase = useSupabaseClient()
const posts = ref([])

const generateRandomImgHeight = (max, min) => Math.trunc(Math.random()*(max-min)+min)

const fetchAllPosts = async () => {
    const {data: allPosts, error} = await supabase
    .from('nuxtshadcnposts')
    .select()

    if (error) {
        console.log(error)
    }

    posts.value = allPosts
}
onMounted(() => {
    try {
        fetchAllPosts()
    } catch (error) {
        console.log('Catch error: ', error)
    }
})
</script>

<template>
<!-- <div v-for="post in posts" :key="post.id">
<pre>{{ JSON.stringify(post, null, 2) }}</pre>
</div> -->
  <div class="columns-2 md:columns-3 lg:columns-4 pt-4">
  <div v-if="!posts.length">
  <div v-for="n in 10" :key="n" >
      <Skeleton class="h-[125px] w-[250px] rounded-xl" />
    <div class="space-y-2">
      <Skeleton class="h-4 w-[250px]" />
      <Skeleton class="h-4 w-[200px]" />
    </div>
  </div>
  </div>
  <div v-else v-for="post in posts" :key="post.id" class="relative mb-4 before:content-[''] before:rounded-md before:absolute before:inset-0 ">
    <img class="w-full rounded-md" :src="`${post.poster}`"
    :style="{
        'width': '100%',
        'height': `${generateRandomImgHeight(250,190)}px`,
        'margin': '2px'
    }"
    >
    <div class="test__body absolute inset-0 p-8 text-white flex flex-col">
    <span class="absolute top-3 left-5 text-xs">
    <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 0 24 24" width="24px" fill="#5f6368"><path d="M0 0h24v24H0V0z" fill="none"/><path d="M12 6c3.79 0 7.17 2.13 8.82 5.5C19.17 14.87 15.79 17 12 17s-7.17-2.13-8.82-5.5C4.83 8.13 8.21 6 12 6m0-2C7 4 2.73 7.11 1 11.5 2.73 15.89 7 19 12 19s9.27-3.11 11-7.5C21.27 7.11 17 4 12 4zm0 5c1.38 0 2.5 1.12 2.5 2.5S13.38 14 12 14s-2.5-1.12-2.5-2.5S10.62 9 12 9m0-2c-2.48 0-4.5 2.02-4.5 4.5S9.52 16 12 16s4.5-2.02 4.5-4.5S14.48 7 12 7z"/></svg>
    3.5k
    </span>
    <span class="absolute top-5 right-2 text-xs">{{post.duration}} days left</span>

      <Drawer>
      <DrawerTrigger as-child>
      <div class="play-icon-div absolute top-20 left-16 md:left-32 md:cursor-pointer">
      <svg xmlns="http://www.w3.org/2000/svg" enable-background="new 0 0 24 24" height="32px" viewBox="0 0 24 24" width="32px" fill="#e5a7a7"><g><rect fill="none" height="32" width="32"/></g><g><path d="M12,2C6.48,2,2,6.48,2,12s4.48,10,10,10s10-4.48,10-10S17.52,2,12,2z M12,20c-4.41,0-8-3.59-8-8s3.59-8,8-8s8,3.59,8,8 S16.41,20,12,20z M9.5,16.5l7-4.5l-7-4.5V16.5z"/></g></svg>
      </div>
      </DrawerTrigger>
      <DrawerContent>
      <div class="mx-auto w-full max-w-sm p-1">
      <DrawerHeader>
      <div class="flex flex-row p-1">
      <Avatar style="margin-right: 12px;">
    <AvatarImage src="https://img.daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg" alt="@username" />
    <AvatarFallback>CN</AvatarFallback>
  </Avatar>
      <div>
         <DrawerTitle>{{ post.title }} - 
         <span class="text-sm">{{ 
                 new Intl.NumberFormat('en-NG', {
          style: 'currency',
          currency: 'NGN'
        }).format(parseInt(post.price))
          }}</span>
          </DrawerTitle>
          <DrawerDescription>{{ post.description }}</DrawerDescription>
      </div>
      </div>
      </DrawerHeader>
      <div class="w-full h-80 p-1">
      <!-- <h3>Video Player</h3> -->
      <!-- <img :src="`${post.poster}`" :alt="`${post.title}`"
      width="100%" height="300px"
      /> -->
      <video :src="`${post.video}`" controls 
      width="100%" height="300px" autoplay
      />
      </div>
        <DrawerFooter>
          <Dialog>
          <DialogTrigger as-child>
          <Button class="z-30">Contact seller</Button>
          </DialogTrigger>
          <DialogContent class="sm:max-w-md">
          <DialogHeader>
          <DialogTitle>Send the seller a message</DialogTitle>
          </DialogHeader>
               <div class="flex items-center space-x-2">
        <div class="grid flex-1 gap-2">
          <Label for="link" class="sr-only">
            Link
          </Label>
          <Input
            id="link"
            default-value="Short message to the seller.."
            read-only
          />
        </div>
        </div>
            <DialogFooter class="sm:justify-start">
        <DialogClose as-child>
          <Button type="button" variant="secondary">
            Close
          </Button>
        </DialogClose>
            <DialogTrigger as-child>
            <Button class="z-30">Send</Button>
            </DialogTrigger>
      </DialogFooter>
          </DialogContent>
          </Dialog>    
          
          <DrawerClose as-child>
            <Button variant="outline" class="z-30">
              Close
            </Button>
          </DrawerClose>
        </DrawerFooter>
      </div>
      </DrawerContent>
      </Drawer>
      <!-- <div class="mt-2">
        <span class="test__tag bg-white bg-opacity-60 py-1 px-4 rounded-md text-black">#{{ post.price }}</span>
      </div> -->
    </div>
     <div>
        <h1 class="test__title text-sm font-bold mb-1">{{ post.title }}</h1>
        <p class="test__author font-sm text-xs font-light mb-1">{{ 
        new Intl.NumberFormat('en-NG', {style: 'currency',currency: 'NGN'})
          .format(post.price)
         }}</p>
         <p class="font-sm text-xs font-extralight">In: {{post.category}}</p>
      </div>
  </div>
  </div>
</template>