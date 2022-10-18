<template>
    <p id="pBest">Your Best Score : {{personnalBest}}</p>
    <button class="btn btn-primary" type="button" data-bs-toggle="offcanvas" v-bind:data-bs-target="'#'+theme" v-bind:aria-controls="theme">See all</button>
    <div class="offcanvas offcanvas-end" tabindex="-1" v-bind:id="theme" aria-labelledby="offcanvasRightLabel">
        <div class="offcanvas-header">
            <h5 class="offcanvas-title" id="offcanvasRightLabel">Scoreboard : {{theme}}</h5>
            <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
        </div>
        <div class="offcanvas-body">
            <p v-for="score in scoreList" :key="score.Moment">{{score.Username+':'+score.Score+':'+score.Moment}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "ScoreBoard",
    props:{
        theme:String
    },
    data(){
        return{
            fullDisplay: false,
            scoreList: [],
            personnalBest: "Connect to register your scores!"
        }
    },
    beforeMount(){
        fetch('/api/score/'+this.theme)
        .then((response)=>{return(response.json())})
        .then((parsed) => {this.scoreList = parsed})

        if(sessionStorage.getItem("token")){
            fetch('/api/users/score/'+this.theme,{
                    headers: {
                    'Content-Type': 'application/json',
                    'authorization': sessionStorage.getItem("token")
                    }
            })
            .then((response)=>{return(response.json())})
            .then((parsed) => {this.personnalBest = parsed.Score})
        }
    }
}
</script>

<style lang="scss" scoped>

#background{
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: #46494c79;
}

#pBest{
    text-align: center;
    background-color: #26282abe;
    border-radius: 5px;
    width: 35%;
    height: 10%;
    margin-left: auto;
    margin-right: auto;
}

section{
    position: absolute;
    top: 30vh;
    left: 30vw;
    height: 40vh;
    width: 40vw;
    box-shadow: rgba(0, 0, 0, 0.5) 1vh 1vh;
    background-color: #46494C;
}

.offcanvas{
    color: black;
}

</style>