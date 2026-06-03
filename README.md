<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Serial Catalog</title>

<style>
body {
  font-family: Arial;
  background: #111;
  color: white;
  margin: 0;
}

input {
  width: 90%;
  margin: 15px;
  padding: 10px;
  border-radius: 8px;
  border: none;
}

.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 15px;
  padding: 15px;
}

.card {
  background: #222;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  transition: 0.2s;
}

.card:hover {
  transform: scale(1.03);
}

.card img {
  width: 100%;
  height: 220px;
  object-fit: cover;
}

.card h3 {
  margin: 8px;
  font-size: 14px;
}

.card p {
  margin: 0 8px 10px;
  font-size: 12px;
  color: #aaa;
}
</style>
</head>

<body>

<input type="text" id="search" placeholder="🔍 Поиск сериалов...">

<div class="container" id="list"></div>

<script>
const SHEET_URL = "https://docs.google.com/spreadsheets/d/1-mtEef4gMgTEL0HfgWGGTQON8jkXSrFXgEGmqt-0ta4/export?format=csv";

let data = [];

fetch(SHEET_URL)
.then(res => res.text())
.then(text => {
    const rows = text.split("\n").slice(1);

    data = rows.map(r => {
        const cols = r.split(",");

        return {
            poster: cols[0],
            title: cols[1],
            link: cols[2],
            desc: cols[3]
        };
    });

    render(data);
});

function render(list) {
    const container = document.getElementById("list");
    container.innerHTML = "";

    list.forEach(item => {
        if (!item.title) return;

        container.innerHTML += `
        <div class="card" onclick="window.open('${item.link}','_blank')">
            <img src="${item.poster}">
            <h3>${item.title}</h3>
            <p>${item.desc || ''}</p>
        </div>`;
    });
}

document.getElementById("search").addEventListener("input", e => {
    const q = e.target.value.toLowerCase();

    render(data.filter(i =>
        (i.title || "").toLowerCase().includes(q)
    ));
});
</script>

</body>
</html>
