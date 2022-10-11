<template>
  <p>Your Best Score : {{personnalBest}}</p>
  <button @click="fullDisplay = !fullDisplay">See All</button>
  <div v-if="fullDisplay" id="background">
    <section>
        <button @click="fullDisplay = !fullDisplay">Close</button>
        <h1>Scoreboard : {{theme}}</h1>
        <p v-for="score in scoreList" :key="score.id">{{score.owner+':'+score.score}}</p>
    </section>
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
            personnalBest: Number
        }
    },
    beforeMount(){
        fetch('http://3.135.95.15:3001/score/'+this.theme)
        .then((response)=>{return(response.json())})
        .then((parsed) => {this.scoreList = parsed})

        fetch('http://3.135.95.15:3001/pscore/'+this.theme)
        .then((response)=>{return(response.json())})
        .then((parsed) => {this.personnalBest = parsed.score})
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

section{
    position: absolute;
    top: 30vh;
    left: 30vw;
    height: 40vh;
    width: 40vw;
    box-shadow: rgba(0, 0, 0, 0.5) 1vh 1vh;
    background-color: #46494C;
}

</style>