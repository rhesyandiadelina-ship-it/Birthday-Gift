# Birthday-Gift
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Special Gift 🎁</title>

<style>
body{
margin:0;
font-family:'Segoe UI',sans-serif;
background:linear-gradient(135deg,#e6ccff,#8b5e83);
text-align:center;
color:white;
overflow:hidden;
}

.screen{display:none;padding:20px;}
.active{display:block;}

h2{margin-top:20px;}

button{
padding:10px 18px;
border:none;
border-radius:20px;
background:white;
color:#8b5e83;
margin-top:15px;
box-shadow:0 5px 15px rgba(0,0,0,0.2);
}

/* PIN */
#display{
font-size:28px;
letter-spacing:10px;
margin:20px;
background:white;
color:black;
padding:10px;
border-radius:12px;
width:160px;
margin:auto;
}

.keypad button{
width:60px;height:60px;
border-radius:50%;
margin:6px;
font-size:18px;
}

.shake{
animation:shake 0.3s;
}
@keyframes shake{
0%{transform:translateX(0);}
25%{transform:translateX(-5px);}
50%{transform:translateX(5px);}
75%{transform:translateX(-5px);}
100%{transform:translateX(0);}
}

/* BALLOON */
.balloon{
width:40px;height:50px;
border-radius:50%;
position:absolute;
cursor:pointer;
animation:float 6s linear infinite;
}
@keyframes float{
from{transform:translateY(100vh);}
to{transform:translateY(-100vh);}
}

/* CARD */
.card{
width:70px;height:70px;
background:white;
color:black;
display:inline-block;
margin:6px;
line-height:70px;
border-radius:12px;
font-weight:bold;
font-size:18px;
}

/* CAT */
.cat{
animation:glow 1s infinite;
}
@keyframes glow{
0%,100%{filter:brightness(1);}
50%{filter:brightness(1.5);}
}

/* HEART */
.heart{
position:absolute;
animation:fall 3s linear;
}
@keyframes fall{
from{transform:translateY(-10px);}
to{transform:translateY(100vh);}
}

/* ENVELOPE */
.envelope{
font-size:70px;
cursor:pointer;
animation:bounce 1.5s infinite;
}
@keyframes bounce{
0%,100%{transform:translateY(0);}
50%{transform:translateY(-12px);}
}

.letter{
background:white;
color:black;
padding:20px;
border-radius:15px;
width:85%;
max-width:320px;
margin:auto;
margin-top:20px;
box-shadow:0 10px 20px rgba(0,0,0,0.2);
}

.hidden{display:none;}

.fade{
animation:fade 0.6s;
}
@keyframes fade{
from{opacity:0;}
to{opacity:1;}
}
</style>
</head>

<body>

<!-- PIN -->
<div class="screen active fade" id="s1">
<h2>🔐 Enter Secret Code</h2>

<div id="display">----</div>

<div class="keypad">
<button onclick="press(1)">1</button>
<button onclick="press(2)">2</button>
<button onclick="press(3)">3</button><br>

<button onclick="press(4)">4</button>
<button onclick="press(5)">5</button>
<button onclick="press(6)">6</button><br>

<button onclick="press(7)">7</button>
<button onclick="press(8)">8</button>
<button onclick="press(9)">9</button><br>

<button onclick="clearPin()">⌫</button>
<button onclick="press(0)">0</button>
<button onclick="checkPin()">OK</button>
</div>
</div>

<!-- BALLOON -->
<div class="screen" id="s2">
<h2>🎈 Pop 5 Balloons</h2>
</div>

<!-- CARD -->
<div class="screen" id="s3">
<h2>🧠 Match the Cards</h2>
<div id="cards"></div>
</div>

<!-- LOVE -->
<div class="screen" id="s4">
<h2>Do u love me? 🐱</h2>
<img class="cat" src="https://media.tenor.com/2roX3uxz_68AAAAC/cute-cat.gif" width="150">
<p>you have no choice 😭</p>
<button onclick="love()">YES</button>
</div>

<!-- RESULT -->
<div class="screen" id="s4b">
<h2>YEAYYY! ILY SO MUCH MY LOVEE 😭💜</h2>
<img src="https://media.tenor.com/XVn6kK5k9tYAAAAC/cute-cat-love.gif" width="150">
<button onclick="next(5)">Next</button>
</div>

<!-- WISH -->
<div class="screen" id="s5">
<h2>🎂 Make a Wish 🥳</h2>
<div style="font-size:70px;">🎂🕯️</div>
<p>tutup matamu, buat permintaan, lalu tiup lilinnya 🤭</p>
<button onclick="next(6)">tiup lilin</button>
</div>

