# hindu.calender
if you are hindu must wach this
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Hindu Festival Calendar with Countdown</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 900px;
      margin: 20px auto;
      padding: 0 10px;
    }
    h1 {
      text-align: center;
    }
    #countdown {
      font-size: 1.3em;
      font-weight: bold;
      margin-bottom: 15px;
      text-align: center;
      color: #333;
    }
    table {
      border-collapse: collapse;
      width: 100%;
    }
    caption {
      font-weight: bold;
      font-size: 1.5em;
      margin-bottom: 10px;
    }
    th, td {
      border: 1px solid #aaa;
      text-align: center;
      padding: 6px;
      height: 90px;
      vertical-align: top;
    }
    th {
      background-color: #eee;
    }
    td .date-number {
      font-weight: bold;
      display: inline-block;
      margin-bottom: 4px;
    }
    td.festival {
      background-color: #ffeb3b;
      border: 3px solid #fbc02d;
      border-radius: 8px;
      cursor: pointer;
      box-shadow: 0 0 10px #fbc02d;
      transition: background-color 0.3s ease;
    }
    td.festival:hover {
      background-color: #fff176;
      box-shadow: 0 0 15px #f9a825;
    }
    .tithi {
      font-size: 0.8em;
      color: #555;
      display: block;
      margin-top: 4px;
    }
    #details {
      margin-top: 20px;
      padding: 10px;
      border: 1px solid #ccc;
      display: none;
      background: #f9f9f9;
    }
    button {
      margin: 4px 6px 4px 0;
      padding: 6px 12px;
    }
    #watermark {
      position: fixed;
      bottom: 10px;
      right: 15px;
      opacity: 0.3;
      font-size: 14px;
      font-style: italic;
      pointer-events: none;
      z-index: 999;
      color: #333;
    }
    /* New bright top watermark */
    #top-watermark {
      position: fixed;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 16px;
      font-weight: bold;
      color: #ff5722;  /* bright orange */
      background-color: rgba(255, 255, 255, 0.9);
      padding: 6px 12px;
      border-radius: 6px;
      box-shadow: 0 0 8px #ff5722;
      z-index: 1000;
      pointer-events: none;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
  </style>
</head>
<body>

<h1>🪔 Hindu Festival Calendar with Countdown</h1>

<!-- Bright Top Watermark -->
<div id="top-watermark">Anmol Singh , IG=rajput_anmolsingh3</div>

<div id="countdown">Loading countdown...</div>

<div>
  <button id="prev-year"><< Previous Year</button>
  <button id="prev-month">< Previous Month</button>
  <button id="next-month">Next Month ></button>
  <button id="next-year">Next Year >></button>
</div>

<div id="calendar"></div>
<div id="details"></div>

<!-- Watermark -->
<div id="watermark">© Anmol Singh</div>

<script>
// Countdown Timer
function updateCountdown() {
  const now = new Date();
  const nextYear = now.getFullYear() + 1;
  const newYear = new Date(nextYear, 0, 1);
  const diff = newYear - now;

  const days = Math.floor(diff / (1000 * 60 * 60 * 24));
  const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
  const minutes = Math.floor((diff / (1000 * 60)) % 60);
  const seconds = Math.floor((diff / 1000) % 60);

  document.getElementById("countdown").textContent =
    `⏳ Time until ${nextYear}: ${days}d ${hours}h ${minutes}m ${seconds}s`;
}
setInterval(updateCountdown, 1000);
updateCountdown();

