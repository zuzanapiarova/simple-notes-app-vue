<script setup>
  import { ref } from "vue";

  const showModal = ref(false);
  const newNote = ref("");
  const notes = ref([]);
  const errorMessage = ref("");

  //array of notes, when clicking on create note, a new note with this structure will be added to the notes array
  const checkErrorMessage = () => {
    if(newNote.value.length >= 1){
      console.log("there is text")
      errorMessage.value = "";
    }
  }
  const addNote = () => {
    console.log()
      if(newNote.value.trim().length < 1) {
        return errorMessage.value = "The note must have at least one character.";
      };
      notes.value.push({
        id: Math.floor(Math.random() * 1000000), 
        text: newNote.value,
        date: new Date(), 
        color: (`rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)})`)
      });
      showModal.value = false;
      newNote.value = "";
      errorMessage.value = "";
  };

</script>

<template>
  <main>
    <Transition>
      <div v-if="showModal" class="overlay">
        <div class="modal">
          <textarea @keyup="checkErrorMessage()" v-model="newNote" name="notes" id="notes" cols="30" rows="10" placeholder="Start typing a new note ..."></textarea>
          <div class="error__wrapper">
            <p class="error__message" v-if="errorMessage">{{ errorMessage }}</p>
          </div>  
          <button @click="addNote()">Add note</button>
          <button @click="showModal = false" class="close">Close</button>
        </div>
      </div>
    </Transition>
        <div class="container">
          <header>
            <h1>Notes</h1>
            <button @click="showModal = true">+</button>
          </header>
          <div class="cards__container">
              <div v-for="note in notes" class="card" :key="note.id" :style="{backgroundColor: note.color}">
                <p>{{ note.text }}</p>
                <p>{{ note.date.toLocaleDateString("en-DE") }}</p>
              </div>
          </div>
        </div>  
  </main>
</template>

<style scoped>

main{
  height: 100vh;
  width: 100vw;
}

.container{
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header {
  line-height: 1.5;
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  align-items: center;
}

h1{
  font-weight: 1;
  margin-bottom: 25px;
  font-size: 75px;
}

.error__wrapper{
  height: 10px;
}

header button{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;

}

#notes button{
  color: purple;
}

.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;

}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.modal{
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  transition: 500ms all ease;
}

.cards__container{
  display: flex;
  flex-direction: row;
}
.card{
  width: 225px;
  height: 225px;
  background-color: yellowgreen;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.error__message{
  font-size: 0.7rem;
  color: red;
}
.modal button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
  border-radius: 10px;
}

.modal .close{background-color: red;}

.date{
  font-size: 12.5px;
  font-weight: bold;
}

textarea{
  border-radius: 15px;
  border: 1px solid lightgray;
  padding: 15px;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
