<template>

  <div class="App">
    <a href="https://en.wikipedia.org/wiki/Monty_Hall_problem">  <h1 > "The Monty Hall Dilemma" </h1>  </a>
    <p>  {{instruction}} </p>

    <div class="Cards noSelect" >
        <div class="TheCard" v-on:click="onCardClick(0)">
            <div class="nFront" v-if="!cardStyles[0]"></div>
            <div class="nBack" v-else v-bind:class="{winImage: imgStatus[0]==='Win',fakeImage1: imgStatus[0]==='Lose1', fakeImage2: imgStatus[0]==='Lose2'}" >  </div>
        </div>

        <div class="TheCard" v-on:click="onCardClick(1)">
            <div class="nFront" v-if="!cardStyles[1]"></div>
            <div class="nBack" v-else v-bind:class="{winImage: imgStatus[1]==='Win',fakeImage1: imgStatus[1]==='Lose1', fakeImage2: imgStatus[1]==='Lose2'}" >  </div>
        </div>

        <div class="TheCard" v-on:click="onCardClick(2)">
            <div class="nFront" v-if="!cardStyles[2]"></div>
            <div class="nBack" v-else v-bind:class="{winImage: imgStatus[2]==='Win',fakeImage1: imgStatus[2]==='Lose1', fakeImage2: imgStatus[2]==='Lose2'}" >  </div>
        </div>
    </div>
      <p> Win:{{gameStat[0]}}   &nbsp &nbsp &nbsp   Lose:{{gameStat[1]}}</p>
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
      firstClick: true, firstRandomCard: -1, numOfWinCard: -1,
      gameIsDone: false,
      cardStyles: [false, false, false],
      cardStatus: ['Fake', 'Fake', 'Fake'], imgStatus: ['', '', ''],
      instruction: 'Select the card',
      gameStat: [0, 0]  // num of win's, lose's
      };
    },

    created(){
      this.generateWinCard(); // Generate first "win" card
      //this.getImgforCard(); // Generate rand img's for card's
      this.getImgforCard();
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
              this.openLastCard();
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
        /*____________________________________
        Случайное целое число от 0 до 2
        ------------------------------------*/
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

      openLastCard: function()
      {
          if(this.cardStyles[0] === false) this.cardStyles.splice(0, 1, true);
          if(this.cardStyles[1] === false) this.cardStyles.splice(1, 1, true);
          if(this.cardStyles[2] === false) this.cardStyles.splice(2, 1, true);
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
          //this.instruction = 'Select the card'
          this.getImgforCard();
      },

      setGameStat:function (gameResult)
      {
        gameResult === true?  this.gameStat[0]++: this.gameStat[1]++
      },

      getImgforCard: function () {
         let firstLoseImgIsUsed = false;
         let secondLoseImgIsUsed = false;
         this.imgStatus = ['', '', ''];
          for(let i=0; i<3; i++)
          {
            if(this.cardStatus[i] === 'Win') this.imgStatus[i]= 'Win';
            else
            {
                if(firstLoseImgIsUsed) this.imgStatus[i] = 'Lose2'
                else
                {
                if(secondLoseImgIsUsed) this.imgStatus[i] = 'Lose1';
                else
                {
                  Math.random() > 0.5 ? this.imgStatus[i] = 'Lose1' : this.imgStatus[i] = 'Lose1';
                }
                }
                this.imgStatus[i]==='Lose1'? firstLoseImgIsUsed = true: secondLoseImgIsUsed = true;
                if (firstLoseImgIsUsed)
                {
                  true
                }
            }
          }

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
          {return this.instruction = 'Select the card'}
          else return this.instruction = 'Сhoose again'
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    /* colours: https://colorscheme.ru/#3T405hWhWzcuD */
h1{
    padding-top: 2.5vw;
    font-size: 3vw;
    margin: 0 0;
    color: rgb(245, 227, 170);
    font-family: Impact;
}
p{
    font-size: 2.5vw;
    color: rgb(245, 227, 170);
    margin: 0 0;
    padding-top: 2vw;
    font-family: fantasy;
}
A{
    color: #8F783F;
}

  .App{
    width: 100%;
      height: 45vw;
      display: flex;
      /*justify-content: space-evenly;*/  /* равномерное распределение дочерних элементов */
      flex-direction: column;
  }

  .Cards{
      margin: 2vw 0;
    position: relative;
    display: flex;
    justify-content: space-evenly;  /* равномерное распределение дочерних элементов */
    align-items: center;            /* центрирование по высоте */
    right: 15%; left: 15%;
    width: 70%; height: 55%;
  }

  .noSelect {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -o-user-select: none;
    user-select: none;
  }

  .TheCard{
    /*position: relative;*/
      display: flex;
      justify-content: space-evenly;  /* равномерное распределение дочерних элементов */
      align-items: center;
    width: 25%;
    height: 100%;
    transform-style: preserve-3d;

  }

.nFront{
    position: relative;
    width: 85%;
    height: 85%;
    transform-style: preserve-3d;
    background: #C4AC71;
    border-radius: 4% 4% 4% 4%;
    transition: all .8s ease;
}
.nFront:hover{
    /*animation: shadow 1s infinite alternate;*/
    background: #C4B286;
    width: 90%;
    height: 90%;
}

.nBack{
    position: relative;
    transform-style: preserve-3d;
    background: #BC8094;
    transform: rotateY(180deg);
    border-radius: 4% 4% 4% 4%;
    transition: all .8s ease;
  }

  .winImage{
      background-image: url("../assets/w.svg");
      background-color: #94BD6C;
      background-repeat: no-repeat;
      background-position: center center;
      background-size: contain;
      width: 100%;
      height: 100%;
  }

  .fakeImage1{
        background-image: url("../assets/f1.svg");
        background-repeat: no-repeat;
        background-position: center center;
        background-size: 60%;
        width: 95%;
        height: 95%;
    }

  .fakeImage2{
        background-image: url("../assets/f2.svg");
        background-repeat: no-repeat;
        background-position: center center;
        background-size: 60%;
        width: 95%;
        height: 95%;
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