<template>
  <div>
    <p>{{letter}}</p>
  </div>
</template>

<script>
export default {
    name:"LetterCase",
    props:{
        letter: String
    }
}
</script>

<style lang="scss" scoped>
    div{
        height: 15vh;
        width: 10vw;
        border-radius: 5px;
        background-color: #1985A1;
        padding: 3vh;
    }
    p{
        font-size: 9vh;
        margin: auto;
    }
</style>