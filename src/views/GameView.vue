<template>
    <div>
        <SiteHeader/>
        <div v-if="!ready">
            <p>...loading...</p>
        </div>

        <div v-if="ready" id="gameScreen">
            <section id="gameBar">
                <p class="display-6">SCORE: {{score}}</p>
                <button type="button" class="btn btn-danger" @click="handleQuit">QUIT</button>
            </section>
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
                    <button type="button" class="btn btn-primary" @click="handleSubmitLetter()">SUBMIT</button>
                </div>
            </section>
        </div>
        <Transition>
            <SuccessPopup v-if="winState" :score="score" :word="word" :pic="picture"/>
        </Transition>
        <Transition>
            <FailPopup  v-if="loseState" :score="score" :word="word" :pic="picture"/>
        </Transition>

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
            picture:"",
            theme:"",
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
            if(sessionStorage.getItem("token")){
                this.postScore();
            }
        },
        handleRetry(){
            this.loseState = false;
            this.letter="";
            this.loadWord();
        },
        handleQuit(){
            this.$router.replace("/");
        },
        loadWord(){
            this.ready = false;
            this.badletters = [];
            fetch('http://3.135.95.15:3001/words/'+this.$route.params.theme)
            .then((response)=>{return(response.json())})
            .then((parsed) => {
                this.word = parsed.Name;
                this.picture = parsed.Picture;
                this.theme = parsed.Theme;
                this.discovered = []
                for(let i= 0;i<this.word.length;i++){
                    if(this.word[i]===" " || this.word[i]==="-"){
                        this.discovered[i] = this.word[i];
                    }else{
                        this.discovered[i] = "#";
                    }
                }
            })
            this.ready = true;
        },postScore(){
            this.ready = false;
            fetch( 'http://3.135.95.15:3001/score/'+this.theme, {
                method: 'POST',
                headers: {
                'Content-Type': 'application/json',
                'authorization': sessionStorage.getItem("token")
                },
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
#gameBar{
    font-size: 10vh;
    padding: 1vh;
    color: white;
    display: flex;
    justify-content: space-between;
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease-in;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>