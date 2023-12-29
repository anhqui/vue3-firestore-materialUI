<template>
    <nav class="green">
    <div class="nav-wrapper">
      <form>
        <div class="input-field">
          <input @change="onChange" id="search" type="search" v-model="search" required>
          <label class="label-icon" for="search"><i class="material-icons">search</i></label>
          <i class="material-icons" @click="clear">close</i>
        </div>
      </form>
    </div>
  </nav>
</template>


<script setup>
import {ref} from "vue"
import {defineProps} from 'vue'

const emit = defineEmits(['search-employees', 'clear-text'])
const search = ref("")

const props = defineProps({
    search:{
        type: String,
        required: true 
    }
})

const onChange = () =>{
    emit('search-employees')
}
const clear = () =>{
    emit('clear-text')
    
}
</script>

<style scoped>
nav{
    margin-top: 30px;
}
</style>