<template>
    <scopedLogin @openModalMessage="openModalMessage = $event"/>
    <ModalCenterReports :modalActive="openModalMessage"  :message="'Cadastro concluído'"/>

</template>
<script setup>
import scopedLogin from "@/components/scopedLogin.vue"
import ModalCenterReports from "@/components/ModalCenterReports.vue";
import { ref } from "vue";


const openModalMessage = ref(false)



</script>
<style>
    
@media (min-width: 420px) {
  body {
    display: flex;
    place-items: center;
    background: rgb(51,70,191);
background: linear-gradient(90deg, rgba(51,70,191,0.3897934173669467) 0%, rgba(46,89,192,0.9108018207282913) 0%, rgba(46,137,192,0.9416141456582633) 11%, rgba(27,116,171,0.9051995798319328) 43%, rgba(27,71,171,0.9051995798319328) 89%);

}

}
</style>