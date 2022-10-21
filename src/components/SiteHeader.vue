<template>
  <header>
    <div>
        <h1>WORD PANIC</h1>
    </div>
    <div class="dropdown" v-if="isConnected">
        <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            {{userName}}
        </button>
        <ul class="dropdown-menu">
            <li><button class="dropdown-item" @click="handleDisconnexion()">Deconnexion</button></li>
        </ul>
    </div>


    <div class="dropdown" v-if="!isConnected">
        <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            Connexion
        </button>
        <ul class="dropdown-menu">
            <button class="btn btn-primary" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasLogin" aria-controls="offcanvasLogin">Login</button>
            <button @click="registering = true" class="btn btn-primary" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasRegister" aria-controls="offcanvasRegister">Register</button>
        </ul>
    </div>


    <!-- LOGIN OFFCANVAS // HIDDEN BY DEFAULT -->
    <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasLogin" aria-labelledby="offcanvasRightLabel">
        <div class="offcanvas-header">
            <h5 class="offcanvas-title" id="offcanvasRightLabel">Login</h5>
            <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
        </div>
        <div class="offcanvas-body">
            <div v-if="!isConnected">
                <div class="mb-3">
                <label for="exampleFormControlInput1" class="form-label">Email address</label>
                <input v-model="lg_email" type="email" class="form-control" id="loginEmail" placeholder="name@example.com">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlInput1" class="form-label">Password</label>
                    <input v-model="lg_password" type="password" class="form-control" id="loginPassword" placeholder="your password">
                </div>
                <button type="submit" class="btn btn-primary" @click="handleLogin">Login</button>
            </div>
            <div v-if="isConnected">
                <p>You are now connected.</p>
            </div>
            
        </div>
    </div>

    <!-- REGISTER OFFCANVAS // HIDDEN BY DEFAULT-->
    <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasRegister" aria-labelledby="offcanvasRightLabel">
        <div class="offcanvas-header">
            <h5 class="offcanvas-title" id="offcanvasRightLabel">Register</h5>
            <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
        </div>
        <div class="offcanvas-body">
            <div v-if="registering">
                <div class="mb-3">
                <label for="exampleFormControlInput1" class="form-label">Username</label>
                <input v-model="rg_username" type="email" class="form-control" id="registerUsername" placeholder="your username">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlInput1" class="form-label">Email address</label>
                    <input v-model="rg_email" type="email" class="form-control" id="registerEmail" placeholder="name@example.com">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlInput1" class="form-label">Password</label>
                    <input v-model="rg_password" type="password" class="form-control" id="registerPassword" placeholder="your password">
                </div>
                <div class="mb-3">
                    <label for="exampleFormControlInput1" class="form-label">Confirm Password</label>
                    <input v-model="rg_repassword" type="password" class="form-control" id="registerRePassword" placeholder="confirm password">
                </div>
                <button type="submit" class="btn btn-primary" @click="handleRegister">Register</button>
            </div>
            <div v-if="!registering">
                <p>You have been sent an email to confirm your inscription.</p>
                <p>You can now login.</p>
            </div>
            
        </div>
    </div>


  </header>
</template>

<script>

export default {
    name: "SiteHeader",
    data() {
        return {
            isConnected: false,
            registering: false,
            userName: "proplayer",
            lg_email: "",
            lg_password: "",
            rg_email: "",
            rg_password: "",
            rg_repassword: "",
            rg_username: ""
        };
    },
    mounted(){
        if(sessionStorage.getItem("token")){
            fetch('http://3.135.95.15:3001/users/myInfo', {
                headers: {
                'Content-Type': 'application/json',
                'authorization': sessionStorage.getItem("token")
                }
            })
            .then((response)=>{return(response.json())})
            .then((parsed) => {this.isConnected = true; this.userName = parsed.Username})
        }
    },
    updated(){
        if(sessionStorage.getItem("token")){
            fetch('http://3.135.95.15:3001/users/myInfo', {
                headers: {
                'Content-Type': 'application/json',
                'authorization': sessionStorage.getItem("token")
                }
            })
            .then((response)=>{return(response.json())})
            .then((parsed) => {this.isConnected = true; this.userName = parsed.Username})
        }
    },
    methods:{
        handleLogin(){

            let obj = {Email:this.lg_email,Password:this.lg_password}
            fetch( 'http://3.135.95.15:3001/auth/login', {
                method: 'POST',
                headers: {
                'Content-Type': 'application/json'
                },
                body: JSON.stringify(obj)
            }).then((res) => {return(res.json())}).then((json) => {sessionStorage.setItem("token",json.token)}).then(()=>{
                fetch('http://3.135.95.15:3001/users/myInfo', {
                    headers: {
                    'Content-Type': 'application/json',
                    'authorization': sessionStorage.getItem("token")
                    }
                })
                .then((response)=>{return(response.json())})
                .then((parsed) => {
                    this.lg_email = "";
                    this.lg_password = "";
                    this.userName = parsed.Username;
                    this.isConnected = true;
                    this.$parent.updateScores();
                })
                })
        },
        handleRegister(){
            if(this.rg_password === this.rg_repassword){
                let obj = {Email:this.rg_email,Password:this.rg_password,Username:this.rg_username}
                fetch( 'http://3.135.95.15:3001/auth/register', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(obj)
                })
                .then(()=>{
                    this.rg_email = "";
                    this.rg_username = "";
                    this.rg_password = "";
                    this.rg_repassword = "";
                    this.registering = false;
                })
            }else{
                alert("The passwords don't match !")
            }
        },
        handleDisconnexion(){
            fetch('http://3.135.95.15:3001/auth/disconnect', {
                headers: {
                'Content-Type': 'application/json',
                'authorization': sessionStorage.getItem("token")
                }
            }).then(() => {
                sessionStorage.clear();
                this.isConnected = false;
                this.$parent.updateScores();
            })
        }
    }
}
</script>

<style lang="scss" scoped>

h1{
    color: #DCDCDD;
}

header{
    padding: 1vw;
    display: flex;
    justify-content: space-between;
    background-color: #46494C;
    height: 10vh;
    border-bottom: 2px solid #1985A1;
}

</style>