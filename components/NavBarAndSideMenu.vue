<script setup lang="ts">
import { InfoIcon, Grid3X3, MessageCircle, Menu, Package2, PenSquareIcon, Search } from 'lucide-vue-next'

import { Button } from '@/components/ui/button'

import { DropdownMenu, DropdownMenuContent, DropdownMenuItem, DropdownMenuLabel, DropdownMenuSeparator, DropdownMenuTrigger } from '@/components/ui/dropdown-menu'
import { Input } from '@/components/ui/input'
import { Sheet, SheetContent, SheetTrigger } from '@/components/ui/sheet'
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger
} from '@/components/ui/tooltip'

const router = useRouter();
//const term = ref('');
let searchTerm = '';

const goToPath =  () => {
  let timer;
  clearTimeout(timer)
  timer = setTimeout(async () => {
    await navigateTo({ path: 'search', query: {q: searchTerm}})
  }, 2500)
}
</script>

<template>
  <!-- <div class="flex min-h-screen w-full flex-col"> -->
    <header class="sticky z-50 top-0 flex h-16 items-center gap-4 border-b bg-slate-200/75 px-4 md:px-6">
      <nav class="hidden flex-col gap-6 text-lg font-medium md:flex md:flex-row md:items-center md:gap-5 md:text-sm lg:gap-6">
        <a
          href="#"
          class="flex items-center gap-2 text-lg font-semibold md:text-base"
        >
          <Package2 class="h-6 w-6" />
          <span class="sr-only">Ororo App</span>
        </a>
        <NuxtLink to="/"
          class="text-foreground transition-colors hover:text-foreground"
        >
          Home
        </NuxtLink>

                <NuxtLink to="/publish"
          class="text-muted-foreground transition-colors hover:text-foreground"
        >
          Publish
        </NuxtLink>
        <NuxtLink to="/chats"
          class="text-muted-foreground transition-colors hover:text-foreground"
        >
          Chats
        </NuxtLink>
        <NuxtLink to="/analytics"
          class="text-muted-foreground transition-colors hover:text-foreground"
        >
          Analytics
        </NuxtLink>
        <NuxtLink to="/faqs"
          class="text-muted-foreground transition-colors hover:text-foreground"
        >
          FAQs
        </NuxtLink>
      </nav>
      <Sheet>
        <SheetTrigger as-child>
          <Button
            variant="outline"
            size="icon"
            class="shrink-0 md:hidden"
          >
            <Menu class="h-5 w-5" />
            <span class="sr-only">Toggle navigation menu</span>
          </Button>
        </SheetTrigger>
        <SheetContent side="left">
          <nav class="grid gap-6 text-lg font-medium">
            <a
              href="#"
              class="flex items-center gap-2 text-lg font-semibold"
            >
              <Package2 class="h-6 w-6" />
              <span class="sr-only">Ororo</span>
            </a>
            <NuxtLink to="/" class="hover:text-foreground mt-6">
            <div class="flex flex-row">
             <div class="mt-0"><Grid3X3 /></div>
             <span class="pl-2">All</span>
            </div>
            </NuxtLink>

            <NuxtLink to="/chats"
              class="text-muted-foreground hover:text-foreground"
            >
            <div class="flex flex-row">
             <div class="mt-0"><MessageCircle/></div>
             <span class="pl-2">Chats</span>
            </div>
            </NuxtLink>
                    <NuxtLink to="/publish"
          class="text-muted-foreground transition-colors hover:text-foreground"
        >
           <div class="flex flex-row">
             <div class="mt-0"><PenSquareIcon/></div>
             <span class="pl-2">Publish</span>
            </div>
        </NuxtLink>
            <NuxtLink to="/faqs"
              class="text-muted-foreground hover:text-foreground"
            >
              FAQs
            </NuxtLink>
            <NuxtLink to="/analytics"
              class="text-muted-foreground hover:text-foreground"
            >
              Analytics
            </NuxtLink>

             <NuxtLink to="#"
              class="text-muted-foreground hover:text-foreground"
            >
              Log out
            </NuxtLink>
          </nav>
        </SheetContent>
      </Sheet>
      <div class="flex w-full items-center gap-4 md:ml-auto md:gap-2 lg:gap-4">
        <!-- <TooltipProvider>
    <Tooltip>
      <TooltipTrigger>Hover</TooltipTrigger>
      <TooltipContent>
        <p>Enter 'electronics', 'phones', etc. to search by category..</p>
        <p>Type an item word, or words, to search by item</p>
      </TooltipContent>
    </Tooltip>
  </TooltipProvider> -->
        <form class="ml-auto flex-1 sm:flex-initial">
          <div class="relative">
            <Search class="absolute left-2.5 top-2.5 h-4 w-4 text-muted-foreground" />
            <!-- <NuxtLink :to="{ path: 'search', query: {q: term}}">
            <Input
              type="search"
              v-model="searchTerm"
              placeholder="Search for something..."
              class="pl-8 sm:w-[300px] md:w-[200px] lg:w-[300px]"
            />
            </NuxtLink> -->
              <Input
              type="search"
              v-model="searchTerm"
              @keyup="goToPath"
              @blur="searchTerm = ''"
              placeholder="Search category or name"
              class="pl-8 sm:w-[300px] md:w-[200px] lg:w-[300px]"
            />
           <InfoIcon class="absolute right-3 top-3 h-4 w-4 cursor-pointer"/> 
          </div>
        </form>
        <DropdownMenu>
          <DropdownMenuTrigger as-child>
            <Button variant="secondary" size="icon" class="rounded-full">
              <!-- <CircleUser class="h-5 w-5" /> -->
                <Avatar>
    <AvatarImage src="https://img.daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg" alt="@username" />
    <AvatarFallback>CN</AvatarFallback>
  </Avatar>
              <span class="sr-only">Toggle user menu</span>
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuLabel>My Account</DropdownMenuLabel>
            <DropdownMenuSeparator />
            <DropdownMenuItem>
            <NuxtLink to="/publish">Create Post</NuxtLink>
            </DropdownMenuItem>
            <DropdownMenuItem>
            <NuxtLink to="/account">Account</NuxtLink>
            </DropdownMenuItem>
            <DropdownMenuItem>
            <NuxtLink to="/faqs">FAQs</NuxtLink>
            </DropdownMenuItem>
            <DropdownMenuSeparator />
            <DropdownMenuItem>Logout</DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      </div>
    </header>

  <!-- </div> -->
</template>