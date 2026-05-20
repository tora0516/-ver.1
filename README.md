# -ver.1[index.html](https://github.com/user-attachments/files/28041507/index.html)
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>オリジナル性格診断</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #111827;
      color: white;
      margin: 0;
      padding: 0;
    }

    .container {
      max-width: 700px;
      margin: 50px auto;
      background: #1f2937;
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }

    h1 {
      text-align: center;
      margin-bottom: 30px;
    }

    .question {
      margin-bottom: 25px;
    }

    .question p {
      font-size: 18px;
      margin-bottom: 10px;
    }

    select {
      width: 100%;
      padding: 10px;
      border-radius: 10px;
      border: none;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 15px;
      background: #3b82f6;
      border: none;
      border-radius: 15px;
      color: white;
      font-size: 18px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background: #2563eb;
    }

    .result {
      margin-top: 30px;
      background: #374151;
      padding: 20px;
      border-radius: 15px;
      display: none;
    }

    .result h2 {
      margin-top: 0;
    }
  </style>
</head>

<body>

<div class="container">
  <h1>オリジナル16タイプ性格診断</h1>

  <div id="questions"></div>

  <button onclick="diagnose()">診断する</button>

  <div class="result" id="resultBox">
    <h2 id="typeName"></h2>
    <p id="typeDetail"></p>
  </div>
</div>

<script>

const questions = [
  { text: "初対面でも自然に会話を始められる", type: "EI" },
  { text: "一人の時間で回復することが多い", type: "EI_reverse" },
  { text: "現実的なデータを重視する", type: "SN" },
  { text: "アイデアや可能性を考えるのが好き", type: "SN_reverse" },
  { text: "判断は論理的に行う方だ", type: "TF" },
  { text: "人の感情を優先して考える", type: "TF_reverse" },
  { text: "予定を決めて動くと安心する", type: "JP" },
  { text: "その場の流れで動く方が好き", type: "JP_reverse" },
  { text: "人と話すと元気が出る", type: "EI" },
  { text: "締切は余裕を持って終わらせたい", type: "JP" }
];

const customTypes = {
  "ESTJ": {
    name: "アイアンキャプテン",
    detail: "責任感が強く、周囲をまとめるリーダータイプ。"
  },
  "ESTP": {
    name: "ブレイズランナー",
    detail: "行動力が高く、刺激を求める挑戦者。"
  },
  "ESFJ": {
    name: "サンライト",
    detail: "周囲を明るく支えるムードメーカー。"
  },
  "ESFP": {
    name: "フェスティバル",
    detail: "楽しいことが大好きなエンターテイナー。"
  },
  "ENTJ": {
    name: "キングメーカー",
    detail: "戦略を描き、人を導くカリスマ。"
  },
  "ENTP": {
    name: "イノベーター",
    detail: "新しい発想を次々生み出す改革者。"
  },
  "ENFJ": {
    name: "ハートガイド",
    detail: "人を励まし、成長へ導く支援者。"
  },
  "ENFP": {
    name: "スパーク",
    detail: "自由な感性で周囲を巻き込む冒険家。"
  },
  "ISTJ": {
    name: "クロノス",
    detail: "冷静かつ正確に物事を進める管理者。"
  },
  "ISTP": {
    name: "シャドウクラフト",
    detail: "静かに技術を極める職人タイプ。"
  },
  "ISFJ": {
    name: "ムーンケア",
    detail: "人知れず周囲を支える優しい守護者。"
  },
  "ISFP": {
    name: "アートウィンド",
    detail: "感性豊かでマイペースな表現者。"
  },
  "INTJ": {
    name: "ブラックオラクル",
    detail: "未来を見据えて戦略を立てる分析者。"
  },
  "INTP": {
    name: "ディープシンカー",
    detail: "知識を追求し続ける探究者。"
  },
  "INFJ": {
    name: "スターセージ",
    detail: "深い洞察力で人を理解する思想家。"
  },
  "INFP": {
    name: "ドリームメーカー",
    detail: "理想を大切にする創造的ロマンチスト。"
  }
};

const questionBox = document.getElementById("questions");

questions.forEach((q, index) => {
  questionBox.innerHTML += `
    <div class="question">
      <p>${index + 1}. ${q.text}</p>

      <select id="q${index}">
        <option value="1">1 全く当てはまらない</option>
        <option value="2">2</option>
        <option value="3">3 普通</option>
        <option value="4">4</option>
        <option value="5">5 とても当てはまる</option>
      </select>
    </div>
  `;
});

function diagnose() {

  let scores = {
    E:0, I:0,
    S:0, N:0,
    T:0, F:0,
    J:0, P:0
  };

  questions.forEach((q, index) => {

    const answer = Number(document.getElementById(`q${index}`).value);

    switch(q.type) {

      case "EI":
        scores.E += answer;
        break;

      case "EI_reverse":
        scores.I += answer;
        break;

      case "SN":
        scores.S += answer;
        break;

      case "SN_reverse":
        scores.N += answer;
        break;

      case "TF":
        scores.T += answer;
        break;

      case "TF_reverse":
        scores.F += answer;
        break;

      case "JP":
        scores.J += answer;
        break;

      case "JP_reverse":
        scores.P += answer;
        break;
    }
  });

  let personality = "";

  personality += scores.E >= scores.I ? "E" : "I";
  personality += scores.S >= scores.N ? "S" : "N";
  personality += scores.T >= scores.F ? "T" : "F";
  personality += scores.J >= scores.P ? "J" : "P";

  const result = customTypes[personality];

  document.getElementById("resultBox").style.display = "block";

  document.getElementById("typeName").innerHTML =
    `あなたのタイプ: ${result.name} (${personality})`;

  document.getElementById("typeDetail").innerHTML =
    result.detail;
}

</script>

</body>
</html>
