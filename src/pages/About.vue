<script setup>
import { ref } from "vue"
import Navbar from '../components/navbar.vue';
import Banner from '../components/banner.vue'
import Laptop2 from '../components/lp2.jpg';

const isDescShow = ref(false);
</script>

<template>
    <Navbar colNav="bg-white" ubahNav="" />
    <Banner color="bg-sky-500" title="Laptop jelek" :image="Laptop2" desc="Laptop murah rusak, dan tidak laku"
    descColor="text-white"  
    @click="isDescShow = !isDescShow" :is-show="isDescShow"/>
</template>