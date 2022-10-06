<template>
  <header>
    <div>
        <h1>WORD PANIC</h1>
    </div>
    <div class="dropdown">
        <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            {{isConnected? userName : "Connexion"}}
        </button>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Login</a></li>
            <li><a class="dropdown-item" href="#">Register</a></li>
        </ul>
    </div>
  </header>
</template>

<script>
export default {
    name: 'SiteHeader',
    data(){
        return {
            isConnected: false,
            userName: "proplayer"
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