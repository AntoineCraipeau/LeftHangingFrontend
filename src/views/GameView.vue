<template>
    <div>
        <SiteHeader/>
        <section id="gaugeWrapper">
            <p>{{badletters}}</p>
        </section>
        <section id="wordWrapper">
            <p>{{discovered}}</p>
        </section>
        <section id="submitWrapper">
            <input v-model="letter" maxlength="1" type="text" placeholder="..."/>
            <button @click="handleSubmitLetter()">SUBMIT</button>
        </section>
    </div>
</template>

<script>

import SiteHeader from "../components/SiteHeader.vue";
export default {
    name: "GameView",
    components: { SiteHeader },
    data(){
        return{
            letter:"",
            word:"tuna",
            discovered:[],
            badletters:[]
        }
    },
    beforeMount(){
        for(let i= 0;i<this.word.length;i++){
            this.discovered[i] = "#"
        }
        console.log(this.$route.params.theme)
        fetch('/api/words/'+this.$route.params.theme)
        .then((response)=>{return(response.json())})
        .then((parsed)=>{console.log(parsed)})
    },
    beforeUnmount(){
        alert("You will lose your progression if you leave the game");
    },
    methods:{
        handleSubmitLetter(){
            if(this.letter!==''){
                if(this.badletters.includes(this.letter) || this.discovered.includes(this.letter)){
                    alert("The letter has already been entered")
                }else{
                    if(this.word.indexOf(this.letter)>(-1)){
                        this.discovered[this.word.indexOf(this.letter)] = this.letter;
                        if(!this.discovered.includes("#")){
                            this.handleVictory();
                        }
                    }else{
                        this.badletters.push(this.letter);
                    } 
                }
                this.letter="";
            }
        },
        handleVictory(){
            alert("Victory");
            this.$router.replace("/")
        }
    }
}
</script>

<style lang="scss" scoped>

</style>