<template>

  <div class="App">
    <h1> siemens tia test prj </h1>
    <p id="instruction"> {{instruction}} </p>
  <div class="Cards noselect" >

    <div class="TheCard">
      <div id="frontCard1" v-bind:class="{Front: !cardStyles[0]}">Front</div>
      <div id="backCard1" v-bind:class="{Back: !cardStyles[0]}">{{cardStatus[0]}}</div>
    </div>
    <div class="TheCard">
      <div @click="rotate(['frontCard2','backCard2'], 1)" id="frontCard2" class="Front">Front</div>
      <div @click="rotate(['frontCard2','backCard2'], 0)" id="backCard2" class="Back">{{cardStatus[1]}}</div>
    </div>
    <div class="TheCard">
      <div @click="rotate(['frontCard3','backCard3'], 1)" id="frontCard3" class="Front">Front</div>
      <div @click="rotate(['frontCard3','backCard3'], 0)" id="backCard3" class="Back">{{cardStatus[2]}}</div>
    </div>

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
    backface-visibility: hidden;
    display: inline-block;
  }

  .Front{
    position: absolute;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    backface-visibility: hidden;
    background: #a14d5d;
    border-radius: 4% 4% 4% 4%;
  }
  .Front:hover{
    animation: shadow 1s infinite alternate;
  }

  .Back{
    position: absolute;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    backface-visibility: hidden;
    background: #76cc39;
    border-radius: 4% 4% 4% 4%;
    transform: rotateY(180deg);
  }

  #frontCard1, #backCard1, #frontCard2, #backCard2 ,#frontCard3, #backCard3{
    transition: all .8s ease;
    box-shadow: 0.4em 0.4em 5px rgba(122,122,122,0.5);
  }
  #frontCard1, #backCard1{
    box-shadow: -0.4em 0.4em 5px rgba(122,122,122,0.5);
  }
  #frontCard2, #backCard2{
    box-shadow: 0 0.4em 5px rgba(122,122,122,0.5);
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
