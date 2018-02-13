<template>
  <div id="app">
    <div id="header">
        <h1>Rock Paper Scissors</h1>
        <p id="intro">
            There will be 9 rounds, please type a bet and click your move.
            If there is a tie your bet will automatically be doubled.
            Once all rounds are complete click reset.
            Your High Score will be kept between rounds.
        </p>

        Bet: <input type="number" v-model="bet" title="bet"><br><br>

        <button type="button" v-on:click="RPS('rock')">Rock</button>
        <button type="button" v-on:click="RPS('paper')">Paper</button>
        <button type="button" v-on:click="RPS('scissors')">Scissors</button>
        <button type="button" v-on:click="RPSRand">Random</button>
        <button type="button" v-on:click="Reset">Reset</button><br>
        <br>
        <span id="hsMSG">{{ hsMSG }}</span>
        <span id="highScore">High Score: {{ highScore }}</span><br>
        <span id="round">Round: {{ round }}</span>
        <p id="rMSG">{{ rMSG }}</p>
        <p id="results">{{ results }}</p>
    </div>
    <div id="p1">
        <h4>Player 1</h4>
        <span id="player1"><strong>Move: </strong>{{ player1Turn }}  </span>
        <span id="p1Credits"><strong>Credits: </strong>{{ p1Credits }}</span><br>
        <img id="player1Img" v-bind:src="p1IMG">
    </div>
    <div id="p2">
        <h4>Player 2</h4>
        <span id="player2"><strong>Move: </strong>{{ player2Turn }}  </span>
        <span id="p2Credits"><strong>Credits: </strong>{{ p2Credits }}</span><br>
        <img id="player2Img" v-bind:src="p2IMG"><br>
    </div>
  </div>
</template>

