<template>
    <div id="background">
      <section>
          <button @click="this.$parent.registering = false">Close</button>
          <h1>Register</h1>
          <div id="form">
            <div>
                <label for="email">Email:</label>
                <input v-model="email" type="text" name="email" id="email"/>
            </div>
            <div>
                <label for="username">Username:</label>
                <input v-model="username" type="text" name="username" id="username"/>
            </div>
            <div>
                <label for="password">Password:</label>
                <input v-model="password" type="text" name="password" id="password"/>
            </div>
            <div>
                <label for="repassword">Confirm Password:</label>
                <input v-model="repassword" type="text" name="repassword" id="repassword"/>
            </div>
            <button @click="handleRegister">Register</button>
        </div>
      </section>
    </div>
  </template>
  
  <script>
  export default {
      name: "RegisterPanel",
      data(){
          return{
            email: "",
            password: "",
            repassword: "",
            username: ""
          }
      },
      methods:{
        handleRegister(){
            if(this.password === this.repassword){
                let obj = {email:this.email,password:this.password,username:this.username}
                fetch( 'http://3.135.95.15:3001/register', {
                    method: 'POST',
                    mode: 'cors', // no-cors, *cors, same-origin
                    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                    credentials: 'same-origin', // include, *same-origin, omit
                    headers: {
                        'Content-Type': 'application/json'
                        // 'Content-Type': 'application/x-www-form-urlencoded',
                        },
                    body: JSON.stringify(obj)
                    })
                    this.$parent.registering = false
            }else{
                alert("The passwords don't match !")
            }
        }
    }
  }
  </script>
  
  <style lang="scss" scoped>
  
  #background{
      position: absolute;
      z-index: 2000;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background-color: #46494c79;
  }
  
  section{
      position: absolute;
      z-index: 2001;
      top: 30vh;
      left: 30vw;
      height: 40vh;
      width: 40vw;
      box-shadow: rgba(0, 0, 0, 0.5) 1vh 1vh;
      background-color: #46494C;
      color: white;
  }
  
  </style>