// Tithi calculations
function normalize(deg) {
  return ((deg % 360) + 360) % 360;
}
function sunLongitude(jd) {
  const d = jd - 2451545.0;
  const M = normalize(357.5291 + 0.98560028 * d);
  const C = 1.9148 * Math.sin(M * Math.PI / 180);
  return normalize(280.4665 + 0.98564736 * d + C);
}
function moonLongitude(jd) {
  const d = jd - 2451550.1;
  const N = normalize(125.1228 - 0.0529538083 * d);
  const M = normalize(115.3654 + 13.0649929509 * d);
  const lon = normalize(M + N);
  return lon;
}
function toJulianDate(date) {
  return date.getTime() / 86400000 + 2440587.5;
}
function calculateTithi(date) {
  const jd = toJulianDate(date);
  const sunLon = sunLongitude(jd);
  const moonLon = moonLongitude(jd);
  const diff = normalize(moonLon - sunLon);
  return Math.floor(diff / 12) + 1;
}
function getTithiName(tithiNum) {
  const names = [
    "Pratipada", "Dvitiya", "Tritiya", "Chaturthi", "Panchami",
    "Shashthi", "Saptami", "Ashtami", "Navami", "Dashami",
    "Ekadashi", "Dvadashi", "Trayodashi", "Chaturdashi", "Purnima / Amavasya"
  ];
  const phase = tithiNum <= 15 ? "Shukla" : "Krishna";
  const tithiName = names[(tithiNum - 1) % 15] + ` (${phase})`;

  let moon = "🌑";
  if (tithiNum >= 1 && tithiNum <= 3) moon = "🌑";
  else if (tithiNum >= 4 && tithiNum <= 7) moon = "🌒";
  else if (tithiNum >= 8 && tithiNum <= 10) moon = "🌓";
  else if (tithiNum >= 11 && tithiNum <= 14) moon = "🌔";
  else if (tithiNum === 15) moon = "🌕";
  else if (tithiNum >= 16 && tithiNum <= 19) moon = "🌖";
  else if (tithiNum >= 20 && tithiNum <= 23) moon = "🌗";
  else if (tithiNum >= 24 && tithiNum <= 27) moon = "🌘";
  else if (tithiNum >= 28 && tithiNum <= 30) moon = "🌑";

  return `${moon} ${tithiName}`;
}

// Festival data
const festivals = {
  "2025-01-14": {
    name: "Makar Sankranti",
    description: "Sun enters Capricorn – harvest festival.",
    muhurat: "7:30 AM – 10:30 AM"
  },
  "2025-01-26": {
    name: "Vasant Panchami",
    description: "Celebration dedicated to Goddess Saraswati.",
    muhurat: "9:00 AM – 12:00 PM"
  },
  "2025-03-14": {
    name: "Holi",
    description: "Festival of colors and joy.",
    muhurat: "9:00 AM – 12:00 PM"
  },
  "2025-03-13": {
    name: "Holika Dahan",
    description: "Bonfire night before Holi – burning negativity.",
    muhurat: "6:30 PM – 8:00 PM"
  },
  "2025-03-29": {
    name: "Rama Navami",
    description: "Birth of Lord Rama.",
    muhurat: "10:00 AM – 12:00 PM"
  },
  "2025-04-10": {
    name: "Hanuman Jayanti",
    description: "Birth of Lord Hanuman.",
    muhurat: "9:00 AM – 11:30 AM"
  },
  "2025-08-08": {
    name: "Raksha Bandhan",
    description: "Festival of brother-sister bond.",
    muhurat: "10:00 AM – 1:00 PM"
  },
  "2025-08-17": {
    name: "Krishna Janmashtami",
    description: "Birth of Lord Krishna.",
    muhurat: "12:00 AM – 12:30 AM (midnight)"
  },
  "2025-09-05": {
    name: "Ganesh Chaturthi",
    description: "Birth of Lord Ganesha.",
    muhurat: "11:00 AM – 1:00 PM"
  },
  "2025-10-02": {
    name: "Navratri Begins",
    description: "Start of 9 days dedicated to Goddess Durga.",
    muhurat: "6:00 AM – 8:00 AM"
  },
  "2025-10-11": {
    name: "Dussehra (Vijayadashami)",
    description: "Victory of good over evil – Lord Rama over Ravana.",
    muhurat: "2:00 PM – 4:00 PM"
  },
  "2025-10-23": {
    name: "Diwali",
    description: "Festival of lights, Lakshmi puja.",
    muhurat: "6:00 PM – 8:30 PM"
  },
  "2025-10-24": {
    name: "Govardhan Puja",
    description: "Worship of Govardhan mountain.",
    muhurat: "9:00 AM – 11:00 AM"
  },
  "2025-10-25": {
    name: "Bhai Dooj",
    description: "Brothers and sisters honor each other.",
    muhurat: "10:00 AM – 12:00 PM"
  },
  "2025-11-04": {
    name: "Karva Chauth",
    description: "Fasting by married women for husbands’ long life.",
    muhurat: "5:30 PM – 7:30 PM"
  },
  "2025-12-12": {
    name: "Gita Jayanti",
    description: "Day when Bhagavad Gita was revealed to Arjuna.",
    muhurat: "11:00 AM – 1:00 PM"
  }
};

