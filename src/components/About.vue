<script setup>
import { useRouter } from 'vue-router';


import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged  } from "firebase/auth";
const provider = new GoogleAuthProvider();
const router = useRouter()


function inicio()
{
  router.push('/')
}
const auth = getAuth();
onAuthStateChanged(auth, (user) => {
  if (user) {
    // User is signed in, see docs for a list of available properties
    // https://firebase.google.com/docs/reference/js/auth.user
    const uid = user.displayName;
    console.log(uid)
    // ...
  } else {
    // User is signed out
    // ...
    console.log("no hay nadie logueado")
  }
});
function signinPopup() {
  
  

const auth = getAuth();
signInWithPopup(auth, provider)
  .then((result) => {
    // This gives you a Google Access Token. You can use it to access the Google API.
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    // The signed-in user info.
    const user = result.user;
    // IdP data available using getAdditionalUserInfo(result)
    // ...
  }).catch((error) => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // The email of the user's account used.
    const email = error.customData.email;
    // The AuthCredential type that was used.
    const credential = GoogleAuthProvider.credentialFromError(error);
    // ...
  });
}
</script>

<template>
  Componente ABOUT con id={{ $route.query }} y {{ $route.params.user }}
  <button @click="inicio">Volver a inicio</button>
  <button @click="signinPopup()">SignIn with Google</button>
</template>

<style scoped>

</style>
