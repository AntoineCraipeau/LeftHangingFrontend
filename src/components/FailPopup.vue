<template>
    <div id="background">
        <section>
            <h1>Fail!</h1>
            <p>The word was {{word}}</p>
            <p>Current Score: {{score}}</p>
            <button @click="this.$parent.handleRetry">Retry</button>
            <button @click="this.$parent.handleQuit">Quit</button>
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

    section{
        position: absolute;
        top: 30vh;
        left: 30vw;
        height: 40vh;
        width: 40vw;
        border-radius: 10vh;
        box-shadow: rgba(0, 0, 0, 0.5) 1vh 1vh;
        background-color: rgb(226, 46, 46);
    }

</style>