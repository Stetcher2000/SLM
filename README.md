<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1,user-scalable=no" />
  <title>Secrets Locked</title>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Source+Code+Pro:400,700&display=swap">
  <style>
    html, body {
      height: 100%;
      margin: 0;
      padding: 0;
      overflow: hidden;
      background: #000;
    }
    body, input, button {
      font-family: 'Source Code Pro', monospace !important;
      color: #fff;
    }
    #matrix-canvas {
      position: fixed;
      left: 0; top: 0;
      width: 100vw; height: 100vh;
      z-index: 0;
      pointer-events: none;
      background: #000;
    }
    .main-content {
      position: absolute;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      z-index: 5;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    .header {
      margin: 0;
      font-size: 3.5rem;
      color: #ff0033;
      text-align: center;
      font-weight: bold;
      text-shadow: 0 0 18px #8000ff, 0 0 7px #8000ff;
      letter-spacing: 2px;
      user-select: none;
    }
    .password {
      font-size: 5rem;
      color: #ff0033;
      text-align: center;
      text-shadow: 0 0 18px #8000ff, 0 0 7px #8000ff;
      margin-top: 0.3rem;
      font-weight: bold;
      user-select: none;
    }
    .hacker-btn {
      margin: 2.2rem auto 0 auto;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(90deg, #111 70%, #41002a 100%);
      border: 2px solid #db1c54;
      color: #39ff14;
      font-size: 1.4rem;
      font-family: 'Source Code Pro', monospace;
      padding: 1.1rem 2.8rem;
      border-radius: 0.6rem;
      box-shadow: 0 0 11px #8000ff70, 0 0 7px #14001a;
      cursor: pointer;
      transition: background 0.25s, color 0.25s;
      position: relative;
      outline: none;
    }
    .hacker-btn:active {
      background: #510025;
      color: #fff;
    }
    .ip-box {
      position: fixed;
      left: 0.4vw; bottom: 0.8vh;
      background: rgba(0,0,0,0.85);
      color: #39ff14;
      font-size: 0.7rem;
      padding: 0.25rem 0.9rem;
      border-radius: 8px 8px 0 0;
      z-index: 100;
      min-width: 80px;
      max-width: 45vw;
      pointer-events: none;
      opacity: 0.65;
      font-family: "Source Code Pro", monospace;
      text-align: left;
      box-shadow: 0 0 5px #21ff65;
    }
    .overlay {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      z-index: 50;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(12,12,12,0.92);
      animation: fadeIn 0.26s;
    }
    @keyframes fadeIn {from {opacity:0} to {opacity:1}}
    .popup {
      background: #060009;
      border: 2.7px solid #7e1046;
      border-radius: 16px;
      box-shadow: 0 0 22px #210839, 0 0 8px #ff0547ea;
      padding: 1.2rem 1.4rem 0.8rem 1.4rem;
      min-width: 65vw; max-width: 90vw;
      color: #39ff14;
      position: relative;
      font-size: 1.18rem;
      text-align: center;
      font-family: "Source Code Pro", monospace;
      user-select: none;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    .popup.red {color:#ff0033;}
    .popup .red-x {
      position: absolute; top: 0.95rem; right: 1.3rem;
      color: #ff073a;
      font-size: 1.7rem;
      font-weight: bold;
      cursor: pointer;
      transition: color 0.2s;
      user-select: none;
      z-index: 20;
    }
    .popup .red-x:active { color: #a0002d; }
    .popup input, .popup textarea {
      width: 85%; margin: 0.9rem 0 0.6rem 0;
      background: #1a1a22;
      border: 2px solid #81ffb6;
      color: #fff !important;
      font-size: 1.1rem;
      padding: 0.36rem 0.7rem;
      border-radius: 9px;
      font-family: "Source Code Pro", monospace;
      outline: none;
      text-align: center;
    }
    .popup label {
      color: #39ff14;
      font-size: 1.11rem;
      display: block;
      margin-top: 1.1rem;
    }
    .popup .btn-pop {
      background: #000;
      color: #39ff14;
      border: 2px solid #14ff65;
      border-radius: 8px;
      padding: 0.41rem 1.6rem;
      font-size: 1.14rem;
      font-family: "Source Code Pro", monospace;
      margin-top: 1.3rem;
      margin-bottom: 0.5rem;
      cursor: pointer;
      box-shadow: 0 0 7px #8000ff41;
      transition: background 0.13s;
    }
    .popup .btn-pop:active { background: #08002a; }
    .load-bar-bg {
      background: #0d1b0d;
      width: 95%;
      max-width: 345px;
      height: 20px;
      border-radius: 8px;
      border: 2px solid #37a636;
      margin: 1.5rem auto 0.9rem auto;
      box-shadow: 0 0 20px #21ff65bb;
      display: flex;
      align-items: center;
    }
    .load-bar {
      height: 100%;
      background: linear-gradient(90deg, #39ff14 60%, #27c800 100%);
      border-radius: 6px;
      box-shadow: 0 0 14px #00ff9c;
      transition: width 0.16s;
    }
    .file-screen {
      position: absolute;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      z-index: 80;
      background: rgba(0,0,0,0.92);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    .file-header {
      font-size: 2.6rem;
      color: #39ff14;
      margin: 0 auto 1.2rem auto;
      text-shadow: 0 0 18px #21ff65, 0 0 9px #151f0e;
    }
    .files-list {
      width: 88%;
      margin: 0 auto;
      max-width: 425px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 1rem;
    }
    .file-row {
      background: #1b1c1c;
      color: #00e033;
      padding: 1.1rem 1.4rem;
      border-radius: 9px;
      box-shadow: 0 0 8px #00ffb420;
      font-family: "Source Code Pro", monospace;
      font-size: 1.22rem;
      display: flex;
      align-items: center;
      justify-content: flex-start;
      width: 100%;
      max-width: 300px;
    }
    .file-row::before {
      content: "📄";
      margin-right: 12px;
      font-size: 1.24rem;
    }
    .ban-lock {
      position: fixed;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      font-size: 7.6rem;
      color: #ff0033;
      text-shadow: 0 0 22px #780034, 0 0 9px #ff003355;
      z-index: 2002;
      animation: shake 0.44s infinite alternate;
      user-select: none;
    }
    @keyframes shake {
      from { transform: translate(-50%, -50%) rotate(-3deg);}
      to {   transform: translate(-50%, -50%) rotate(3deg);}
    }
    .recording-icon {
      position: fixed;
      top: 2vh; left: 2vw;
      font-size: 1.6rem;
      color: #ff0033;
      z-index: 2002;
      font-family: "Source Code Pro", monospace;
      background: #10070d;
      border-radius: 50%;
      box-shadow: 0 0 7px #ff123d;
      padding: 0.18em 0.34em;
      animation: pulse 0.8s infinite alternate;
      display: flex;
      align-items: center;
      gap: 0.4em;
    }
    .recording-icon span { font-size: 0.77rem; color: #ff0033; letter-spacing: 1px; }
    @keyframes pulse {
      from { box-shadow: 0 0 6px #ff123d, 0 0 1px #a2002d;}
      to { box-shadow: 0 0 28px #ff0033, 0 0 6px #a2002d;}
    }
    @media (max-width: 450px) {
      .header { font-size: 2.2rem; }
      .password { font-size: 3.1rem; }
      .popup { font-size: 1rem; min-width: 92vw;}
    }
  </style>
</head>
<body>
<canvas id="matrix-canvas"></canvas>
<div class="main-content" id="mainPage">
  <div class="header">Secrets Locked</div>
  <div class="password">PASSWORD</div>
  <button class="hacker-btn" id="roseBtn">
    <span style="font-size:1.5em;vertical-align:middle;">🥀</span>&nbsp;Unlock
  </button>
</div>
<div class="ip-box" id="ipBox">IP: --.--.--.--</div>
<div id="modal-root"></div>
<script>
  // Matrix Digital Rain
  const canvas = document.getElementById('matrix-canvas');
  const ctx = canvas.getContext('2d');
  let w = 0, h = 0;
  const fontSize = 16, columns = [];
  const chars = "アァカサタナハマヤャラワガザダバパイィキシチニヒミリヰギジヂビピウゥクスツヌフムユュルグズヅブプエェケセテネヘメレヱゲゼデベペオォコソトノホモヨョロヲゴゾドボポヴッンabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789".split('');
  function resizeMatrix() {
    w = window.innerWidth;
    h = window.innerHeight;
    canvas.width = w; canvas.height = h;
    for (let i = 0; i < (w/fontSize); i++) columns[i] = Math.floor(Math.random()*h/fontSize);
  }
  resizeMatrix();
  window.addEventListener('resize',resizeMatrix);
  function drawMatrix() {
    ctx.fillStyle = "rgba(0,0,0,0.14)";
    ctx.fillRect(0,0,w,h);
    ctx.font = fontSize + "px 'Source Code Pro', monospace";
    ctx.fillStyle = "#18ff25";
    for (let i = 0; i < columns.length; i++) {
      const text = chars[Math.floor(Math.random()*chars.length)];
      ctx.fillText(text, i*fontSize, columns[i]*fontSize);
      if (columns[i]*fontSize > h && Math.random()>0.99) columns[i] = 0;
      columns[i]++;
    }
    requestAnimationFrame(drawMatrix);
  }
  drawMatrix();

  // Fetch IP address
  fetch("https://api.ipify.org?format=json")
    .then(r=>r.json()).then(obj=>{
      document.getElementById("ipBox").textContent = "IP: " + obj.ip;
    }).catch(()=>{});

  // Modal/popup logic
  function $(q) { return document.getElementById(q)||document.querySelector(q);}
  let state = { step: 0, tries: 0, nameTried: 0, banned: false, resume: false, correct: false, unlocked: false };
  let atNameStep = false;

  // Modal root for popups
  const modalRoot = $("modal-root");
  function closeOverlay() { modalRoot.innerHTML = ""; }
  function showLoading() {
    modalRoot.innerHTML = `
    <div class="overlay">
      <div class="popup" style="color:#39ff14;min-width:180px;font-size:1.28rem;">
        Decrypting Secrets
        <div class="load-bar-bg"><div class="load-bar" id="bar" style="width:0%"></div></div>
      </div>
    </div>`;
    let percent = 0;
    const bar = document.getElementById("bar");
    let tStart = Date.now();
    function tick() {
      percent = Math.min(100, Math.floor((Date.now() - tStart)/10000 * 100));
      bar.style.width = percent+"%";
      if (percent < 100) requestAnimationFrame(tick);
    }
    tick();
  }
  function askGoodPerson() {
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup">
          <span class="red-x" onclick="location.reload()">&times;</span>
          Was he a good person?<br>
          <input id="goodInput" autocomplete="off" autocorrect="off" spellcheck="false" maxlength="7" />
          <button class="btn-pop" onclick="checkGoodPerson()">Submit</button>
        </div>
      </div>
    `;
    $("goodInput").focus();
  }
  function checkGoodPerson() {
    const user = ($("goodInput")?.value||"").trim().toLowerCase();
    if(user === "no") {
      closeOverlay(); nextStepMovie();
    } else {
      $(".popup").classList.add("red");
      $(".popup").style.color = "#ff0033";
      $(".popup").innerHTML += `<div style='color:#ff0033;margin-top:0.7em;'>Sorry, it seems like you have answered incorrectly</div>`;
      setTimeout(()=>{location.reload();},3000);
    }
  }
  function nextStepMovie() {
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup">
          <span class="red-x" onclick="location.reload()">&times;</span>
          Favorite Movie of all time?<br>
          <input id="movieInput" autocomplete="off" autocorrect="off" spellcheck="false" maxlength="31"/>
          <button class="btn-pop" onclick="checkMovie()">Submit</button>
        </div>
      </div>
    `;
    $("movieInput").focus();
  }
  function checkMovie() {
    const user = ($("movieInput")?.value||"").trim().toLowerCase();
    if(user === "never back down") {
      closeOverlay(); nextStepRose();
    } else {
      $(".popup").classList.add("red");
      $(".popup").style.color = "#ff0033";
      $(".popup").innerHTML += `<div style='color:#ff0033;margin-top:0.7em;'>Sorry, it seems like you have answered incorrectly</div>`;
      setTimeout(()=>{location.reload();},3000);
    }
  }
  function nextStepRose() {
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup">
          <span class="red-x" onclick="location.reload()">&times;</span>
          Favorite color rose?<br>
          <input id="roseInput" autocomplete="off" autocorrect="off" maxlength="11" />
          <button class="btn-pop" onclick="checkRose()">Submit</button>
        </div>
      </div>
    `;
    $("roseInput").focus();
  }
  function checkRose() {
    const user = ($("roseInput")?.value||"").trim().toLowerCase();
    if(user === "white") {
      closeOverlay(); enterNameStep();
    } else {
      $(".popup").classList.add("red");
      $(".popup").style.color = "#ff0033";
      $(".popup").innerHTML += `<div style='color:#ff0033;margin-top:0.7em;'>Sorry, it seems like you have answered incorrectly</div>`;
      setTimeout(()=>{location.reload();},3000);
    }
  }
  function enterNameStep(){
    atNameStep = true; state.nameTried = 0; state.banned = false;
    showNamePopup();
  }
  function showNamePopup(){
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup">
          <span class="red-x" onclick="location.reload()">&times;</span>
          Enter your first name and last name<br>
          <input id="firstName" placeholder="First Name" autocomplete="off" autocorrect="off" style="margin-top:0.7rem"/><br>
          <input id="lastName" placeholder="Last Name" autocomplete="off" autocorrect="off"/><br>
          <button class="btn-pop" onclick="checkName()">Submit</button>
        </div>
      </div>
    `;
    $("firstName").focus();
  }
  function checkName(){
    const fname = ($("firstName")?.value||"").trim().toLowerCase();
    const lname = ($("lastName")?.value||"").trim().toLowerCase();
    if(fname === "shaina" && lname === "knox"){
      closeOverlay(); showAccessGranted();
    } else {
      state.nameTried++;
      showScreenRecording();
      if(state.nameTried >= 3){
        banIP();
      } else {
        setTimeout(showNamePopup,2200);
      }
    }
  }
  function showScreenRecording(){
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup red" style="font-size:1.22rem;">Your screen is now being recorded...</div>
      </div>
    `;
    showRecordingIcon();
    setTimeout(closeOverlay,2000);
  }
  function banIP(){
    closeOverlay();
    document.body.innerHTML += `<div class="ban-lock"><span>🔒</span></div>`;
  }
  function showRecordingIcon(){
    if(!document.querySelector(".recording-icon")){
      let icon = document.createElement("div");
      icon.className = "recording-icon";
      icon.innerHTML = `● <span>REC</span>`;
      document.body.appendChild(icon);
    }
  }
  function showAccessGranted(){
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup" style="color:#39ff14">Access Granted!</div>
      </div>
    `;
    setTimeout(()=>{closeOverlay(); showVerify();},3200);
  }
  function showVerify(){
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup" style="color:#39ff14">Hmm… I’ll need to verify it’s you</div>
      </div>
    `;
    setTimeout(()=>{closeOverlay(); askUVProtected();},5000);
  }
  function askUVProtected(){
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup">
          How is it UV protected?<br>
          <input id="uvInput" autocomplete="off" autocorrect="off" maxlength="24"/>
          <button class="btn-pop" onclick="checkUV()">Submit</button>
        </div>
      </div>
    `;
    $("uvInput").focus();
  }
  function checkUV() {
    const answer = ($("uvInput")?.value||"").trim().toLowerCase();
    if(answer==="a light bulb"||answer==="light bulb"){
      closeOverlay(); showWelcome();
    } else {
      $(".popup").classList.add("red");
      $(".popup").style.color = "#ff0033";
      $(".popup").innerHTML += `<div style='color:#ff0033;margin-top:0.7em;'>Sorry, it seems like you have answered incorrectly</div>`;
      setTimeout(()=>{
        closeOverlay()
        enterNameStep();
      },2400);
    }
  }
  function showWelcome(){
    modalRoot.innerHTML = `
      <div class="overlay">
        <div class="popup" style="color:#39ff14">Welcome Shaina Knox!</div>
      </div>
    `;
    setTimeout(()=>{closeOverlay(); showFiles();},2600);
  }
  function showFiles(){
    document.body.innerHTML += `
      <div class="file-screen" id="fileScreen">
        <div class="file-header">Top Secret Files</div>
        <div class="files-list">
          <div class="file-row">File_001-Expunged.txt</div>
          <div class="file-row">File_002-Redacted.log</div>
          <div class="file-row">File_003-TopSecret.mp4</div>
        </div>
      </div>
    `;
  }
  $("roseBtn").onclick = function(){
    if(atNameStep){
      enterNameStep();
    } else {
      showLoading();
      setTimeout(()=>{closeOverlay(); askGoodPerson();},10020);
    }
  }
</script>
</body>
</html>
