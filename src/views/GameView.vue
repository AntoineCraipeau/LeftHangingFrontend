<template>
    <div>
        <SiteHeader/>
        <div v-if="!ready">
            <p>...loading...</p>
        </div>

        <div v-if="ready" id="gameScreen">
            <section id="gaugeWrapper">
                <div id="gauge">
                    <ErrorCase v-for="(letter,index) in badletters" :letter="letter" :key="index"/>
                </div>
                <div id="bomb"></div>
            </section>
            <section id="wordWrapper">
                <LetterCase v-for="(letter,index) in discovered" :letter="letter" :key="index"/>
            </section>
            <section id="submitWrapper">
                <div>
                    <input v-model="letter" maxlength="1" type="text" placeholder="" id="letterPrompt" ref="letterPrompt" autofocus/>
                </div>
                <div>
                    <button @click="handleSubmitLetter()">SUBMIT</button>
                </div>
            </section>
            <section>
                <p>Score: {{score}}</p>
            </section>
        </div>

        <div v-if="winState">
            <SuccessPopup :score="score" :word="word"/>
        </div>

        <div v-if="loseState">
            <FailPopup :score="score" :word="word"/>
        </div>

    </div>
</template>

<script>

import SiteHeader from "../components/SiteHeader.vue";
import SuccessPopup from "@/components/SuccessPopup.vue";
import FailPopup from "@/components/FailPopup.vue"
import LetterCase from "@/components/LetterCase.vue";
import ErrorCase from "@/components/ErrorCase.vue"

export default {
    name: "GameView",
    components: { SiteHeader, SuccessPopup, FailPopup, LetterCase, ErrorCase },
    data(){
        return{
            ready: false,
            winState: false,
            loseState: false,
            score: 0,
            letter:"",
            word:"",
            discovered:[],
            badletters:[]
        }
    },
    beforeMount(){
        this.loadWord();
    },
    mounted(){
        window.addEventListener('keydown', (e) => {
            if (e.key == 'Enter') {
                this.handleSubmitLetter()
            }
        }); 
    },
    updated(){
        this.$refs.letterPrompt.focus();
    },
    beforeUnmount(){
        
    },
    methods:{
        handleSubmitLetter(){
            if(this.letter!==''){
                this.letter = this.letter.toLowerCase();
                if(this.badletters.includes(this.letter) || this.discovered.includes(this.letter)){
                    alert("The letter has already been entered")
                }else{
                    if(this.word.indexOf(this.letter)>(-1)){
                        for(let i = 0;i<this.word.length;i++){
                            if(this.letter === this.word.charAt(i)){
                                console.log(i);
                                this.discovered[i] = this.letter;
                            }
                        }
                        if(!this.discovered.includes("#")){
                            this.handleWin();
                        }
                    }else{
                        this.badletters.push(this.letter);
                        if(this.badletters.length>9){
                            this.handleLose();
                        }
                    } 
                }
                this.letter="";
                this.$refs.letterPrompt.focus();
            }
        },
        handleWin(){
            this.winState = true;
            this.score++;
        },
        handleContinue(){
            this.winState = false;
            this.loadWord();
        },
        handleLose(){
            this.loseState = true;
            this.postScore();
        },
        handleRetry(){
            this.loseState = false;
            this.loadWord();
        },
        handleQuit(){
            this.$router.replace("/");
        },
        loadWord(){
            this.ready = false;
            this.badletters = [];
            fetch('/api/words/'+this.$route.params.theme)
            .then((response)=>{return(response.json())})
            .then((parsed) => {
                this.word = parsed.response;
                for(let i= 0;i<this.word.length;i++){
                    this.discovered[i] = "#";
                    this.ready = true;
                }
            })
            this.ready = true;
        },postScore(){
            this.ready = false;
            fetch( '/api/score/'+this.$route.params.theme, {
                method: 'POST',
                body: JSON.stringify({score:this.score})
            })
            this.ready = true;
        }
    }
}

</script>

<style lang="scss" scoped>
#gameScreen{
    background-color: #4C5C68;
    color: white;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    height: 90vh;
    width: 100%;
}
#gaugeWrapper{
    padding-top: 5vh;
    height: 25vh;
    #gauge{
        padding: 1vh;
        background-color: rgba(202, 187, 187, 0.507);
        height: 12vh;
        width: 80vw;
        display: flex;
        justify-content: left;
        flex-direction: row;
        margin-left: auto;
        margin-right: auto;
    }
    #bomb{
        height: 20vh;
        width: 20vh;
        border-radius: 20vh;
        background-color: black;
        position: relative;
        left: 81vw;
        top: -16vh;
    }
}
#wordWrapper{
    height: 20vh;
    width: 80vw;
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    margin-left: auto;
    margin-right: auto;
}
#submitWrapper{
    height: 20vh;
    input{
        width: 4vw;
        height: 12vh;
        font-size: 10vh;
    }
}
</style>