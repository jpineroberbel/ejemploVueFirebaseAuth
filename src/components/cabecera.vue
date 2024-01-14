<script setup>
import { getAuth, signOut } from "firebase/auth";
import { GoogleAuthProvider,signInWithPopup } from "firebase/auth";
import {ref} from 'vue'
import { useRouter } from 'vue-router'
const auth = getAuth();

let usuario = ref(auth.currentUser);
const router = useRouter();

function cerrarSesion()
{
    signOut(auth).then(() => {
        // Sign-out successful.
        usuario.value="";
         router.push({ path: '/' })
    }).catch((error) => {
        // An error happened.
    });

}

function iniciarSesion()
{
    const provider = new GoogleAuthProvider();
    signInWithPopup(auth, provider)
    .then((result) => {

        const user = result.user;
        usuario.value= user;
       router.push({ path: '/privado' })

    }).catch((error) => {
        
  });
 
}
</script>


<template>
    <div>
        CABECERA 
        <router-link to="/">Inicio</router-link>
        <router-link to="/privado">Mis Notas</router-link>
    
        <div v-if="usuario">
            Bienvenido {{ usuario.displayName }}
            <button v-on:click="cerrarSesion()">Cerrar Sesion</button>
        </div>
        <div v-else>
            <button v-on:click="iniciarSesion()">Iniciar Sesion</button>
        </div>
    </div>
</template>