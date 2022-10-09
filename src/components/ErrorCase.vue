<template>
    <div>
      <p>{{letter}}</p>
    </div>
  </template>
  
  <script>
  export default {
      name:"ErrorCase",
      props:{
          letter: String
      }
  }
  </script>
  
  <style lang="scss" scoped>
      div{
          height: 10vh;
          width: 7.2vw;
          background-color: red;
          padding: 3vh;
      }
      p{
          font-size: 6vh;
          margin: auto;
      }
  </style>