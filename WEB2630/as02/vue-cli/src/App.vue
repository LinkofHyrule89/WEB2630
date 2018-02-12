<template>
  <div id="app">
    <p>Rock Paper Scissors <br>
      There will be 9 rounds, please type a bet and click your move. <br>
      If there is a tie your bet will automatically be doubled. <br>
      Once all rounds are complete click reset. <br>
      Your High Score will be kept between rounds.
    </p>

    Bet: <input type="number" v-model="bet"><br><br>

    <button type="button" v-on:click="RPS('rock', player2Turn)">Rock</button>
    <button type="button" v-on:click="RPS('paper', player2Turn)">Paper</button>
    <button type="button" v-on:click="RPS('scissors', player2Turn)">Scissors</button>
    <button type="button" v-on:click="RPSRand(player1Turn, player2Turn)">Random</button>

    <button type="button" v-on:click="Reset">Reset</button>
    <p id="hsMSG"></p>
    <p id="highScore">High Score: 0</p>
    <p id="rMSG"></p>
    <p id="round">Round: 1</p>
    <p id="results"></p>
    <p id="player1">Player 1 Move:</p>
    <img id="player1Img" src="https://i.imgur.com/VFyOGbE.png">
    <p id="p1Credits">P1 Credits: 150</p>
    <p id="player2">Player 2 Move:</p>
    <img id="player2Img" src="https://i.imgur.com/VFyOGbE.png"><br>
    <p id="p2Credits">P2 Credits: 150</p>
  </div>
</template>

