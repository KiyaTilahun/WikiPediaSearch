<template>
    <div class="flex flex-col w-full">
        <div class="mx-auto">
            <img src="https://www.wikipedia.org/portal/wikipedia.org/assets/img/Wikipedia-logo-v2@2x.png" alt="wiki"
                class="max-w-[200px]">
        </div>
        <form class=" mx-auto  m-2 w-full md:w-1/2" v-on:submit.prevent="findWiki">
            <label for="default-search"
                class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
            <div class="relative">
                <div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
                    <svg class="w-4 h-4 text-gray-500 dark:text-gray-400" aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z" />
                    </svg>
                </div>
                <input v-model="searchName" type="search" id="default-search"
                    class="block w-full p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                    placeholder="Search Mockups, Logos..." />
                <button type="submit"
                    class="text-white absolute end-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Search</button>
            </div>
        </form>
        <div v-if="isLoading" class="spinner text-blue-800 z-99 relative">Loading ...</div>
    </div>

    <div v-for="item in searchresult" :key="item.id" v-if="searchresult.length">
        
    
    <div
        class="mb-2 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 w-full md:w-3/4 mx-auto">
       
        <div class="p-5">
            <a href="#">
                <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white" >{{ item.title }}</h5>
            </a>
            <p class="mb-3 font-normal text-gray-700 dark:text-gray-400" v-html="item.snippet"></p>
            <a :href="`https://en.wikipedia.org/?curid=${item.pageid}`" target="_blank"
                class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                Read more
                <svg class="rtl:rotate-180 w-3.5 h-3.5 ms-2" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                    fill="none" viewBox="0 0 14 10">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M1 5h12m0 0L9 1m4 4L9 9" />
                </svg>
            </a>
        </div>
    </div>
</div>


</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';
const searchName = ref('');
const searchresult = ref('');
const isLoading=ref(false);
const findWiki = async () => {
    let encodedQuery = encodeURIComponent(searchName.value)
    const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodedQuery}`;
    try {
        isLoading.value=true;
        const response = await axios.get(url);
       
        searchresult.value = response.data.query.search
 

    }
    catch (error) {

    }finally{
        isLoading.value=false;
    }
}
</script>
<style>

.spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  height: 10rem;
}

</style>