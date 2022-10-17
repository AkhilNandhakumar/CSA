---
title: Fetch of Backend API
layout: default
description: very cool epic frontend
permalink: /api/frontend
---
<!-- HTML table fragment for page -->
<table>
  <thead>
  <tr>
    <th>Recipe</th>
    <th>Yummy</th>
    <th>Yucky</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>

<!-- Script is layed out in a sequence (without a function) and will execute when page is loaded -->
<script>

  // prepare HTML defined "result" container for new output
  const resultContainer = document.getElementById("result");

  // keys for joke reactions
  const YUMMY = "yummy";
  const YUCKY = "yucky";

  // prepare fetch urls
  // const url = "https://flask.nighthawkcodingsociety.com/api/jokes";
  const url = "https://codersbackend.tk/api/recipes";
  const get_url = url +"/";
  const like_url = url + "/like/";  // haha reaction
  const jeer_url = url + "/jeer/";  // boohoo reaction

  // prepare fetch GET options

  const options = {
    method: 'GET', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
  };
  // prepare fetch PUT options, clones with JS Spread Operator (...)
  const put_options = {...options, method: 'PUT'}; // clones and replaces method

  // fetch the API
  fetch(get_url, options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          error('GET API response failure: ' + response.status);
          return;
      }
      // valid response will have JSON data
      response.json().then(data => {
          console.log(data);
          for (const row of data) {
            // make "tr element" for each "row of data"
            const tr = document.createElement("tr");
            
            // td for joke cell
            const recipe = document.createElement("td");
              recipe.innerHTML = row.id + ". " + row.recipe;  // add fetched data to innerHTML

            // td for haha cell with onclick actions
            const yummy = document.createElement("td");
              const yummy_but = document.createElement('button');
              yummy_but.id = YUMMY+row.id   // establishes a HAHA JS id for cell
              yummy_but.innerHTML = row.yummy;  // add fetched "haha count" to innerHTML
              yummy_but.onclick = function () {
                // onclick function call with "like parameters"
                reaction(YUMMY, like_url+row.id, yummy_but.id);  
              };
              yummy.appendChild(yummy_but);  // add "haha button" to haha cell

            // td for boohoo cell with onclick actions
            const yucky = document.createElement("td");
              const yucky_but = document.createElement('button');
              yucky_but.id = YUCKY+row.id  // establishes a BOOHOO JS id for cell
              yucky_but.innerHTML = row.yucky;  // add fetched "boohoo count" to innerHTML
              yucky_but.onclick = function () {
                // onclick function call with "jeer parameters"
                reaction(YUCKY, jeer_url+row.id, yucky_but.id);  
              };
              yucky.appendChild(yucky_but);  // add "boohoo button" to boohoo cell
             
            // this builds ALL td's (cells) into tr (row) element
            tr.appendChild(recipe);
            tr.appendChild(yummy);
            tr.appendChild(yucky);

            // this adds all the tr (row) work above to the HTML "result" container
            resultContainer.appendChild(tr);
          }
      })
  })
  // catch fetch errors (ie Nginx ACCESS to server blocked)
  .catch(err => {
    error(err + " " + get_url);
  });

  // Reaction function to likes or jeers user actions
  function reaction(type, put_url, elemID) {

    // fetch the API
    fetch(put_url, put_options)
    // response is a RESTful "promise" on any successful fetch
    .then(response => {
      // check for response errors
      if (response.status !== 200) {
          error("PUT API response failure: " + response.status)
          return;  // api failure
      }
      // valid response will have JSON data
      response.json().then(data => {
          console.log(data);
          // Likes or Jeers updated/incremented
          if (type === YUMMY) // like data element
            document.getElementById(elemID).innerHTML = data.yummy;  // fetched haha data assigned to haha Document Object Model (DOM)
          else if (type === YUCKY) // jeer data element
            document.getElementById(elemID).innerHTML = data.yucky;  // fetched boohoo data assigned to boohoo Document Object Model (DOM)
          else
            error("unknown type: " + type);  // should never occur
      })
    })
    // catch fetch errors (ie Nginx ACCESS to server blocked)
    .catch(err => {
      error(err + " " + put_url);
    });
    
  }

  // Something went wrong with actions or responses
  function error(err) {
    // log as Error in console
    console.error(err);
    // append error to resultContainer
    const tr = document.createElement("tr");
    const td = document.createElement("td");
    td.innerHTML = err;
    tr.appendChild(td);
    resultContainer.appendChild(tr);
  }

</script>
