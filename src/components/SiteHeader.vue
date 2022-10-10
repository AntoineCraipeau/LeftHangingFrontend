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
            <li><button class="dropdown-item" @click="isConnected = false">Deconnexion</button></li>
        </ul>
    </div>

    <div class="dropdown" v-if="!isConnected">
        <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            Connexion
        </button>
        <ul class="dropdown-menu">
            <li><button class="dropdown-item" @click="logging = true">Login</button></li>
            <li><button class="dropdown-item" @click="registering = true">Register</button></li>
        </ul>
    </div>
  </header>
  <Login v-if="logging"/>
  <Register v-if="registering"/>
</template>

<script>
import Login from './Login.vue';
import Register from './Register.vue';
export default {
    name: "SiteHeader",
    data() {
        return {
            logging: false,
            registering: false,
            isConnected: false,
            userName: "proplayer"
        };
    },
    components: { Login, Register }
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