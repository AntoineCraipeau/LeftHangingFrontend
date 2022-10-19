<template>
    <div id="background">
        <section>
            <h1 class="display-1">Fail!</h1>
            <p class="display-6">The word was {{word}}</p>
            <img  v-bind:src=pic />
            <p class="display-6">Final Score: {{score}}</p>
            <div class="buttonRow">
                <button type="button" class="btn btn-warning" @click="this.$parent.$parent.handleRetry">Retry</button>
                <button type="button" class="btn btn-dark" @click="this.$parent.$parent.handleQuit">Quit</button>
            </div>
        </section>
    </div>
</template>

<script>
export default {
    name:"FailPopup",
    props:{
        score: Number,
        word: String,
        pic: String
    }
}
</script>

<style lang="scss" scoped>

    #background{
        position: absolute;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background-color: rgba(255, 0, 0, 0.464);
    }

    img{
        height: 60%;
        width: auto;
        margin: 1%;
    }

    section{
        position: absolute;
        top: 10vh;
        left: 30vw;
        height: 80vh;
        width: 40vw;
        min-width: 600px;
        border-radius: 10vh;
        box-shadow: rgba(0, 0, 0, 0.5) 1vh 1vh;
        background-color: rgb(226, 46, 46);
        padding: 2%;
    }

    .buttonRow{
        display: flex;
        justify-content: space-around;
        width: 50%;
        margin-left: auto;
        margin-right: auto;
    }

    .display-6{
      font-size: larger;
    }

</style>