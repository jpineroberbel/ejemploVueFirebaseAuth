<script setup>
import { useCollection } from 'vuefire'
import { collection, addDoc, query, where} from 'firebase/firestore'
import { useFirestore, getCurrentUser } from 'vuefire'
import { getAuth, signInWithPopup } from "firebase/auth";


let usuario = getAuth().currentUser
const db = useFirestore()
let col = collection(db, 'todos');
  let que = query(col,where("usuario", "==", usuario.uid));
  console.log(que)
  const todos = useCollection(que);
 let miNuevaNota = "";


function nuevaNota()
{
  const docRef = addDoc(collection(db, "todos"), {
      text: miNuevaNota,
      prioridad: "Media",
      usuario: usuario.uid
  });
  miNuevaNota = "";
}

</script>

<template>
   <input type="text" name="" id="" v-model="miNuevaNota" v-on:keyup.enter="nuevaNota">
   <button v-on:click="nuevaNota">Nueva Nota</button>
   <p>Notas de {{ usuario.displayName }}</p>
    
  <ul>
    <li v-for="todo in todos" >
      <span>{{ todo.text }}</span>
    </li>
  </ul>
</template>