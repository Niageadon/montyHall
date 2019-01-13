<template>

  <div class="App">
    <h1> siemens tia test prj </h1>
    <p> {{instruction}} </p>
    <p> win:{{gameStat[0]}} lose:{{gameStat[1]}}</p>
    <div class="Cards noselect" >
      <div ref="Card1" id="Card1" v-on:click="onCardClick(0)" v-bind:class="{Front: !cardStyles[0], Back: cardStyles[0]}"> </div>
      <div ref="Card2" id="Card2" v-on:click="onCardClick(1)" v-bind:class="{Front: !cardStyles[1], Back: cardStyles[1]}"> </div>
      <div ref="Card3" id="Card3" v-on:click="onCardClick(2)" v-bind:class="{Front: !cardStyles[2], Back: cardStyles[2]}"> </div>
    </div>

    <div class="fakeImage"> </div>
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
      firstClick: true, firstRandomCard: -1,
      gameIsDone: false,
      numOfWinCard: 0,
      cardStyles: [false, false, false],
      cardStatus: ['Fake', 'Fake', 'Fake'],
      instruction: 'Select the card',
      gameStat: [0, 0]  // win, lose
      };
    },

    created(){
      this.generateWinCard() // Generate first "win" card
      this.$refs.Card1.style.color = 'red'
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

      onCardClick: function(numOfClickedCard)
      {
        if(this.gameIsDone || this.firstRandomCard === numOfClickedCard) return;
        else {
            if (this.firstClick)
            {
                this.openFakeCard(numOfClickedCard);
                this.firstClick = false;
            }
            else
            {
              this.gameIsDone = true;
              let gameWin = (numOfClickedCard === this.numOfWinCard);
              this.rotateCard(numOfClickedCard);
              this.setGameStat(gameWin);
            setTimeout(() => {
              this.restart();
            }, 1100);
            }

        }
      },

      openFakeCard: function(numOfClickedCard)
      {
        let cardToOpen = 0;
        if (numOfClickedCard === this.numOfWinCard)
        {
          switch (numOfClickedCard)
          {
            case(0): cardToOpen = Math.random()>0.5? 1 : 2; break;
            case(1): cardToOpen = Math.random()>0.5? 0 : 2; break;
            case(2): cardToOpen = Math.random()>0.5? 0 : 1; break;
          }
        }
        else {
            switch (this.numOfWinCard)
            {
                case(0): cardToOpen = numOfClickedCard === 1? 2 : 1; break;
                case(1): cardToOpen = numOfClickedCard === 0? 2 : 0; break;
                case(2): cardToOpen = numOfClickedCard === 1? 0 : 1; break;
            }
        }
        this.firstRandomCard = cardToOpen
        this.rotateCard(cardToOpen);
      },

      randomCard: function()
      {
        return Math.ceil(Math.random() * 3 - 1)
      },

      generateWinCard()
      {
        this.numOfWinCard = this.randomCard()
        switch (this.numOfWinCard)
        {
          case (0): this.cardStatus[0] = "Win"; break;
          case (1): this.cardStatus[1] = "Win"; break;
          case (2): this.cardStatus[2] = "Win"; break;
        }
      },

      restart: function () {
          this.cardStyles.splice(0, 1, false);
          this.cardStyles.splice(1, 1, false);
          this.cardStyles.splice(2, 1, false);
          this.cardStatus.splice(0, 3, "fake");
          this.cardStatus.splice(1, 1, "fake");
          this.cardStatus.splice(2, 1, "fake");
          //this.cardStatus[0] = "fake";
          //this.cardStatus[1] = "fake";
          //this.cardStatus[2] = "fake";
          this.generateWinCard();
          this.firstRandomCard = -1;
          this.gameIsDone = false;
          this.firstClick = true;
      },

      setGameStat:function (gameResult)
      {
        gameResult === true?  this.gameStat[0]++: this.gameStat[1]++
      }

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
          if(this.firstClick === true)
          {return this.instruction = 'Chose again'}
          else return this.instruction = 'Select the card'
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

  .winImage{
      margin-top: 200px ;
      display: block;
    background-image: url('https://i.ytimg.com/vi/b7YnpWa678s/maxresdefault.jpg');
      width: 200px;
      height: 200px;
  }

  .fakeImage{
    background-color: blueviolet;
    background-image: url("../assets/w.svg");
    background-repeat: no-repeat;
    background-position: center center;

    background-size: contain;

    width: 150px;
    height: 150px;
    margin-top: 170px;
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