// Calendar logic
let currentYear = new Date().getFullYear();
let currentMonth = new Date().getMonth();

function renderCalendar(year, month) {
  const calendar = document.getElementById("calendar");
  const details = document.getElementById("details");
  calendar.innerHTML = "";
  details.style.display = "none";

  const table = document.createElement("table");
  const caption = document.createElement("caption");
  caption.textContent = `${year} - ${new Date(year, month).toLocaleString("default", { month: "long" })}`;
  table.appendChild(caption);

  const weekdays = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
  const thead = document.createElement("thead");
  const row = document.createElement("tr");
  weekdays.forEach(d => {
    const th = document.createElement("th");
    th.textContent = d;
    row.appendChild(th);
  });
  thead.appendChild(row);
  table.appendChild(thead);

  const tbody = document.createElement("tbody");
  const firstDay = new Date(year, month, 1).getDay();
  const daysInMonth = new Date(year, month + 1, 0).getDate();

  let day = 1;
  for (let i = 0; i < 6 && day <= daysInMonth; i++) {
    const tr = document.createElement("tr");
    for (let j = 0; j < 7; j++) {
      const td = document.createElement("td");

      if ((i === 0 && j < firstDay) || day > daysInMonth) {
        td.textContent = "";
      } else {
        const fullDate = `${year}-${String(month + 1).padStart(2, "0")}-${String(day).padStart(2, "0")}`;
        const dt = new Date(year, month, day);
        const tithiNum = calculateTithi(dt);
        const tithiName = getTithiName(tithiNum);

        const dateNum = document.createElement("span");
        dateNum.className = "date-number";
        dateNum.textContent = day;
        td.appendChild(dateNum);

        const tithiSpan = document.createElement("span");
        tithiSpan.className = "tithi";
        tithiSpan.textContent = tithiName;
        td.appendChild(tithiSpan);

        if (festivals[fullDate]) {
          td.classList.add("festival");
          td.innerHTML += " 🌺";
          td.title = festivals[fullDate].name;
          td.onclick = () => {
            const fest = festivals[fullDate];
            details.style.display = "block";
            details.innerHTML = `
              <h3>${fest.name} (${fullDate})</h3>
              <p>${fest.description}</p>
              <strong>Subh Muhurat:</strong> ${fest.muhurat}
            `;
          };
        }
        day++;
      }
      tr.appendChild(td);
    }
    tbody.appendChild(tr);
  }
  table.appendChild(tbody);
  calendar.appendChild(table);
}
renderCalendar(currentYear, currentMonth);

// Navigation
document.getElementById("prev-month").onclick = () => {
  currentMonth--;
  if (currentMonth < 0) {
    currentMonth = 11;
    currentYear--;
  }
  renderCalendar(currentYear, currentMonth);
};
document.getElementById("next-month").onclick = () => {
  currentMonth++;
  if (currentMonth > 11) {
    currentMonth = 0;
    currentYear++;
  }
  renderCalendar(currentYear, currentMonth);
};
document.getElementById("prev-year").onclick = () => {
  currentYear--;
  renderCalendar(currentYear, currentMonth);
};
document.getElementById("next-year").onclick = () => {
  currentYear++;
  renderCalendar(currentYear, currentMonth);
};
</script>
</body>
</html>
