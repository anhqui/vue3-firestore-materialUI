<template>
    <div id="edit-employee">
        <h3>Edit Employee</h3>
        <div class="row">
            <form @submit.prevent="updateEmployee" class="col s12">
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" disabled v-model="employee_id" placeholder="Employee ID#" required>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" v-model="name" placeholder="Name" required>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" v-model="dept" placeholder="Department" required>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" v-model="position" placeholder="Position" required>
                    </div>
                </div>
                <button type="submit" class="btn">Submit</button>
                <router-link to="/" class="btn grey">Cancel</router-link>

            </form>
        </div>
    </div>
</template>

<script setup>
import db from "./firebaseInit"
import {useRoute, useRouter} from 'vue-router'
import { collection, updateDoc, getDocs, doc, query, where } from "firebase/firestore"; 
import {ref, onMounted} from "vue"

const route = useRoute()
const router = useRouter()

const employee_id = ref(null)
const name = ref("")
const dept = ref("")
const position = ref("")
const segments = ref([]);

onMounted(async () => {
     const q =  query(collection(db, "employees"), where('employee_id', '==', route.params.employee_id));
     const querySnapshot = await getDocs(q);
    querySnapshot.forEach((doc) => {
    employee_id.value =  doc.data().employee_id
    name.value =  doc.data().name
    dept.value =  doc.data().dept
    position.value =  doc.data().position
    segments.value = doc.ref._key.path.segments;
});
    
})

const updateEmployee = () =>{
    const docRef = doc(db, "employees", segments.value[segments.value.length -1]);
    updateDoc(docRef, {
    employee_id: employee_id.value,
    name: name.value,
    dept: dept.value,
    position: position.value
  }) 
       router.push({name: 'view-employee', params: {employee_id: employee_id.value}})
}

</script>