<script>
export default
{
  name: 'app',
  data ()
  {
    return{
      player1Turn,
      player2Turn,
      round: 1,
      p1Credits: 150,
      p2Credits: 150,
      bet: 0,
      mustDouble: false,
      highScore: 0
    }
  },

  methods:
  {
    // Used for other buttons.
    RPS: function(player1Turn, player2Turn)
    {
          var vm = this;

          // Check if we've done 9 rounds.
          if (vm.round == 9)
          {
              if (vm.p1Credits > vm.p2Credits)
              {
                  document.getElementById("rMSG").innerHTML = "Player 1 Wins Overall! That was the last round click reset to try again!";

                  if(vm.p1Credits > vm.highScore)
                  {
                      vm.highScore = vm.p1Credits;
                      document.getElementById("hsMSG").innerHTML = "New High Score!";
                      document.getElementById("highScore").innerHTML = "high Score: " + vm.highScore;
                      document.getElementById("round").innerHTML = "Round: " + vm.round;
                  }
              }
              else
              {
                  document.getElementById("rMSG").innerHTML = "Player 2 Wins Overall! That was the last round click reset to try again!";
                  document.getElementById("round").innerHTML = "Round: " + vm.round;
              }
          }

          // Checks if we're still going.
          if (vm.round != 9)
          {
              // Checks if the player must bet double.
              if (vm.mustDouble === true)
              {
                  vm.et = bet * 2;
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

              // Determines winner and applies credits if tied forces double bid on next found.
              if (vm.player1Turn == vm.player2Turn)
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("It's a tie!");
                  document.getElementById("results").innerHTML= "It's a tie! Your next bet will automatically be doubled.";
                  vm.mustDouble = true;
              }

              if (vm.player1Turn == "rock" && vm.player2Turn == "paper")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("Player 2 wins!");
                  document.getElementById("results").innerHTML= "Player 2 Wins!";
                  vm.p1Credits-= bet;
                  vm.p2Credits+= bet;
                  document.getElementById("p1Credits").innerHTML = "P1 Credits: " + vm.p1Credits;
                  document.getElementById("p2Credits").innerHTML = "P2 Credits: " + vm.p2Credits;
              }

              else if (vm.player1Turn == "rock" && vm.player2Turn == "scissors")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("Player 1 wins!");
                  document.getElementById("results").innerHTML= "Player 1 Wins!";
                  vm.p1Credits+= vm.bet;
                  vm.p2Credits-= vm.bet;
                  document.getElementById("p1Credits").innerHTML = "P1 Credits: " + vm.p1Credits;
                  document.getElementById("p2Credits").innerHTML = "P2 Credits: " + vm.p2Credits;
              }

              else if (vm.player1Turn == "paper" && vm.player2Turn == "rock")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("Player 1 wins!");
                  document.getElementById("results").innerHTML= "Player 1 Wins!";
                  vm.p1Credits+= vm.bet;
                  vm.p2Credits-= vm.bet;
                  document.getElementById("p1Credits").innerHTML = "P1 Credits: " + vm.p1Credits;
                  document.getElementById("p2Credits").innerHTML = "P2 Credits: " + vm.p2Credits;
              }

              else if (vm.player1Turn == "paper" && vm.player2Turn == "scissors")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("Player 2 wins!");
                  document.getElementById("results").innerHTML= "Player 2 Wins!";
                  vm.p1Credits-= vm.bet;
                  vm.p2Credits+= vm.bet;
                  document.getElementById("p1Credits").innerHTML = "P1 Credits: " + vm.p1Credits;
                  document.getElementById("p2Credits").innerHTML = "P2 Credits: " + vm.p2Credits;
              }

              else if (vm.player1Turn == "scissors" && vm.player2Turn == "rock")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("Player 2 wins!");
                  document.getElementById("results").innerHTML= "Player 2 Wins!";
                  vm.p1Credits-= vm.bet;
                  vm.p2Credits+= vm.bet;
                  document.getElementById("p1Credits").innerHTML = "P1 Credits: " + vm.p1Credits;
                  document.getElementById("p2Credits").innerHTML = "P2 Credits: " + vm.p2Credits;
              }

              else if (vm.player1Turn == "scissors" && vm.player2Turn == "paper")
              {
                  console.log("Player 1 Picks: " + vm.player1Turn);
                  document.getElementById("player1").innerHTML = "Player 1 picks: " + vm.player1Turn;
                  console.log("Player 2 Picks: " + vm.player2Turn);
                  document.getElementById("player2").innerHTML = "Player 2 picks: " + vm.player2Turn;
                  console.log("Player 1 wins!");
                  document.getElementById("results").innerHTML= "Player 1 Wins!";
                  vm.p1Credits+= vm.bet;
                  vm.p2Credits-= vm.bet;
                  document.getElementById("p1Credits").innerHTML = "P1 Credits: " + vm.p1Credits;
                  document.getElementById("p2Credits").innerHTML = "P2 Credits: " + vm.p2Credits;
              }

              // Determines picture displayed for each player.
              if (vm.player1Turn == "rock")
              {
                  document.getElementById("player1Img").src = "https://i.imgur.com/DBcTJR3.jpg";
              }
              else if (vm.player1Turn == "paper")
              {
                  document.getElementById("player1Img").src = "https://i.imgur.com/pkX5uPU.jpg";
              }
              else if (vm.player1Turn == "scissors")
              {
                  document.getElementById("player1Img").src = "https://i.imgur.com/b7K9zyR.jpg";
              }

              if (vm.player2Turn == "rock")
              {
                  document.getElementById("player2Img").src = "https://i.imgur.com/DBcTJR3.jpg";
              }
              else if (vm.player2Turn == "paper")
              {
                  document.getElementById("player2Img").src = "https://i.imgur.com/pkX5uPU.jpg";
              }
              else if (vm.player2Turn == "scissors")
              {
                  document.getElementById("player2Img").src = "https://i.imgur.com/b7K9zyR.jpg";
              }
              vm.round++;
              document.getElementById("round").innerHTML = "Round: " + (vm.round - 1);
          }

      },

    // Used for Random button.
    RPSRand: function(player1Turn, player2Turn)
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
    RPS(player1Turn, player2Turn)
},

    // Used for Reset button.
    Reset: function()
    {
        var vm = this;
        vm.round = 1;
        vm.p1Credits = 150;
        vm.p2Credits = 150;
        document.getElementById("round").innerHTML = "Round: 1";
        document.getElementById("player1").innerHTML = "Player 1: ";
        document.getElementById("player2").innerHTML = "Player 2: ";
        document.getElementById("player1Img").src = "https://i.imgur.com/VFyOGbE.png";
        document.getElementById("player2Img").src = "https://i.imgur.com/VFyOGbE.png";
        document.getElementById("p1Credits").innerHTML = "P1 Credits: " + p1Credits;
        document.getElementById("p2Credits").innerHTML = "P2 Credits: " + p2Credits;
        document.getElementById("results").innerHTML= "";
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
</style>
