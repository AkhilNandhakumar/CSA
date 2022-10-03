---
layout: page
title: Frontend Test
permalink: /frontend/
toc: true
---

<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Name</th>
    <th>Nationality</th>
    <th>Rank</th>
  </tr>
  </thead>
  <tbody id="result">
  </tbody>
</table>

<br>



<script>
  const resultContainer = document.getElementById("result");

   // function holds data for players
    function Player(name, nationality, rank) {
        this.name = name;
        this.nationality = nationality;
        this.rank = rank;
    }

    // json conversion function
    Player.prototype.toJSON = function() {
        const obj = {name: this.name, nationality: this.nationality, rank: this.rank};
        const json = JSON.stringify(obj);
        return json;
    }

    // list of players
    var list = [ 
        new Player( "Spain", "#1", "Carlos Alcaraz Garfia"),
        new Player( "Russia", "#4", "Danil Medvedev"),
        new Player( "Norway", "#2", "Casper Ruud"),
        new Player( "Spain", "#3", "Rafael Nadal"),
        new Player( "USA", "#2885893", "Akhil Nandhakumar"),
        new Player( "USA", "GOAT", "Mr. Mort"),
        new Player( "Test", "#10", "Test")
    ];
  
    function PlayerClass(players){
        this.PlayerClass = players;
        this.json = [];
        this.PlayerClass.forEach(players => this.json.push(players.toJSON()));
    }
  
    // creates playerlist object
    playerlist = new PlayerClass(list);

// javascript variables and methods to build html using previous data

    for (const row of playerlist.PlayerClass) {

        const tr = document.createElement("tr");

        const name = document.createElement("td");
        const id = document.createElement("td");
        const rank = document.createElement("td");

        name.innerHTML = row.name;
        id.innerHTML = row.nationality; 
        rank.innerHTML = row.rank; 
        

        tr.appendChild(rank);
        tr.appendChild(name);
        tr.appendChild(id);
        

        resultContainer.appendChild(tr);
    }

// API ------------------------------------------------
    
async function api() {

const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': 'af654d789amshce4b35d071f3bd2p1c0cc8jsn8db3aa6a8acc',
		'X-RapidAPI-Host': 'ultimate-tennis1.p.rapidapi.com'
	}
};

  return fetch('https://ultimate-tennis1.p.rapidapi.com/live_scores', options).then(response => response.json())
}

const x  = await api();
console.log(x);

</script>
