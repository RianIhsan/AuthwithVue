<script setup>
import { ref } from "vue"
import Navbar from '../components/navbar.vue';
import Banner from '../components/banner.vue';
import Laptop from '../components/lp.jpg';
import modal from '../components/Modal.vue'

const isDescShow = ref(false);
const isModalShow = ref(false)
</script>

<template>
    <Navbar @open-modal="isModalShow = true" colNav="bg-white" ubahNav="true"/>
    <banner title="LAPTOP MURAH" color="bg-sky-700"
    desc="Laptop mahal untuk pasar orang kaya"
    :image="Laptop" descColor="text-white"
    @click="isDescShow = !isDescShow" :is-show="isDescShow"
    
    />
    
    
</template>