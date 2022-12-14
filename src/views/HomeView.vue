<template>
  <div class="home">
    <h1>home</h1>
    <h4>Non-Reactive:</h4>
    <p>my name is {{name1}} and my age is {{age1}}</p>
    <h4>Reactive:</h4>
    <p>my name is {{name2}} and my age is {{age2}}</p>
    <button @click="handleClick">Change Data</button>
    <button @click="age2++">Add 1 to Age</button>
    <input type="text" v-model="name2">
  </div>
</template>


<script>
import { ref } from 'vue'


export default {
  name: 'HomeView',
  // setup() im Lifecycle zuerst ausgeführt (vor created/mounted etc.)
  // Im Setup kann JS-Code ausgeführt werden. Dieser muss am Ende returnt, 
  // und kann anschließend verwendet werden. "this" funktioniert im Setup nicht 
  // Variablen im setup() (anders als in data()) nicht reactive, 
  // dies kann per ref() vollzogen werden (refs = reactive values; im Setup mit .value).
  setup() {

    let name1 = "marten"
    let age1 = 35
    
    const name2 = ref("marten2")
    const age2 = ref(38)

    const handleClick = () => {
      console.log("you clicked me")
      name1 = "luigi"
      name2.value = "wario"
      age2.value = 67
    }

    return { name1, age1, name2, age2, handleClick }
  }
}
</script>
