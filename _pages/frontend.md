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
    <!-- javascript generated data -->
  </tbody>
</table>

<!-- Script is layed out in a sequence (no function) and will execute when page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

   // define a function to hold data for each team member
    function Player(name, nationality, rank) {
        this.name = name;
        this.nationality = nationality;
        this.rank = rank;
    }

    // define a JSON conversion "method" associated with Student
    Player.prototype.toJSON = function() {
        const obj = {name: this.name, nationality: this.nationality, rank: this.rank};
        const json = JSON.stringify(obj);  // json/string is useful when passing data on internet
        return json;
    }

    // define array of students
    var list = [ 
        new Player( "Spain", "#1", "Carlos Alcaraz Garfia"),
        new Player( "Russia", "#4", "Danil Medvedev"),
        new Player( "Norway", "#2", "Casper Ruud"),
        new Player( "Spain", "#3", "Rafael Nadal"),
        new Player( "USA", "#2885893", "Akhil Nandhakumar"),
        new Player( "USA", "GOAT", "Mr. Mort"),
        new Player( "Test", "#10", "Test")
    ];
  
    // define a classroom and build Classroom objects and json
    function PlayerClass(players){
        // add each Student to Classroom
        this.PlayerClass = players;
        // build json/string format of Classroom
        this.json = [];
        this.PlayerClass.forEach(players => this.json.push(players.toJSON()));
    }
  
    // make a CompSci classroom
    playerlist = new PlayerClass(list);

    for (const row of playerlist.PlayerClass) {
        // tr for each row
        const tr = document.createElement("tr");
        // td for each column
        const name = document.createElement("td");
        const id = document.createElement("td");
        const rank = document.createElement("td");
        // data is specific to the API
        name.innerHTML = row.name;
        id.innerHTML = row.nationality; 
        rank.innerHTML = row.rank; 
        
        // this build td's into tr
        tr.appendChild(rank);
        tr.appendChild(name);
        tr.appendChild(id);
        
        // add HTML to container
        resultContainer.appendChild(tr);
    }
</script>
