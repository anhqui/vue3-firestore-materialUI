<template>
    <div id="view-employee">
       <ul class="collection with-header">
        <li class="collection-header"><h4>{{name}}</h4></li>
        <li class="collection-item">Employee ID#: {{employee_id}}</li>
        <li class="collection-item">Department: {{dept}}</li>
        <li class="collection-item">Position: {{position}}</li>
       </ul>
       <router-link to="/" class="btn grey">Back</router-link>
       <button @click="deleteEmployee" class="btn red">Delete</button>
          <div class="fixed-action-btn">
            <router-link :to="{name: 'edit-employee', params:{employee_id: employee_id}}" class="btn-floating btn-large red">
            <i class="fa fa-edit"></i></router-link>
        </div>
    </div>
</template>

<script setup>
import db from './firebaseInit'
import { collection, getDocs,  doc, deleteDoc, where, query } from "firebase/firestore"; 
import {useRoute, useRouter} from 'vue-router'
import {onMounted, ref} from 'vue'

const route = useRoute()
const router = useRouter()

const employee_id = ref(null)
const name = ref("")
const dept = ref("")
const position = ref("")
const segments = ref([]);

onMounted(async () =>{
    const q =  query(collection(db, "employees"), where('employee_id', '==', route.params.employee_id));
     const querySnapshot = await getDocs(q);
         querySnapshot.forEach((doc) => {
    // console.log(doc.ref._key.path.segments[doc.ref._key.path.segments.length -1])
    employee_id.value =  doc.data().employee_id
    name.value =  doc.data().name
    dept.value =  doc.data().dept
    position.value =  doc.data().position
    segments.value = doc.ref._key.path.segments;
    
});
    
})

const deleteEmployee = async () =>{
    if(confirm('Are you sure?')){
      deleteDoc(doc(db, "employees", segments.value[segments.value.length -1]));
      router.push('/')
    }
}

</script>

