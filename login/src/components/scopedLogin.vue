<template>
    <div class="containerLogin">
        <h1>Login</h1>
        <div class="flex  mb-10 pl-5 h-32 mt-[30%] flex-col">
            <h2 class="mb-5 font-mono">E-mail  <input class="text-black w-72 bg-slate-300 outline-none rounded-sm" v-model="email" type="text"></h2>
            <h2 class="mb-5 font-mono flex items-start pl-2">Senha <input  v-model="passwordValue" class="text-black w-[264px] ml-2 bg-slate-300 outline-none rounded-l-sm" :type="visualitPassword"><button @click="visualitPasswordFunction()" class="bg-slate-300 rounded-r-sm"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="black" class="w-6 h-[22px]">
  <path d="M12 15a3 3 0 1 0 0-6 3 3 0 0 0 0 6Z" />
  <path fill-rule="evenodd" d="M1.323 11.447C2.811 6.976 7.028 3.75 12.001 3.75c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113-1.487 4.471-5.705 7.697-10.677 7.697-4.97 0-9.186-3.223-10.675-7.69a1.762 1.762 0 0 1 0-1.113ZM17.25 12a5.25 5.25 0 1 1-10.5 0 5.25 5.25 0 0 1 10.5 0Z" clip-rule="evenodd" />
</svg>
</button></h2>
            <h2 class="font-mono pb-1" v-if="register">Repetir Senha <input class="text-black w-[231px] bg-slate-300 outline-none rounded-sm" v-model="repPasswordValue" type="password"></h2>
            <label v-if="register && passwordsObjectForce.forcePassword" class="mt-2 w-fit text-green-500">*Senha forte</label>
            <label v-if="register && !passwordsObjectForce.forcePassword && passwordObject.password.length > 0" class="mt-1 w-fit text-red-500">*Senha fraca</label>
            <label v-if="register && !passwordsObject.sucessReptPassword &&  passwordObject.reptPassword.length > 6 " class="mt-1 w-fit text-red-500">*Senha incorreta</label>
            <ul v-if="register" class="text-white opacity-75">
                <li>A senha deve possuir</li>
                <li>*Mais de 6 caracteres</li>
                <li>*Letras maiusculas e minusculas</li>
                <li>*Números e simbolos(@#$%^&)</li>

            </ul>
        </div>
           <div class="h-16 mt-36">
               <button class="btnLogin" v-if="!register" @click="login()">Entrar</button>
               <button class="btnLogin !px-7" v-if="register" @click="singup()" :disabled="!passwordsObject.sucessReptPassword">Cadastrar</button>
               <button class="flex justify-start pl-2  text-white" v-if="!register" @click="openRegister()">Cadastrar-se</button>
               <button class="flex justify-start pl-2  text-white" v-if="register"  @click="openRegister()">Entrar</button>
           </div>
           <div>
            <label for=""></label>
           </div>
       
    </div>

</template>

<!-- @click="emit('openModalMessage', true)" -->

<script setup>
import { reactive, ref, watch } from 'vue';
import axios from 'axios'


const register = ref(false) 
const passwordValue = ref('')
const repPasswordValue = ref('')
const passwordObject = {'password': '', 'reptPassword': ''}
const passwordsObjectForce = {'forcePassword': ''}
const passwordsObject = {'sucessReptPassword': ''}
const email = ref('')
const visualitPassword = ref('password')
const token = ref('')
// const openModalMessage = ref(false);

const emit = defineEmits(['openModalMessage'])


const openRegister = () => {
    register.value = !register.value;
}

function visualitPasswordFunction(){
    if(visualitPassword.value === 'password'){
        visualitPassword.value = ''

    }else{
        visualitPassword.value = 'password'
    }


}


const tratamentPassword = (password) => {
try {
    let expressionAbcMinuscule = /[a-z]/;
    let expressionAbcMaiuscule = /[A-Z]/;
    let expressionNumber = /[0-9]/;
    let expressionSimbol = /[@#$%^&]/;

    let passExpressionAbcMinuscule = expressionAbcMinuscule.test(password.password);
    let passExpressionAbcMaiuscule = expressionAbcMaiuscule.test(password.password);
    let passExpressionNumber = expressionNumber.test(password.password);
    let passExpressionSimbol = expressionSimbol.test(password.password); 
    let compareSecurity = password.password.length > 6 && passExpressionAbcMinuscule && passExpressionAbcMaiuscule && passExpressionNumber && passExpressionSimbol;
    let comparePass = password.password == password.reptPassword;
   
    if(compareSecurity){
        passwordsObjectForce.forcePassword = true;
        if(comparePass){
            passwordsObject.sucessReptPassword = true;
            return passwordsObject;
        }if(!comparePass){
            passwordsObject.sucessReptPassword = false;
            return passwordsObject;
        }
    }if(!compareSecurity){
        passwordsObjectForce.forcePassword = false;
        passwordsObject.sucessReptPassword = false;
        return passwordsObjectForce;
    };
    }catch (error) {
    console.error(error)
    }

}


const singup = async() => {

    try {   
      
      const data = await axios({
            url:`http://localhost:8080/user/singup`,
            method:'POST',
            data: {
                email: email.value,
                senha: passwordValue.value

            }
        })
        if(data.status == 200){
            emit('openModalMessage', true)
        }else{
           console.log('errosss')
        }



        console.log(data)
   
    } catch (error) {
        console.error(error)
        alert('Usuario ja existe')
    }

}


const login = async() => {
    
    try {
        const data = await axios({
            url: `http://localhost:8080/user/login`,
            method: 'POST',
            data:{
                senha: passwordValue.value,
                email: email.value
            }
        })
        token.value = 'Bearer ' + data.data.token
        console.log(token.value)
        if(data.status == 200){
            alert('Logado com Sucesso')
        }else{
           console.log('errosss')
        }


    } catch (error) {

        console.error(error)
        alert('Email ou senha invalido')
    }



}


const sendRegister = () => {
    openModalMessage.value = true;


}


 
watch(passwordValue, () => {
    passwordObject.password = passwordValue.value;
    tratamentPassword(passwordObject);
});

watch(repPasswordValue, () => {
    passwordObject.reptPassword = repPasswordValue.value;
    tratamentPassword(passwordObject);
});








</script>
<style scoped>
h1{
    @apply font-semibold text-slate-400 text-4xl mb-2 flex justify-center pb-1
}
h2{
    @apply font-medium text-slate-200 
}
.btnLogin{

    @apply bg-gray-600 px-10 text-white ml-[35%] py-2 rounded-md hover:bg-cyan-950 hover:-translate-y-1 
}
.containerLogin {
    @apply h-[500px] w-[400px] border border-none rounded-lg bg-gray-900 bg-opacity-30 hover:bg-opacity-35
}


</style>