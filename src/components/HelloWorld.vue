<template>

  <div class="App">
    <h1> siemens tia test prj </h1>
    <p id="instruction"> {{instruction}} </p>
  <div class="Cards noselect" >
      <div id="Card1" v-on:click="rotateCard(0)" v-bind:class="{Front: !cardStyles[0], Back: cardStyles[0]}"> </div>
      <div id="Card2" v-on:click="rotateCard(1)" v-bind:class="{Front: !cardStyles[1], Back: cardStyles[1]}"> </div>
      <div id="Card3" v-on:click="rotateCard(2)" v-bind:class="{Front: !cardStyles[2], Back: cardStyles[2]}"> </div>
  </div>
  </div>

</template>

<script>
  export default {
    name: 'HelloWorld',
    props: {
      msg: String
    },
    data: function()
    {
    return{
      firstClick: true,
      test: false,
      numOfWinCard: 0,
      numOfChosenCard: 0,
      cardStyles: [false, false, false],
      cardStatus: ['Fake', 'Fake', 'Fake'],
      instruction: 'Select the card',

      };
    },

    created(){
      this.generateWinCard() // Первый
    },

    methods:
    {
      rotateCard: function(numOfCard)
      {
        switch(numOfCard)
        {
          case(0): this.cardStyles.splice(0, 1, !this.cardStyles[0]); break;
          case(1): this.cardStyles.splice(1, 1, !this.cardStyles[1]); break;
          case(2): this.cardStyles.splice(2, 1, !this.cardStyles[2]); break;
        }
      },

      styleTransform: function(item){
        //this.$refs.heading.style.color ='red'
        item.target.style.backgroundColor = 'red'
        console.log(item)
      },

      onCardClick: function([idFront, idBack])
      {
        if(this.firstClick)
        {


          this.instruction = "Now, chose one again"
          this.firstClick = !this.firstClick
        }
      },

      openFakeCard: function()
      {
        let cardToOpen = 0;
        if (this.numOfChosenCard === this.numOfWinCard)
        {
          switch (this.numOfChosenCard)
          {
            case(1): cardToOpen = Math.random()>0.5? 2:3; break;
            case(2): cardToOpen = Math.random()>0.5? 1:3; break;
            case(3): cardToOpen = Math.random()>0.5? 1:2; break;
          }
        }

        switch (this.numOfWinCard)
        {
          case(1): cardToOpen = this.numOfChosenCard === 2? 3:2; break;
          case(2): cardToOpen = this.numOfChosenCard === 1? 3:1; break;
          case(3): cardToOpen = this.numOfChosenCard === 1? 2:1; break;
        }

        return cardToOpen;
      },

      randomCard: function()
      {
        return Math.ceil(Math.random() * 3)
      },

      generateWinCard()
      {
        this.numOfWinCard = this.randomCard()
        switch (this.numOfWinCard) {
          case (1):
            this.cardStatus[0] = "Win";
            break;
          case (2):
            this.cardStatus[1] = "Win";
            break;
          case (3):
            this.cardStatus[2] = "Win";
            break;
        }
      },

      rotate: function([idFront, idBack], front)
      { // rotate card's on click
        if (front)
        {
          document.getElementById(idFront).style.transform = "rotateY(180deg)";
          document.getElementById(idBack).style.transform = "rotateY(0deg)";
        }
        else
        {
          document.getElementById(idFront).style.transform = "rotateY(0deg)";
          document.getElementById(idBack).style.transform = "rotateY(180deg)";
        }},
    },

    computed:
    {
      cardStyle () {
        return { transform: 'rotateY(' + this.turn + 'deg)'}
      },

    },

    watch:
    {
      firstClick: function()
      {
        return this.test = !this.firstClick
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

  .App{
    width: 100%;
    height: 600px;
  }

  .Cards{
    position: absolute;
    right: 15%; left: 15%;
    width: 70%; height: 30vw;
    background: aqua;
  }

  .noselect {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    user-select: none;
  }

  .TheCard{
    position: relative;
    margin: 2% 3.5%;
    width: 25%;
    height: 90%;
    transform-style: preserve-3d;
    display: inline-block;
  }

  .Front{
    position: relative;
    width: 25%;
    height: 90%;
    margin: 2% 3.5%;
    transform-style: preserve-3d;
    display: inline-block;
    background: black;
    transition: all .8s ease;
    box-shadow: 0 0.4em 5px rgba(122,122,122,0.5);
  }
  .Front:hover{
    animation: shadow 1s infinite alternate;
  }

  .Back{
    position: relative;
    width: 25%;
    height: 90%;
    margin: 2% 3.5%;
    transform-style: preserve-3d;
    display: inline-block;
    background: #c55c4b;
    transform: rotateY(180deg);
    transition: all .8s ease;
  }



  @keyframes shadow {
    from {
      box-shadow: 0 0 55px 10px #ff2217;
    }
    to {
      box-shadow: 0 0 90px 20px #000000;
    }
  }

</style>
