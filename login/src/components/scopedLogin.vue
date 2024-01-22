<template>
    <div class="containerLogin">
        <h1>Login</h1>

        <div class="flex pl-5 mt-[40%] flex-col">
            <h2 class="mb-10">E-mail  <input class="text-black w-72 bg-slate-300 outline-none rounded-sm" type="text"></h2>
            <h2>Senha <input class="text-black w-72 bg-slate-300 outline-none rounded-sm" type="password"></h2>
        </div>
           <button class="btnLogin">Entrar</button>
           <button class="flex justify-start pl-2 text-white">Cadastrar-se</button>
       
    </div>


</template>
<script setup>

</script>
<style scoped>
h1{
    @apply font-semibold text-slate-400 text-4xl mb-2 flex justify-center pb-1

}
h2{
    @apply font-medium text-slate-200 

}

.btnLogin{

    @apply mt-32 bg-gray-600 px-10 text-white ml-[35%] py-2 rounded-md hover:bg-blue-800 hover:-translate-y-1 

}
.containerLogin {

    @apply h-[500px] w-[400px] border border-none rounded-lg bg-gray-700 bg-opacity-30 hover:bg-opacity-35


}


</style>