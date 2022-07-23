<script setup>
    defineProps({
        title: "",
        color: "",
        desc:"",
        image:"",
        descColor:"",
        isShow: false,
    })
</script>


<template>
 <main :class="color" class="flex bg-sky-600 h-screen w-full justify-center px-6">
    <div class="flex items-center justify-center w-full h-[70vh]">
        <div class="flex flex-col">
            <h1 :class="text-color" class="text-white font-medium text-7xl">{{ title }}</h1>
            <p v-if="isShow" class="max-w-sm" :class="descColor" >
                {{ desc }}
            </p>
        </div>
        <img :src="image" class="h-3/5 m-10 rounded-2xl" alt="">   
    </div>
 </main>
</template>