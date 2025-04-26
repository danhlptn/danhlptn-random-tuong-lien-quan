<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Random Tướng Liên Quân</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-[#12002f] min-h-screen flex flex-col items-center p-4">

  <!-- Title -->
  <h1 class="text-3xl md:text-5xl text-white font-bold mb-6 mt-4">🎮 Random Tướng Liên Quân 🎮</h1>

  <!-- Filter + Search -->
  <div class="flex flex-wrap justify-center gap-4 mb-6">
    <select id="role" class="p-2 rounded-md bg-gray-700 text-white">
      <option value="all">Tất cả</option>
      <option value="dau-si">Đấu Sĩ</option>
      <option value="phap-su">Pháp Sư</option>
      <option value="xa-thu">Xạ Thủ</option>
      <option value="sat-thu">Sát Thủ</option>
      <option value="do-don">Đỡ Đòn</option>
      <option value="tro-thu">Trợ Thủ</option>
    </select>

    <input id="search" type="text" placeholder="Tìm kiếm tướng..." class="p-2 rounded-md bg-gray-700 text-white" oninput="searchTuong()">
    
    <button onclick="randomTuong()" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-lg">
      Random
    </button>
    <button onclick="randomFullTeam()" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-lg">
      Random Full Team
    </button>
  </div>

  <!-- Kết quả random -->
  <div id="result" class="text-center text-white text-2xl my-4"></div>

  <!-- Danh sách tướng -->
  <div id="list" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-6">
    <!-- Các tướng sẽ hiện ở đây -->
  </div>

<script>
// Dữ liệu tướng
const tuongList = {
  "dau-si": ["Zuka", "Veres", "Richter", "Rourke", "Airi", "Arthur", "Superman", "Zephys", "Triệu Vân", "Omen", "Maloch", "Skud", "Tachi", "Lữ Bố", "Kil’Groth", "Errol", "Wiro", "Florentino", "Qi", "Amily", "Allain", "Roxie", "Volkath", "Ryoma", "Astrid", "Yan", "Wonder Woman", "Taara", "Dextra", "Yena", "Arduin", "Bijan", "Charlotte"],
  "phap-su": ["Dirak", "Raz", "Preyta", "Natalya", "Ignis", "Lauriel", "Điêu Thuyền", "Veera", "Marja", "Tulen", "Krixi", "Mganga", "Liliana", "Kahlii", "Yue", "Annette", "Sephera", "Ilumia", "Ishar", "Zata", "Jinna", "Aleister", "Azzen’Ka", "D’Arcy", "Lorion", "Sephora", "Iginis"],
  "xa-thu": ["Valhein", "Violet", "Yorn", "Fennik", "Slimz", "Joker", "Tel'Annas", "Moren", "Lindis", "Wisp", "Elsu", "Hayate", "Capheny", "Celica", "Eland’orr", "Laville", "Thorne", "Bright", "Heino"],
  "sat-thu": ["Nakroth", "Murad", "Quillen", "Airi", "Butterfly", "Zill", "Kriknak", "Ngộ Không", "Keera", "Enzo", "Yan", "Volkath", "Ryoma", "Astrid", "Qi", "Bijan", "Paine", "Bright", "Allain", "Zata", "Raz", "Dirak", "Tachi", "Errol", "Zephys", "Superman"],
  "do-don": ["Toro", "Gildur", "Grakk", "Sephera", "Helen", "Ishar", "Krizzix", "Chaugnar", "Annette", "Baldum", "Omega", "Dolia", "Arum", "Lumburr", "Skud", "Maloch", "Wiro", "Roxie", "Dextra", "Arduin", "Y’bneth", "Cresht", "Thane", "Taara"],
  "tro-thu": ["Alice", "Annette", "Sephera", "Krizzix", "Chaugnar", "Baldum", "Omega", "Dolia", "Arum", "Lumburr", "Teemee", "Zip", "Rouie", "Helen", "Ishar", "Payna", "Y’bneth", "Cresht", "Thane"]
};
const tatCaTuong = Object.values(tuongList).flat();

function renderTuong(list) {
  const listDiv = document.getElementById('list');
  listDiv.innerHTML = '';

  list.forEach(name => {
    const item = document.createElement('div');
    item.className = "bg-[#1e1e3f] p-4 rounded-xl text-center text-white hover:bg-[#2c2c5f] cursor-pointer transition";

    item.innerHTML = `
      <div class="text-5xl mb-2">🎯</div>
      <div class="text-sm font-bold">${name}</div>
    `;

    listDiv.appendChild(item);
  });
}

function randomTuong() {
  const role = document.getElementById('role').value;
  const list = role === 'all' ? tatCaTuong : tuongList[role];
  const randomHero = list[Math.floor(Math.random() * list.length)];

  document.getElementById('result').innerHTML = `🎯 Bạn random được: <strong>${randomHero}</strong>`;
}

function randomFullTeam() {
  const role = document.getElementById('role').value;
  const list = role === 'all' ? [...tatCaTuong] : [...tuongList[role]];

  const team = [];
  while (team.length < 5 && list.length > 0) {
    const randomIndex = Math.floor(Math.random() * list.length);
    team.push(list[randomIndex]);
    list.splice(randomIndex, 1);
  }

  document.getElementById('result').innerHTML = `
    🛡️ Đội hình: <br>
    ${team.map(name => `<strong>${name}</strong>`).join('<br>')}
  `;
}

function searchTuong() {
  const keyword = document.getElementById('search').value.toLowerCase();
  const role = document.getElementById('role').value;
  const list = role === 'all' ? tatCaTuong : tuongList[role];

  const filtered = list.filter(name => name.toLowerCase().includes(keyword));
  renderTuong(filtered);
}

// Mặc định render tất cả
renderTuong(tatCaTuong);
</script>

</body>
</html>
