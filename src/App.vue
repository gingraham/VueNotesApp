<script setup>
import {ref} from 'vue';
const showModal = ref(false);
const newNote = ref('')
const errorMessage = ref('')
const notes = ref([])
//date formatter
const date = new Date();
const options = { weekday: 'short', year: 'numeric', month: 'short', day: 'numeric' };
const dateTimeFormatter = new Intl.DateTimeFormat('en-US', options);
const formattedDateParts = dateTimeFormatter.formatToParts(date);
const day = formattedDateParts[0].value
const month = formattedDateParts[2].value
const dd = formattedDateParts[4].value
const year = formattedDateParts[6].value
console.log(formattedDateParts)
const formattedTime = date.toLocaleTimeString([], { hour: "2-digit", minute: "2-digit" });
//Random color Generator
function getRandomColor() {
  let color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}
//function to add a new Note
const addNote = ()=>{
  if(newNote.value.length < 10){
    errorMessage.value = "Please enter more than 10 characters!"
    return
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    text: newNote.value,
    date: `${day} | ${month}, ${dd} ${year} | ${formattedTime}`,
    backgroundColor: getRandomColor()
  })
  showModal.value =false;
  newNote.value = ''
}
</script>


<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage" class="errorMsg">{{errorMessage}}</p>
        <button @click="addNote" type="button">Add Note</button>
        <button type="button" class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true" type="button">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes"
         :key="note.id" class="card" 
         :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{note.date}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width: 100%;
}
.container{
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}
header button{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(255, 255, 255);
  border-radius: 100%;
  color: black;
}
.card{
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 0 20px 20px 0;
}
.main-text{
  color: black;
}
.date{
  font-weight: bold;
  font-size: 12px;
  color: rgb(51, 49, 49);
}
.cards-container{
  display: flex;
  flex-wrap: wrap;
}
.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .7);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal{
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: violet;
  color: white;
  margin-top:15px;
  border-radius: 5px;
  border: none;
}
.modal .close{
  background-color: red;
  margin-top:7px;
}
.errorMsg{
  color: red;
  margin-top: 10px;
}
</style>