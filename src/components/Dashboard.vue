<template>
    <div id="dashboard">
       <ul class="collection with-header">
        <li class="collection-header"><h4>Employees</h4></li>
        <li v-for="employee in employees" :key="employee.id" class="collection-item">
            <div class="chip">
            {{employee.dept}}
            </div> 
            {{employee.employee_id}}:{{employee.name}}  
            <router-link :to="{name: 'view-employee', params:{employee_id: employee.employee_id}}" class="right"><i class="fa fa-eye"></i></router-link>
            </li>
       </ul>
        <div class="fixed-action-btn">
            <router-link to="/new" class="btn-floating btn-large red">
            <i class="fa fa-plus"></i></router-link>
        </div>

    </div>
</template>

<script setup>
import {onMounted, ref} from 'vue'
import db from './firebaseInit'
import { collection, getDocs, orderBy, query } from "firebase/firestore"; 

const employees = ref([])
// const employ = ref([])

onMounted(async ()=>{
    // const querySnapshot = await getDocs(collection(db, "employees"));
    const q = query(collection(db, "employees"), orderBy('dept'));
    const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
//   console.log(doc.data());
const data = {
    'id': doc.id,
    'employee_id': doc.data().employee_id,
    'name': doc.data().name,
    'dept': doc.data().dept,
    'position': doc.data().position
}
employees.value.push(data)


});

// employ.value.push(employees.value[0])
})


</script>

