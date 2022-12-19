<template>
  <div class="home">
    <h1>home</h1>
    <h4>No-Refs/Non-Reactive:</h4>
    <p>my name is {{name1}} and my age is {{age1}}</p>

    <h4>Refs:</h4>
    <p>my name is {{name2}} and my age is {{age2}}</p>
    <button @click="handleClick">Change Data</button>
    <button @click="age2++">Add 1 to Age</button>
    <input type="text" v-model="name2">
    <br><br><br>
    
    <h4>Ref VS Reactive</h4>
    <h5>Ref:</h5>
    <p>my name is {{ninjaOne.name}} and my age is {{ninjaOne.age}}</p>
    <button @click="updateNinjaOne">Update ninja one</button>
    <h5>Reactive:</h5>
    <p>my name is {{ninjaTwo.name}} and my age is {{ninjaTwo.age}}</p>
    <button @click="updateNinjaTwo">Update ninja two</button>
    <br><br><br>

    <h4>Computed Values</h4>
    <input type="text" v-model="search">
    <p> your search term is: {{ search }}</p>
    <div v-for="name in matchingNames" :key="name">{{ name }}</div>
    <button @click="handleStop" >stop watching</button>

  </div>
</template>


<script>
import { ref, reactive, computed, watch, watchEffect } from 'vue'


export default {
  name: 'HomeView',
  // setup() wird im Lifecycle zuerst ausgeführt (vor created/mounted etc.)
  // Im Setup kann JS-Code ausgeführt werden. Dieser muss am Ende returnt, 
  // und kann anschließend verwendet werden. "this" funktioniert im Setup nicht 
  // Variablen im setup() (anders als in data()) nicht dynamisch/reactive, 
  // dies kann per ref() vollzogen werden (refs = reactive values; im Setup mit .value).
  // oder per reactive() (kein .value notwendig)
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
    
    //Ref VS Reactive
    //Ref: .value notwendig, kann deshalb aber "primitive types" lesen (s.o. name2/age2); 
    //      funktioniert besser mit externen Compositions Funktionen, da diese mit Ref ihre
    //      Reaktivität behalten, wenn sie extern ausgeführt werden
    //Reactive: keine "primitive types" möglich (da ohne .value), liest nur Objects (/Arrays?)

    const ninjaOne = ref({name: "Mario", age: 42})
    const ninjaTwo = reactive({name: "Mario2", age: 33})

    const updateNinjaOne = () => {
      ninjaOne.value.age = 67
    }

    const updateNinjaTwo = () => {
      ninjaTwo.age = 67
    }


    //Computed Values

    const names = ref(["mario", "yoshi", "luigi", "toad", "bowser", "koopa", "peach"])
    const search = ref("")
    // leerer String von search wird durch v-model befüllt

    const matchingNames = computed(() => {
      return names.value.filter((name) => name.includes(search.value))
    })


    //watch & watchEffect
    //watch: Überwachte Value muss explizit, außerhalb der Funktion, angegeben werden, triggert nur, wenn sich diese ändert.
    //  = lazy, specific state, callback-funct: previous and current state
    //watchEffect: Wird beim mount getriggert und überwacht alle values/dependecies die innerhalb der Funktion angegeben werden.
    const stopWatch = watch(search, () => {
      console.log("watch function ran")
    })

    const stopEffect = watchEffect(() => {
      console.log("watchEffect function ran", search.value)
    })

    const handleStop = () => {
      stopWatch()
      stopEffect()
    }


    return { name1, age1, name2, age2, handleClick, 
      ninjaOne, ninjaTwo, updateNinjaOne, updateNinjaTwo, 
      names, search, matchingNames, stopWatch, stopEffect, handleStop }
  }
}
</script>
