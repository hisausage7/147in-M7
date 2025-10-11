<!DOCTYPE html>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>147測驗｜單一檔案版</title>
    <style>
      /* 回首頁浮動按鈕 */
      #homeBtn {
        position: absolute;
        top: 100px; /* 深色模式按鈕在 20px，這裡放在它下方 */
        right: 20px;
        padding: 10px 20px;
        background: #28a745;
        color: #fff;
        border: none;
        border-radius: 8px;
        text-decoration: none;
        font-size: 1em;
        box-shadow: 0 4px 12px rgba(0,0,0,.15);
        transition: background .3s, transform .2s, box-shadow .2s;
        z-index: 1000;
      }
      #homeBtn:hover { background: #218838; transform: translateY(-2px); box-shadow: 0 6px 16px rgba(0,0,0,.25); }
      body.dark #homeBtn { background: #2e7d32; }
      body.dark #homeBtn:hover { background: #25652a; }

      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 40px;
        font-size: 1.6em;
        background: #f0f4f8;
        color: #000;
        transition: background-color 0.5s, color 0.5s;
      }
      body.dark { background: #121212; color: #fff; }
      #container {
        max-width: 1200px; margin: auto; background: #fff; padding: 40px;
        border-radius: 20px; box-shadow: 0 0 10px rgba(0,0,0,.1);
        transition: background-color .5s, color .5s;
      }
      body.dark #container { background: #1e1e1e; }
      .hidden { display: none; }
      h1, h2 { text-align: center; font-size: 2.4em; }
      #rules { background: #e9ecef; padding: 20px; margin-bottom: 40px; border-radius: 10px; font-size: 1.2em; }
      body.dark #rules { background: #333; }
      .btn {
        background: #007bff; color: #fff; border: none; padding: 20px 40px;
        margin: 10px; border-radius: 10px; cursor: pointer; font-size: 1.2em;
        transition: background-color .3s;
      }
      .btn:hover { background: #0056b3; }
      #leaveBtn { background: #dc3545; }
      #progress, #timer { font-weight: bold; font-size: 1.4em; }
      .progress-container { background: #ddd; height: 10px; border-radius: 5px; margin-top: 20px; overflow: hidden; }
      body.dark .progress-container { background: #555; }
      .progress-bar { height: 100%; width: 0; background: #007bff; transition: width .6s ease; }
      .question { margin: 40px 0 20px; font-size: 1.8em; }
      .options label { display: block; margin-bottom: 16px; font-size: 1.4em; }
      table { width: 100%; border-collapse: collapse; margin-top: 40px; font-size: 1.2em; }
      th, td { border: 1px solid #ccc; padding: 16px; text-align: left; }
      tr.wrong { background-color: #ffe6e6; }
      body.dark tr.wrong { background-color: #661111; }
      #darkModeToggle {
        position: absolute; top: 20px; right: 20px; padding: 10px 20px;
        background: #333; color: #fff; border: none; border-radius: 8px; cursor: pointer; font-size: 1em;
        transition: background .3s;
      }
      #darkModeToggle:hover { background: #555; }
    </style>
  </head>
  <body>
    <!-- 吃飽太閒做的網站，能幫到你我覺得很開心 -->
    <button id="darkModeToggle">深色模式 / Dark Mode</button>
    <a id="homeBtn" href="https://hisausage7.github.io/147test/" title="回首頁">🏠 回首頁</a>

    <div id="container">
      <div id="welcome">
        <h1>147測驗M7-入學考</h1>
        <div id="rules">
          <p><strong>考試注意事項 / Exam Rules:</strong></p>
          <p>1. 請輸入姓名後才能開始作答。 / You must enter your name to start the quiz.</p>
          <p>2. 考試限時80分鐘，自動倒數。 / The quiz is timed for 80 minutes, countdown starts immediately.</p>
          <p>3. 作答途中可隨時點擊「離開考試」提前結束。 / You can click "Leave Quiz" anytime to finish early.</p>
          <p>4. 完成後會自動顯示所有答題結果與成績。 / Results and scores will be displayed after completion.</p>
          <p>5. 答對題目顯示O，答錯題目顯示X。 / Correct answers will show O, incorrect answers will show X.</p>
          <p>6. 測驗過程為亂序出題。 / The test process was chaotic and disordered in setting questions.</p>
          <p>!版權及源代碼所有-航機系008沈崑宸!</p>
          <p>!僅作為自我測驗使用!</p>
        </div>
        <input type="text" id="nameInput" placeholder="輸入姓名 / Enter your name"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1.4em;" />
        <input type="number" id="questionLimit" placeholder="輸入題數,至多50題 / Enter number of questions"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1.4em;" />
        <button id="startBtn" class="btn">開始測驗 / Start Quiz</button>
      </div>

      <div id="quiz" class="hidden">
        <div>
          <span id="welcomeName"></span>
          <span id="timer" style="float:right">80:00</span>
        </div>
        <div id="progress">題數: <span id="current">0</span> / <span id="total">0</span></div>
        <div class="progress-container"><div id="progressBar" class="progress-bar"></div></div>
        <div class="question" id="questionText"></div>
        <div class="options" id="options"></div>
        <button id="prevBtn" class="btn" style="background:#6c757d">上一題 / Previous</button>
        <button id="leaveBtn" class="btn">離開考試 / Leave Quiz</button>
      </div>

      <div id="results" class="hidden">
        <h2>測驗結果 / Results</h2>
        <div>
          <button id="retryBtn" class="btn">重新開始 / Retry</button>
          <button id="showWrongBtn" class="btn" style="background:#ffc107">只看錯題 / Wrong Only</button>
          <button id="showAllBtn" class="btn">看全部結果 / Show All</button>
        </div>
        <table>
          <thead>
            <tr><th>題目</th><th>您的答案</th><th>正確答案</th><th>結果</th></tr>
          </thead>
          <tbody id="resultsBody"></tbody>
        </table>
        <div id="scoreSummary" style="text-align:center;margin-top:20px;font-size:1.2em"></div>
      </div>
    </div>

    <script>
      document.addEventListener("DOMContentLoaded", function () {
        const questions = [
  {
    question: "An aircraft should not be refueled when（下列何種狀況下飛機不可加油？）",
    options: ["A. the APU is running.", "B. navigation and landing light in operation.", "C. within 10 meters (30 feet) of radar operating."],
    answer: "C"
  },
  {
    question: "Additives that speed up a paint's cure time are referred to as（加速塗料固化之添加劑是）",
    options: ["A. Hardeners（固化劑）", "B. Retarders（緩凝劑）", "C. Lacquer（亮光漆）"],
    answer: "A"
  },
  {
    question: "What is the torque value of a nut to use a 6 in torque wrench setting 150 in-lb with a 3 in extension bar?（延長桿3英吋、扭力扳手長6英吋、設定150吋磅時實際扭力為？）",
    options: ["A. 205 in-lb", "B. 215 in-lb", "C. 225 in-lb"],
    answer: "C"
  },
  {
    question: "What type of meter is used for measuring very high values of resistance?（用於測量非常高電阻值的電表是？）",
    options: ["A. Mega-ohmmeter（百萬歐姆表）", "B. Shunt-type ohmmeter（分流歐姆表）", "C. Multimeter（三用電表）"],
    answer: "A"
  },
  {
    question: "The Air Transport Association (ATA) Specification No.100...（ATA 100 規範之功能說明）",
    options: ["A. Neither (1) nor (2) is true", "B. Both (1) and (2) are true", "C. Only (1) is true"],
    answer: "B"
  },
  {
    question: "The width of a visible outline on a drawing is（工程圖上可見輪廓線寬度為）",
    options: ["A. 0.3 mm", "B. 0.7 mm", "C. 0.5 mm"],
    answer: "B"
  },
  {
    question: "A drawing in which all of the parts are brought together as an assembly is called（所有零件組裝在一起之工程圖稱為）",
    options: ["A. Sectional drawing（剖面圖）", "B. Detail drawing（細部圖）", "C. Installation drawing（安裝圖）"],
    answer: "C"
  },
  {
    question: "What is the allowable manufacturing tolerance for a bushing...（可接受製造誤差為）",
    options: ["A. 0.0028", "B. 1.0650", "C. 1.0647"],
    answer: "A"
  },
  {
    question: "What does GA stand for on a drawing?（工程圖上 GA 代表）",
    options: ["A. General Assembly", "B. General Arrangement", "C. Gradient Axis"],
    answer: "B"
  },
  {
    question: "What is the maximum bow allowed in a strut?（支柱允許的最大弓度為）",
    options: ["A. 1 in 200", "B. 1 in 500", "C. 1 in 600"],
    answer: "C"
  },
  {
    question: "Which process obtains the necessary electrical conductivity between metallic parts?（飛機金屬組件間導電性測試為）",
    options: ["A. Grounding test", "B. Insulation test", "C. Bonding test"],
    answer: "C"
  },
  {
    question: "A repair has a double riveted joint. The shear strength would be（雙重鉚接之剪力強度為）",
    options: ["A. 125%", "B. 75%", "C. 100%"],
    answer: "B"
  },
  {
    question: "The minimum acceptable dimensions for a formed end of a rivet are（鉚釘柄成形頭最小尺寸為）",
    options: ["A. 0.65D × 1.0D", "B. 0.5D × 1.5D", "C. 0.65D × 1.5D"],
    answer: "B"
  },
  {
    question: "A scratch or nick in aluminum tubing can be repaired by burnishing if it does not（鋁管刮痕可修復條件）",
    options: ["A. Appear in heel of bend", "B. Exceed 5% of diameter", "C. Exceed 10% of diameter"],
    answer: "A"
  },
  {
    question: "What is the full designation for an aluminum nut for a flared fitting 5/8 in OD line?（喇叭口接頭鋁螺帽代號）",
    options: ["A. AN818D5", "B. AN818D10", "C. AN818D16"],
    answer: "B"
  },
  {
    question: "The length of a spring if NOT under load is called（無負載彈簧長度稱為）",
    options: ["A. Free length", "B. Block length", "C. Wire length"],
    answer: "A"
  },
  {
    question: "Thrust bearings transmit（止推軸承傳遞）",
    options: ["A. Thrust loads, limiting axial movement", "B. Radial loads, limiting axial movement", "C. Thrust loads, limiting radial movement"],
    answer: "A"
  },
  {
    question: "Backlash is a type of wear associated with（齒隙磨損與何有關）",
    options: ["A. Gears", "B. Rivets", "C. Bearings"],
    answer: "A"
  },
  {
    question: "When using a tension meter on large aircraft, consider（大型飛機鋼索張力量測需注意）",
    options: ["A. Temperature", "B. Moisture", "C. Altitude"],
    answer: "A"
  },
  {
    question: "The sharpest bend without weakening the part is called（不削弱金屬之最小彎曲半徑稱為）",
    options: ["A. Max radius", "B. Min radius", "C. Bend allowance"],
    answer: "B"
  },
  {
    question: "When using a megger to test insulation, disconnect capacitive filters because（使用絕緣測試器時應斷開濾波器因）",
    options: ["A. Prevent damage to megger", "B. Avoid false readings", "C. Prevent filter damage"],
    answer: "B"
  },
  {
    question: "What type of diagram is used to explain principle of operation?（用於解釋原理之圖）",
    options: ["A. Block diagram", "B. Pictorial diagram", "C. System schematic diagram"],
    answer: "C"
  },
  {
    question: "A line used to show an invisible edge is a（表示隱藏邊的線為）",
    options: ["A. Phantom line", "B. Break line", "C. Hidden line"],
    answer: "C"
  },
  {
    question: "If there is positive allowance between smallest hole and largest shaft, the fit is（正裕度裝配稱為）",
    options: ["A. Transition fit", "B. Clearance fit", "C. Interference fit"],
    answer: "B"
  },
  {
    question: "The length of a blended corrosion repair should be（腐蝕修磨長度不得小於）",
    options: ["A. 10× depth", "B. 20× depth", "C. 5× depth"],
    answer: "B"
  },
  {
    question: "When coaxial cable is installed, secure it every（同軸電纜固定間距為）",
    options: ["A. 1 ft", "B. Sag point", "C. 2 ft"],
    answer: "A"
  },
  {
    question: "Teflon hoses may be used for（TFE/Teflon軟管可用於）",
    options: ["A. Fuel", "B. Hydraulic", "C. All fluids"],
    answer: "C"
  },
  {
    question: "The coils in a coil spring that move under load are（在負載下活動之彈簧圈為）",
    options: ["A. Coil surge", "B. End coils", "C. Active coils"],
    answer: "C"
  },
  {
    question: "A tapered roller bearing is designed to take（錐形滾柱軸承承受）",
    options: ["A. Radial loads only", "B. Both radial and axial", "C. Axial only"],
    answer: "B"
  },
  {
    question: "Control chains should be fitted（飛機控制鏈安裝時）",
    options: ["A. With minimum slack", "B. Easily removable", "C. As loose as possible"],
    answer: "A"
  },
  {
    question: "Completed terminal sleeves should be checked with（鬆緊套檢查工具為）",
    options: ["A. Go-no-go gauge", "B. Surface gage", "C. Depth gage"],
    answer: "A"
  },
  {
    question: "The skin on an aircraft is normally made of（飛機蒙皮材料為）",
    options: ["A. 2024 aluminum alloy", "B. 7075 aluminum alloy", "C. 2117 aluminum alloy"],
    answer: "A"
  },
  {
    question: "In bending, the neutral axis is（中性軸定義）",
    options: ["A. Equal tension/compression line", "B. Inside radius line", "C. Same as setback"],
    answer: "A"
  },
  {
    question: "Maintenance records must be kept at least（維修紀錄保留期限）",
    options: ["A. 1 year", "B. 2 years", "C. 5 years"],
    answer: "B"
  },
  {
    question: "Information for Removal and Installation can be found in page（維修手冊拆裝章節頁碼）",
    options: ["A. 201-300", "B. 301-400", "C. 401-500"],
    answer: "C"
  },
  {
    question: "In Dead Weight Tester, piston area 0.25 in², weight 5 lb, pressure is（靜重試驗壓力為）",
    options: ["A. 1.25 psi", "B. 20 psi", "C. 200 psi"],
    answer: "B"
  },
  {
    question: "When attaching a terminal to cable, joint strength should be（電纜端子接合強度應為）",
    options: ["A. 2× tensile strength", "B. ≥ tensile strength", "C. Soft tube covered"],
    answer: "B"
  },
  {
    question: "Complete designation for 90° elbow joining 3/8 in OD tubes（九十度彎頭接頭代號）",
    options: ["A. AN821-3", "B. AN821-6", "C. AN821-9"],
    answer: "B"
  },
  {
    question: "After cleaning a bearing, it should be（軸承清洗後應）",
    options: ["A. Air dry", "B. Warm dry air", "C. Lint-free rag"],
    answer: "B"
  },
  {
    question: "How to check a chain for elongation?（檢查鏈條伸長方式）",
    options: ["A. Hang and sight", "B. Adjust ends", "C. Lay flat and tension measure"],
    answer: "C"
  },
  {
    question: "Pulley alignment angle shall not exceed（滑輪校準夾角不得超過）",
    options: ["A. 1°", "B. 2°", "C. 3°"],
    answer: "B"
  },
  {
    question: "What protects a cable through a bulkhead hole?（鋼索穿隔艙保護物）",
    options: ["A. Seal", "B. Grommet", "C. Compound"],
    answer: "B"
  },
  {
    question: "When drilling stainless steel, drill angle and speed should be（鑽不鏽鋼角度及轉速）",
    options: ["A. 90°, low speed", "B. 90°, high speed", "C. 140°, low speed"],
    answer: "C"
  },
  {
    question: "Prior to aluminum alloy bonding we use（鋁合金膠合前使用）",
    options: ["A. Alkaline etch", "B. Acid etch", "C. Solvent wipe"],
    answer: "B"
  },
  {
    question: "After honeycomb repair curing, use which NDT method?（蜂巢結構修復檢查方法）",
    options: ["A. Eddy current", "B. Metallic ring", "C. Ultrasonic"],
    answer: "B"
  },
  {
    question: "When towing an aircraft（拖機時應）",
    options: ["A. Tow backward", "B. Nosewheel free swivel", "C. Deflate all struts"],
    answer: "B"
  },
  {
    question: "Aircraft is airworthy when（判斷適航條件）",
    options: ["A. (1)(2)", "B. (1)(3)", "C. (2)(3)"],
    answer: "C"
  },
  {
    question: "When testing a fuel metering unit（檢測燃油計量單元方法）",
    options: ["A. In series", "B. Disconnected", "C. In parallel"],
    answer: "A"
  },
  {
    question: "Use of triangular files is limited to internal angles less than（銼刀限用內角）",
    options: ["A. 120°", "B. 100°", "C. 90°"],
    answer: "C"
  },
  {
    question: "2 micron meters equals（2微米等於）",
    options: ["A. 0.002\"", "B. 0.002 mm", "C. 0.0002\""],
    answer: "B"
  }
];



        

        function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
          }
          return array;
        }

        let shuffledQuestions;
        let current = 0, total = 0, timer = 80 * 60, interval, answers = [];

        function fmtTime(s) {
          const m = Math.floor(s / 60).toString().padStart(2, "0");
          const sec = (s % 60).toString().padStart(2, "0");
          return m + ":" + sec;
        }

        const startBtn = document.getElementById("startBtn");
        const prevBtn = document.getElementById("prevBtn");
        const leaveBtn = document.getElementById("leaveBtn");
        const retryBtn = document.getElementById("retryBtn");
        const showWrongBtn = document.getElementById("showWrongBtn");
        const showAllBtn = document.getElementById("showAllBtn");
        const darkModeToggle = document.getElementById("darkModeToggle");
        const progressBar = document.getElementById("progressBar");

        startBtn.addEventListener("click", () => {
          const n = document.getElementById("nameInput").value.trim();
          const qLimit = parseInt(document.getElementById("questionLimit").value);

          if (!n) return alert("請輸入姓名 / Enter your name");
          if (!qLimit || qLimit <= 0) return alert("請輸入要作答的題數,最多5題 / Enter number of questions");

          shuffledQuestions = shuffle([...questions]).slice(0, Math.min(qLimit, questions.length));
          total = shuffledQuestions.length;
          current = 0; answers = []; timer = 80 * 60;

          document.getElementById("welcome").classList.add("hidden");
          document.getElementById("quiz").classList.remove("hidden");
          document.getElementById("welcomeName").innerText = "歡迎: " + n;
          document.getElementById("total").innerText = total;
          updateProgress();

          interval = setInterval(() => {
            if (timer > 0) {
              timer--;
              document.getElementById("timer").innerText = fmtTime(timer);
            } else {
              clearInterval(interval);
              finish();
            }
          }, 1000);

          showQ();
        });

        prevBtn.addEventListener("click", () => {
          if (current > 0) {
            current--;
            answers.pop();
            showQ();
          }
        });

        leaveBtn.addEventListener("click", finish);
        retryBtn.addEventListener("click", () => location.reload());
        showWrongBtn.addEventListener("click", filterResultsWrong);
        showAllBtn.addEventListener("click", showAllResults);
        darkModeToggle.addEventListener("click", () => document.body.classList.toggle("dark"));

        function showQ() {
          if (current >= total) return finish();
          document.getElementById("current").innerText = current + 1;
          updateProgress();

          const q = shuffledQuestions[current];
          document.getElementById("questionText").innerText = q.question;

          const optDiv = document.getElementById("options");
          optDiv.innerHTML = "";

          let optionsWithFlag = q.options.map((option) => ({
            text: option,
            isAnswer: option.charAt(0) === q.answer,
          }));
          optionsWithFlag = shuffle(optionsWithFlag);

          optionsWithFlag.forEach((opt) => {
            const lbl = document.createElement("label");
            const rd = document.createElement("input");
            rd.type = "radio";
            rd.name = "opt";
            rd.value = opt.text;
            rd.onchange = () => {
              answers.push({
                q,
                selectedText: opt.text,
                correctText: q.options.find((optItem) => optItem.charAt(0) === q.answer),
                correct: opt.isAnswer,
              });
              current++;
              showQ();
            };
            lbl.append(rd, " ", opt.text);
            optDiv.append(lbl);
          });
        }

        function updateProgress() {
          const percent = (current / total) * 100;
          progressBar.style.width = percent + "%";
        }

        function finish() {
          clearInterval(interval);
          document.getElementById("quiz").classList.add("hidden");
          document.getElementById("results").classList.remove("hidden");
          renderResults(answers);
          document.getElementById("scoreSummary").innerText =
            `答對 ${answers.filter((a) => a.correct).length} 題 / 已作答 ${answers.length} 題 / 共 ${total} 題`;
          window.scrollTo({ top: 0, behavior: "smooth" });
        }

        function renderResults(data) {
          const tb = document.getElementById("resultsBody");
          tb.innerHTML = "";
          data.forEach((a) => {
            const tr = document.createElement("tr");
            tr.innerHTML = `
              <td>${a.q.question}</td>
              <td>${a.selectedText}</td>
              <td>${a.correctText}</td>
              <td>${a.correct ? "O" : "X"}</td>
            `;
            if (!a.correct) tr.classList.add("wrong");
            tb.append(tr);
          });
        }

        function filterResultsWrong() { renderResults(answers.filter((a) => !a.correct)); }
        function showAllResults() { renderResults(answers); }
      });
    </script>
  </body>
</html>
