<template>
 <section class="bg-gray-50 dark:bg-gray-900">
  <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0">
     
      <div class="flex items-center mb-6 text-2xl font-semibold text-gray-900 dark:text-white">
          <!-- <img class="w-8 h-8 mr-2" src="src/assets/Logo.png" alt="logo"> -->
          Integral cotton    
      </div>
      
      <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
          <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
              <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl dark:text-white">
                  Inicia sesión con tu cuenta
              </h1>
              <form autocomplete="off" @submit.prevent="iniciarSesion()" class="space-y-4 md:space-y-6" action="#">
                  <div>
                      <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Email</label>
                      <input v-model="email" type="email" name="email" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="nombre@email.com" required="">
                  </div>
                  <div>
                      <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Contraseña</label>
                      <input v-model="userPassword" type="password" name="password" id="password" placeholder="••••••••" class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required="">
                  </div>
                 
                  <button type="submit" class="w-full text-white bg-blue-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Iniciar sesión</button>
                  <p class="text-sm font-light text-gray-500 dark:text-gray-400">
                      ¿Aun no tienes una cuenta?  <router-link :to="{ name: 'Register' }" class="font-medium text-primary-600 hover:underline dark:text-primary-500">Regístrate</router-link>
                  </p>
                   <div class="flex items-center justify-between">
                      <div class="flex items-start">
                     
                       <!--   PARA RECORDAR AL USUARIO FLOWBITE
                          <div class="flex items-center h-5">
                            <input id="remember" aria-describedby="remember" type="checkbox" class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-primary-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-primary-600 dark:ring-offset-gray-800" required="">
                          </div>
                          <div class="ml-3 text-sm">
                            <label for="remember" class="text-gray-500 dark:text-gray-300">Recuérdame</label>
                          </div> 
                          -->
                        <a href="#" class="text-sm font-medium text-primary-600 hover:underline dark:text-primary-500">¿Olvidaste tu contraseña?</a>
                      </div>                      
                  </div>
              </form>
          </div>
      </div>
  </div>
</section>
</template>

<script>
import router from "@/router/index.js";
import axios from "axios"
import Swal from 'sweetalert2'

export default{
  
  data: () => ({
    email : '',
    userPassword: ''

  }), 
        
  methods:{
      iniciarSesion(){
                let jsonDatos = {
                email : this.email,
                userPassword: this.userPassword
            }

            let url = 'http://localhost:8000/api/User/login'
            let data = jsonDatos
            let config = {
              headers:{'Content-Type': 'application/json'},
              withCredentials: true
            }
            
            axios.post(url, data, config).then(res =>{
                //console.log(res);
                if(res.status === 200){
                    alert('Login exitoso')
                    router.push("Upload")
                }
            })
        },
        
        checkValidation(){
          if(!this.email){
            this.$refs.email.focus();
            Swal.fire('El campo email es requerido');
            return;
          }
           if(!this.userPassword){
            this.$refs.userPassword.focus();
            Swal.fire('El campo contrseña es requerido');
            return;
          }
          return true;
        }
  }
     
}   

</script>