<template>
    <div id="background">
      <section>
          <button @click="this.$parent.logging = false">Close</button>
          <h1>Login</h1>
          <div id="form">
            <div>
                <label for="email">Email:</label>
                <input v-model="email" type="text" name="email" id="email"/>
            </div>
            <div>
                <label for="password">Password:</label>
                <input v-model="password" ref="password" type="text" name="password" id="password"/>
            </div>
            <button @click="handleLogin">Log In</button>
        </div>
      </section>
    </div>
</template>
  
  <script>
  export default {
    name: "LoginPanel",
    data(){
        return{
            email: "",
            password: ""
        }
    },
    methods:{
        handleLogin(){
            let obj = {email:this.email,password:this.password}
            fetch( '/api/login', {
                method: 'POST',
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(obj)
            })
            //To be edited later when auth is working
            this.$parent.isConnected = true
            this.$parent.logging = false
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