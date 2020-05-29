<template>
  <div class="container">
    <form id="initialize-form">
      <h2>Welcome</h2>
      <label for="initialize-directory">Find the path to your video folder</label>
      <input type="file" id="initialize-directory" class="initialize-field" required webkitdirectory directory multiple>

      <input type="submit" class="initialize-submit" @click="saveUserInfo">
    </form>
  </div>
</template>

<script>
import { eventBus } from '../main.js';
const fs = require('fs');

export default {
  methods:{
    saveUserInfo: function(e){
      e.preventDefault();

      let directory = document.getElementById('initialize-directory').files[0].path;

      let userData = {'directory': directory};

      fs.writeFile("userinfo.json", JSON.stringify(userData), (err) => {
        if(err) {
          console.log('An error ocurred creating the file: ' + err.message)
        }
      })

      this.switchComponent('Home')
    },

    switchComponent(component) {
      eventBus.$emit('switchComponent', component);
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: calc(100vh - 5.25rem);
}

#initialize-form {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  width: 20rem;
  max-width: 95%;
  box-shadow: 0 0 6px rgba(0,0,0,.15);
  padding: 2rem;
  background-color: #fff;
  border-radius: 2px;
  z-index: 2;
}

h2 {
  font-size: 1.5rem;
  margin: 0 0 1.25rem 0;
  color: #ccc;
}

input:last-of-type {
  margin-top: 1.25rem;
}

label {
  font-size: .875rem;
  padding-bottom: .375rem;
}

.initialize-field, .initialize-submit {
  padding: .5rem;
  border-radius: 2px;
  border: 1px solid #ccc;
  opacity: .8;
  transition: opacity .2s ease-in-out;
}

.initialize-field:hover,
.initialize-field:focus,
.initialize-submit:hover,
.initialize-submit:focus {
  opacity: 1;
}

.initialize-submit {
  background: #456aaf;
  border: none;
  outline: none;
  color: #fff;
  width: min-content;
  cursor: pointer;
  text-transform: uppercase;
}
</style>