<script>
export default
{
  name: 'app',
  data ()
  {
    return{
      player1Turn: 'None',
      player2Turn: 'None',
      round: 0,
      p1Credits: 150,
      p2Credits: 150,
      p1IMG: 'https://i.imgur.com/VFyOGbE.png',
      p2IMG: 'https://i.imgur.com/VFyOGbE.png',
      bet: 0,
      mustDouble: false,
      highScore: 0,
      hsMSG: '',
      rMSG: '',
      results: ''
    }
  },

  methods:
  {
    // Used for other buttons.
    RPS: function(p1Turn)
    {
          var vm = this;
          vm.player1Turn = p1Turn;
          vm.player2Turn = '';

          // Check if we've done 9 rounds.
          if (vm.round === 9)
          {
              if (vm.p1Credits > vm.p2Credits)
              {
                  vm.rMSG = "Player 1 Wins Overall! That was the last round click reset to try again!";
                  vm.results = '';

                  if(vm.p1Credits > vm.highScore)
                  {
                      vm.highScore = vm.p1Credits;
                      vm.hsMSG = "New High Score!";
                  }
              }
              else
              {
                  vm.rMSG = "Player 2 Wins Overall! That was the last round click reset to try again!";
                  vm.results = '';
              }
          }

          // Checks if we're still going.
          if (vm.round !== 9)
          {
              // Checks if the player must bet double.
              if (vm.mustDouble === true)
              {
                  vm.bet = parseInt(vm.bet) * 2;
                  vm.mustDouble = false;
              }

              // Math for computer player.
              switch (Math.floor(Math.random() * 3) +1)
              {
                  case 1:
                      vm.player2Turn = "rock";
                      break;
                  case 2:
                      vm.player2Turn = "paper";
                      break;
                  case 3:
                      vm.player2Turn = "scissors";
                      break;
              }

              // Determines picture displayed for each player.
              if (vm.player1Turn === "rock")
              {
                  vm.p1IMG = "https://i.imgur.com/DBcTJR3.jpg";
              }
              else if (vm.player1Turn === "paper")
              {
                  vm.p1IMG = "https://i.imgur.com/pkX5uPU.jpg";
              }
              else if (vm.player1Turn === "scissors")
              {
                  vm.p1IMG = "https://i.imgur.com/b7K9zyR.jpg";
              }

              if (vm.player2Turn === "rock")
              {
                  vm.p2IMG = "https://i.imgur.com/DBcTJR3.jpg";
              }
              else if (vm.player2Turn === "paper")
              {
                  vm.p2IMG = "https://i.imgur.com/pkX5uPU.jpg";
              }
              else if (vm.player2Turn === "scissors")
              {
                  vm.p2IMG = "https://i.imgur.com/b7K9zyR.jpg";
              }

              // Determines winner and applies credits if tied forces double bid on next found.
              if (vm.player1Turn === vm.player2Turn)
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  vm.results = "It's a tie! Your next bet will automatically be doubled.";
                  vm.mustDouble = true;
              }

              if (vm.player1Turn === "rock" && vm.player2Turn === "paper")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  console.log("Player 2 wins!");
                  vm.results = "Player 2 Wins!";
                  vm.p1Credits-= parseInt(vm.bet);
                  vm.p2Credits+= parseInt(vm.bet);
              }

              else if (vm.player1Turn === "rock" && vm.player2Turn === "scissors")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  console.log("Player 1 wins!");
                  vm.results = "Player 1 Wins!";
                  vm.p1Credits+= parseInt(vm.bet);
                  vm.p2Credits-= parseInt(vm.bet);
              }

              else if (vm.player1Turn === "paper" && vm.player2Turn === "rock")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  console.log("Player 1 wins!");
                  vm.results = "Player 1 Wins!";
                  vm.p1Credits+= parseInt(vm.bet);
                  vm.p2Credits-= parseInt(vm.bet);
              }

              else if (vm.player1Turn === "paper" && vm.player2Turn === "scissors")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  console.log("Player 2 wins!");
                  vm.results = "Player 2 Wins!";
                  vm.p1Credits-= parseInt(vm.bet);
                  vm.p2Credits+= parseInt(vm.bet);
              }

              else if (vm.player1Turn === "scissors" && vm.player2Turn === "rock")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  console.log("Player 2 wins!");
                  vm.results = "Player 2 Wins!";
                  vm.p1Credits-= parseInt(vm.bet);
                  vm.p2Credits+= parseInt(vm.bet);
              }

              else if (vm.player1Turn === "scissors" && vm.player2Turn === "paper")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  console.log("Player 1 wins!");
                  vm.results = "Player 1 Wins!";
                  vm.p1Credits+= parseInt(vm.bet);
                  vm.p2Credits-= parseInt(vm.bet);
              }

              vm.round++;
          }

      },

    // Used for Random button.
    RPSRand: function()
    {
        var vm = this;
    switch (Math.floor(Math.random() * 3) +1)
    {
        case 1:
            vm.player1Turn= 'rock';
            break;
        case 2:
            vm.player1Turn= 'paper';
            break;
        case 3:
            vm.player1Turn= 'scissors';
            break;
    }
    vm.RPS(player1Turn)
},

    // Used for Reset button.
    Reset: function()
    {
        var vm = this;
        vm.round = 0;
        vm.p1Credits = 150;
        vm.p2Credits = 150;
        vm.p1IMG = "https://i.imgur.com/VFyOGbE.png";
        vm.p2IMG = "https://i.imgur.com/VFyOGbE.png";
        vm.results = '';
        vm.player1Turn = 'none';
        vm.player2Turn = 'none';
        vm.rMSG = '';
        vm.hsMSG = '';
    },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

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

    #p1
    {
        width: 50%;
        float: left;
    }

    #p2
    {
        width: 50%;
        float: right;
    }
    #header
    {
        width: 100%;
    }
    #intro
    {
        width: 50%;
        padding-left: 25%;
    }
    #app
    {
        margin: 0;
    }
</style>
