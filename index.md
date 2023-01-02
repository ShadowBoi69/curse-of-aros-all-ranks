---
# Curse of Aros - All Ranks

<form onsubmit="search()">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" value=""/><br/>
  <input type="checkbox" id="lw" name="lw"/>&nbsp;<label for="lw">Lone Wolf</label><br/>
  <input type="submit" value="Find"/>
</form>
<table id="table">
  <tr>
    <th>Name</th>
    <th>Rank</th>
    <th>XP</th>
    <th>Page</th>
  </tr>
  <tr>
    <td class="left">Overall</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Melee</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Magic</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Mining</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Smithing</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Woodcutting</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Crafting</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Fishing</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Cooking</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Tailoring</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

<p>Made by <a href="https://kaue.me">https://kaue.me</a></p>

<p>Data from <a href="https://curseofaros.com">https://curseofaros.com</a></p>

<style>

table {
  padding: 2px;
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 2px;
}

td {
  text-align: right;
}

.left {
  text-align: left;
}

</style>

<script>

const MAX_PAGE = 100000;
const DEFAULT_NAME = "Bot";
const DEFAULT_LW = "0";
const INDEX = {
  "rank": 1,
  "xp": 2,
  "page": 3,
};
const skills = ["overall", "melee", "magic", "mining", "smithing", "woodcutting", "crafting", "fishing", "cooking", "tailoring"];

function byId(id) {
  return document.getElementById(id);
}

function setCell(row, index, value) {
  row.cells[INDEX[index]].innerHTML = value;
}

let search = async () => {
  const name = byId("name").value;
  const table = byId("table");
  const lw = byId("lw").checked ? "1" : "0";

  for (let i = 0; i < skills.length; i++) {
    const row = table.rows[i + 1];
    let suffix = skills[i];
    if (suffix != "") {
      suffix = "-" + suffix;
    }
    let found = false;
    let rank = 0;
    for (let page = 0; page < MAX_PAGE; page++) {
      setCell(row, "page", page + 1);
      const url = "https://www.curseofaros.com/highscores" + suffix + ".json?p=" + page + "&lw=" + lw;
      const response = await fetch(url);
      const json = await response.json();
      for (let i = 0; i < json.length; i++) {
        rank++;
        const item = json[i];
        if (item.name != name) {
          continue
        }
        setCell(row, "rank", rank);
        setCell(row, "xp", item.xp);
        found = true;
        break
      }
      if (found) {
        break;
      }
    }
    if (!found) {
      console.log("name", name, "not found");
    }
  }
};

function main() {
  const queryString = window.location.search;
  const urlParams = new URLSearchParams(queryString);

  let name = DEFAULT_NAME;
  if (urlParams.has("name")) {
    got = urlParams.get("name");
    if (got != "") {
      name = got;
    }
  }
  byId("name").value = name;

  let lw = DEFAULT_LW;
  if (urlParams.has("lw")) {
    got = urlParams.get("lw");
    if (got != "") {
      lw = got;
    }
  }
  byId("lw").checked = lw == "on";

  search();
}

main();

</script>
