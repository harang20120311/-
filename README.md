<!DOCTYPE html><html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>쿠키런 캐릭터 입력기</title>
  <style>
    body {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      padding: 20px;
      font-family: 'Segoe UI', sans-serif;
      background: #f5f5f5;
    }
    .form, .preview {
      flex: 1 1 45%;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      max-height: 90vh;
      overflow-y: auto;
    }
    label {
      font-weight: bold;
      display: block;
      margin-top: 15px;
    }
    input, textarea {
      width: 100%;
      padding: 6px;
      border-radius: 6px;
      border: 1px solid #ccc;
      margin-top: 5px;
    }
    img {
      max-width: 100%;
      margin-top: 10px;
      border-radius: 10px;
    }
    .btns {
      margin-top: 20px;
      display: flex;
      gap: 10px;
    }
    button {
      padding: 10px 15px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
    }
    .save { background: #4caf50; color: white; }
    .download { background: #2196f3; color: white; }
  </style>
</head>
<body>  <div class="form">
    <h2>🍪 쿠키 정보 입력</h2>
    <label>쿠키 이름</label>
    <input id="cookieName" type="text"><label>쿠키 이미지</label>
<input type="file" accept="image/*" id="cookieImage">

<label>스킬 설명</label>
<textarea id="skillDesc"></textarea>

<label>Lv.1 에너지</label>
<input id="energy1" type="text">

<label>Lv.1 스킬 상세</label>
<textarea id="skillDetail1"></textarea>

<label>Lv.15 에너지</label>
<input id="energy15" type="text">

<label>Lv.15 스킬 상세</label>
<textarea id="skillDetail15"></textarea>

<label>매직 캔디 설명</label>
<textarea id="candyDesc"></textarea>

<label>캔디 Lv.5 효과</label>
<textarea id="candyEffect"></textarea>

<label>Í    <label>\xcd95복 1~4</label>
<input id="bless1" type="text" placeholder="Í\xcd95복 1">
<input id="bless2" type="text" placeholder="Í\xcd95복 2">
<input id="bless3" type="text" placeholder="Í\xcd95복 3">
<input id="bless4" type="text" placeholder="Í\xcd95복 4">

<label>관계 체크 (예: Wizard Cookie - Friendly)</label>
<textarea id="relationship"></textarea>

<label>대사 (일반)</label>
<textarea id="quoteGeneral"></textarea>

<label>대사 (피고나)</label>
<textarea id="quoteTired"></textarea>

<label>대사 (로비)</label>
<textarea id="quoteLobby"></textarea>

<div class="btns">
  <button class="save" onclick="downloadHTML()">💾 저장하기</button>
  <button class="download" onclick="downloadImage()">🖼️ 이미지 다운로드</button>
</div>

  </div>  <div class="preview">
    <h2 id="previewName">[쿠키 이름]</h2>
    <img id="previewImage" src="" style="display:none;" />
    <h3>🌟 스킬 설명</h3>
    <p id="previewSkillDesc"></p><h4>Lv.1</h4>
<p><b>에너지:</b> <span id="previewEnergy1"></span></p>
<p id="previewSkill1"></p>

<h4>Lv.15</h4>
<p><b>에너지:</b> <span id="previewEnergy15"></span></p>
<p id="previewSkill15"></p>

<h3>🍬 매직 캔디</h3>
<p id="previewCandyDesc"></p>
<p><b>Lv.5 효과:</b> <span id="previewCandyEffect"></span></p>

<h3>✨ Í \xcd95복</h3>
<ul>
  <li id="previewBless1"></li>
  <li id="previewBless2"></li>
  <li id="previewBless3"></li>
  <li id="previewBless4"></li>
</ul>

<h3>📌 관계 체크</h3>
<pre id="previewRelationship"></pre>

<h3>💬 대사</h3>
<b>일반:</b>
<p id="previewQuoteGeneral"></p>
<b>피고나:</b>
<p id="previewQuoteTired"></p>
<b>로비:</b>
<p id="previewQuoteLobby"></p>

  </div>  <script>
    const fields = [
      ['cookieName', 'previewName'],
      ['skillDesc', 'previewSkillDesc'],
      ['energy1', 'previewEnergy1'],
      ['skillDetail1', 'previewSkill1'],
      ['energy15', 'previewEnergy15'],
      ['skillDetail15', 'previewSkill15'],
      ['candyDesc', 'previewCandyDesc'],
      ['candyEffect', 'previewCandyEffect'],
      ['bless1', 'previewBless1'],
      ['bless2', 'previewBless2'],
      ['bless3', 'previewBless3'],
      ['bless4', 'previewBless4'],
      ['relationship', 'previewRelationship'],
      ['quoteGeneral', 'previewQuoteGeneral'],
      ['quoteTired', 'previewQuoteTired'],
      ['quoteLobby', 'previewQuoteLobby'],
    ];

    fields.forEach(([inputId, previewId]) => {
      document.getElementById(inputId).addEventListener('input', () => {
        document.getElementById(previewId).textContent = document.getElementById(inputId).value;
      });
    });

    document.getElementById('cookieImage').addEventListener('change', (e) => {
      const file = e.target.files[0];
      const reader = new FileReader();
      reader.onload = () => {
        const img = document.getElementById('previewImage');
        img.src = reader.result;
        img.style.display = 'block';
        img.setAttribute('data-src', reader.result);
      };
      if (file) reader.readAsDataURL(file);
    });

    function downloadHTML() {
      const content = document.documentElement.outerHTML;
      const blob = new Blob([content], { type: 'text/html' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = "cookie_character.html";
      a.click();
    }

    function downloadImage() {
      const img = document.getElementById('previewImage');
      if (!img || !img.getAttribute('data-src')) return alert("이미지가 없습니다.");
      const a = document.createElement('a');
      a.href = img.getAttribute('data-src');
      a.download = "cookie_image.png";
      a.click();
    }
  </script></body>
</html><!DOCTYPE html><html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>쿠키런 캐릭터 입력기</title>
  <style>
    body {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      padding: 20px;
      font-family: 'Segoe UI', sans-serif;
      background: #f5f5f5;
    }
    .form, .preview {
      flex: 1 1 45%;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      max-height: 90vh;
      overflow-y: auto;
    }
    label {
      font-weight: bold;
      display: block;
      margin-top: 15px;
    }
    input, textarea {
      width: 100%;
      padding: 6px;
      border-radius: 6px;
      border: 1px solid #ccc;
      margin-top: 5px;
    }
    img {
      max-width: 100%;
      margin-top: 10px;
      border-radius: 10px;
    }
    .btns {
      margin-top: 20px;
      display: flex;
      gap: 10px;
    }
    button {
      padding: 10px 15px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
    }
    .save { background: #4caf50; color: white; }
    .download { background: #2196f3; color: white; }
  </style>
</head>
<body>  <div class="form">
    <h2>🍪 쿠키 정보 입력</h2>
    <label>쿠키 이름</label>
    <input id="cookieName" type="text"><label>쿠키 이미지</label>
<input type="file" accept="image/*" id="cookieImage">

<label>스킬 설명</label>
<textarea id="skillDesc"></textarea>

<label>Lv.1 에너지</label>
<input id="energy1" type="text">

<label>Lv.1 스킬 상세</label>
<textarea id="skillDetail1"></textarea>

<label>Lv.15 에너지</label>
<input id="energy15" type="text">

<label>Lv.15 스킬 상세</label>
<textarea id="skillDetail15"></textarea>

<label>매직 캔디 설명</label>
<textarea id="candyDesc"></textarea>

<label>캔디 Lv.5 효과</label>
<textarea id="candyEffect"></textarea>

<label>Í    <label>\xcd95복 1~4</label>
<input id="bless1" type="text" placeholder="Í\xcd95복 1">
<input id="bless2" type="text" placeholder="Í\xcd95복 2">
<input id="bless3" type="text" placeholder="Í\xcd95복 3">
<input id="bless4" type="text" placeholder="Í\xcd95복 4">

<label>관계 체크 (예: Wizard Cookie - Friendly)</label>
<textarea id="relationship"></textarea>

<label>대사 (일반)</label>
<textarea id="quoteGeneral"></textarea>

<label>대사 (피고나)</label>
<textarea id="quoteTired"></textarea>

<label>대사 (로비)</label>
<textarea id="quoteLobby"></textarea>

<div class="btns">
  <button class="save" onclick="downloadHTML()">💾 저장하기</button>
  <button class="download" onclick="downloadImage()">🖼️ 이미지 다운로드</button>
</div>

  </div>  <div class="preview">
    <h2 id="previewName">[쿠키 이름]</h2>
    <img id="previewImage" src="" style="display:none;" />
    <h3>🌟 스킬 설명</h3>
    <p id="previewSkillDesc"></p><h4>Lv.1</h4>
<p><b>에너지:</b> <span id="previewEnergy1"></span></p>
<p id="previewSkill1"></p>

<h4>Lv.15</h4>
<p><b>에너지:</b> <span id="previewEnergy15"></span></p>
<p id="previewSkill15"></p>

<h3>🍬 매직 캔디</h3>
<p id="previewCandyDesc"></p>
<p><b>Lv.5 효과:</b> <span id="previewCandyEffect"></span></p>

<h3>✨ Í \xcd95복</h3>
<ul>
  <li id="previewBless1"></li>
  <li id="previewBless2"></li>
  <li id="previewBless3"></li>
  <li id="previewBless4"></li>
</ul>

<h3>📌 관계 체크</h3>
<pre id="previewRelationship"></pre>

<h3>💬 대사</h3>
<b>일반:</b>
<p id="previewQuoteGeneral"></p>
<b>피고나:</b>
<p id="previewQuoteTired"></p>
<b>로비:</b>
<p id="previewQuoteLobby"></p>

  </div>  <script>
    const fields = [
      ['cookieName', 'previewName'],
      ['skillDesc', 'previewSkillDesc'],
      ['energy1', 'previewEnergy1'],
      ['skillDetail1', 'previewSkill1'],
      ['energy15', 'previewEnergy15'],
      ['skillDetail15', 'previewSkill15'],
      ['candyDesc', 'previewCandyDesc'],
      ['candyEffect', 'previewCandyEffect'],
      ['bless1', 'previewBless1'],
      ['bless2', 'previewBless2'],
      ['bless3', 'previewBless3'],
      ['bless4', 'previewBless4'],
      ['relationship', 'previewRelationship'],
      ['quoteGeneral', 'previewQuoteGeneral'],
      ['quoteTired', 'previewQuoteTired'],
      ['quoteLobby', 'previewQuoteLobby'],
    ];

    fields.forEach(([inputId, previewId]) => {
      document.getElementById(inputId).addEventListener('input', () => {
        document.getElementById(previewId).textContent = document.getElementById(inputId).value;
      });
    });

    document.getElementById('cookieImage').addEventListener('change', (e) => {
      const file = e.target.files[0];
      const reader = new FileReader();
      reader.onload = () => {
        const img = document.getElementById('previewImage');
        img.src = reader.result;
        img.style.display = 'block';
        img.setAttribute('data-src', reader.result);
      };
      if (file) reader.readAsDataURL(file);
    });

    function downloadHTML() {
      const content = document.documentElement.outerHTML;
      const blob = new Blob([content], { type: 'text/html' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = "cookie_character.html";
      a.click();
    }

    function downloadImage() {
      const img = document.getElementById('previewImage');
      if (!img || !img.getAttribute('data-src')) return alert("이미지가 없습니다.");
      const a = document.createElement('a');
      a.href = img.getAttribute('data-src');
      a.download = "cookie_image.png";
      a.click();
    }
  </script></body>
</html># -