<!-- NAME -->
<div class="screen" id="s6">
<h2>Enter your name</h2>
<input id="name">
<br>
<button onclick="checkName()">Open</button>
</div>

<!-- LETTER -->
<div class="screen" id="s7">
<div class="envelope" onclick="openLetter()">💌</div>
<p>Klik amplopnya...</p>

<div id="letterBox" class="letter hidden">
<p id="text"></p>
</div>
</div>

<script>

/* NAV */
function next(n){
document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
document.getElementById('s'+n).classList.add('active');
}

/* PIN */
let pin="";
function press(n){
if(pin.length<4){ pin+=n; update(); }
}
function clearPin(){
pin=pin.slice(0,-1); update();
}
function update(){
let m=pin.split("").map(()=> "●").join("");
display.innerText=m.padEnd(4,"-");
}
function checkPin(){
if(pin==="2026"){
next(2);
startBalloon();
}else{
display.classList.add("shake");
setTimeout(()=>display.classList.remove("shake"),300);
pin=""; update();
}
}

/* BALLOON */
let count=0;
function startBalloon(){
let interval=setInterval(()=>{
let b=document.createElement("div");
b.className="balloon";
b.style.left=Math.random()*window.innerWidth+"px";
b.style.background=["#ff9aa2","#b5ead7","#c7ceea"][Math.floor(Math.random()*3)];
b.onclick=()=>{
b.remove(); count++;
if(count>=5){
clearInterval(interval);
next(3);
startCards();
}
}
document.body.appendChild(b);
setTimeout(()=>b.remove(),6000);
},700);
}

/* CARD */
let first=null;
function startCards(){
let vals=[1,1,2,2];
vals.sort(()=>Math.random()-0.5);
cards.innerHTML="";
vals.forEach(v=>{
let c=document.createElement("div");
c.className="card";
c.innerHTML="?";
c.dataset.val=v;
c.onclick=()=>{
c.innerHTML=v;
if(!first){first=c;}
else{
if(first.dataset.val==c.dataset.val){
first=null;
setTimeout(()=>next(4),600);
}else{
setTimeout(()=>{
c.innerHTML="?";
first.innerHTML="?";
first=null;
},600);
}
}
}
cards.appendChild(c);
});
}

/* LOVE */
function love(){
next('4b');
for(let i=0;i<30;i++){
let h=document.createElement("div");
h.className="heart";
h.innerHTML="💜";
h.style.left=Math.random()*100+"%";
document.body.appendChild(h);
setTimeout(()=>h.remove(),3000);
}
}

/* NAME */
function checkName(){
if(name.value.toLowerCase()=="cya"){
next(7);
}else alert("salah 😭");
}

/* ENVELOPE */
function openLetter(){
letterBox.classList.remove("hidden");
for(let i=0;i<20;i++){
let h=document.createElement("div");
h.innerHTML="💜";
h.className="heart";
h.style.left=Math.random()*100+"%";
document.body.appendChild(h);
setTimeout(()=>h.remove(),3000);
}
typeText();
}

/* TEXT */
let msg=`cheers to your special day, Kacyaa ✨
maaf yaa kalau aku masih belum terlalu pinter merangkai kata di hari spesial kakak ini, tapi aku cuma mau bilang makasih banyak… karena Kacyaa udah jadi orang yang baik banget ke aku sejak pertama kali aku gabung discord, yang nyambut aku, ngajak main, dan bikin aku ngerasa diterima 🫰

terima kasih juga yaa udah mau jadiin aku adeknya Kacyaa. walaupun kita jarang chat, tapi aku tetep anggap Kacyaa sebagai sosok yang baik banget 🤍

waktu cepat banget berlalu, sekarang usia Kacyaa bertambah satu tahun lagi. hari ini mungkin hari yang spesial buat kakak, dan maaf yaa aku belum bisa ngasih apa-apa selain doa 🙏

hari ini aku langitkan semua doa terbaikku untuk Kacyaa. semoga hal-hal yang pernah membuat kakak runtuh justru jadi alasan untuk tumbuh. semoga dunia selalu menjaga kakak di mana pun berada. semoga hari-hari kakak selalu dipenuhi cinta yang nggak ada batasnya. dan semoga semua usaha kakak, termasuk membahagiakan orang tua, perlahan terwujud 💫

happy birthday sekali lagi yaa, Kacyaa 🎉✨`;

let i=0;
function typeText(){
let t=document.getElementById("text");
let typing=setInterval(()=>{
t.innerHTML+=msg[i];
i++;
if(i>=msg.length) clearInterval(typing);
},20);
}

</script>

</body>
</html>
