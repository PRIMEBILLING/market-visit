<!doctype html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Market Visit</title>
<style>
body{font-family:Arial;max-width:450px;margin:auto;padding:18px}
input,textarea,button{width:100%;box-sizing:border-box;padding:11px;margin:7px 0;font-size:16px}
button{background:#4285f4;color:#fff;border:0;border-radius:6px}
button:disabled{background:#aaa}
.secondary{background:#eee;color:#333}
label{display:block;font-weight:bold;font-size:14px;margin-top:12px}
.info{background:#f2f6ff;padding:12px;border-radius:7px}
.gps{background:#f5f5f5;padding:10px;border-radius:6px;white-space:pre-line;font-size:13px}
.status{margin-top:10px;font-weight:bold;white-space:pre-line}
video,img{width:100%;border-radius:8px}
video{background:#000;display:none}
img{display:none;margin-top:8px}
.dec{display:flex;gap:8px;background:#fff8e1;padding:10px}
.dec input{width:auto}
.note{font-size:12px;color:#666}
</style>
</head>

<body>

<h3>Employee Login</h3>

<div id="login">
<label>Employee ID</label>
<input id="emp" placeholder="EMP001">
<button onclick="login()">Login</button>
<div id="ls" class="status"></div>
</div>

<div id="form" style="display:none">

<h3>Market Visit Entry</h3>

<div class="info">
Employee: <b id="name"></b> (<span id="eid"></span>)<br>
Head Quarter: <span id="hq"></span><br>
Email: <span id="email"></span>
</div>

<label>To Visit City / District / State</label>
<input id="city" placeholder="Noida, UP">

<label>Visit Customer Firm Name</label>
<input id="firm" placeholder="Firm name">

<label>Visit Customer Name</label>
<input id="customer" placeholder="Customer name">

<label>Visit Area Pin Code</label>
<input id="pin" inputmode="numeric" maxlength="6" placeholder="201301">

<label>Visited Customer Photo - Live Camera Only</label>

<video id="video" autoplay playsinline></video>

<button id="start" onclick="startCamera()">
📷 Start Live Camera
</button>

<button id="capture" onclick="capture()" style="display:none">
📸 Capture Live Photo
</button>

<button id="stop" class="secondary" onclick="stopCamera()" style="display:none">
Stop Camera
</button>

<canvas id="canvas" style="display:none"></canvas>

<img id="photo">

<button id="retake" class="secondary" onclick="retake()" style="display:none">
🔄 Retake Photo
</button>

<div id="gps" class="gps">
📍 GPS: Waiting...
</div>

<div class="note">
Photo केवल live camera frame से capture होगी। Gallery upload नहीं है।
</div>

<label>Visit Remarks / Discussion Notes</label>

<textarea id="remarks" rows="3"></textarea>

<div class="dec">
<input type="checkbox" id="decl" onchange="check()">
<span>
I confirm that the above attendance and market visit details are true and correct.
</span>
</div>

<button id="submit" onclick="submitVisit()" disabled>
Submit Visit
</button>

<div id="status" class="status"></div>

<button class="secondary" onclick="logout()">
Logout
</button>

</div>

<script>

const API_URL =
'https://script.google.com/macros/s/AKfycbyALO95cTRsYWt4yA5P1bijCghETY8G1MsxRyZpMpsmP6ChkRqqPsqoEo3oAUJZ2H7tUA/exec';

let employee = {};
let stream = null;
let photoBase64 = null;
let lat = null;
let lng = null;
let acc = null;
let address = '';

async function getData(params){

let response =
await fetch(
API_URL + '?' +
new URLSearchParams(params)
);

if(!response.ok)
throw new Error('Server error: ' + response.status);

return response.json();

}

async function login(){

let id =
document.getElementById('emp').value.trim();

let status =
document.getElementById('ls');

if(!id){

status.textContent =
'Employee ID bharein.';

return;

}

status.textContent =
'Checking...';

try{

let result =
await getData({
action:'verifyEmployee',
empId:id
});

if(!result.success){

status.textContent =
'❌ Employee ID nahi mila.';

return;

}

employee = result;

document.getElementById('login').style.display='none';
document.getElementById('form').style.display='block';

document.getElementById('name').textContent =
result.name || '';

document.getElementById('eid').textContent =
result.empId || '';

document.getElementById('hq').textContent =
result.hq || '';

document.getElementById('email').textContent =
result.email || '';

}catch(error){

status.textContent =
'❌ ' + error.message;

}

}

async function startCamera(){

if(
!navigator.mediaDevices ||
!navigator.mediaDevices.getUserMedia
){

document.getElementById('gps').textContent =
'❌ Camera unavailable.\nHTTPS Chrome page use karein.';

return;

}

try{

stream =
await navigator.mediaDevices.getUserMedia({

video:{
facingMode:{ideal:'environment'},
width:{ideal:1280},
height:{ideal:720}
},

audio:false

});

let video =
document.getElementById('video');

video.srcObject =
stream;

video.style.display =
'block';

document.getElementById('start').style.display =
'none';

document.getElementById('capture').style.display =
'block';

document.getElementById('stop').style.display =
'block';

document.getElementById('gps').textContent =
'📍 Camera ready.\nGPS प्राप्त हो रहा है...';

getGPS();

}catch(error){

document.getElementById('gps').textContent =
'❌ Camera access error: ' +
error.message +
'\n\nChrome में Camera = Allow करें.';

}

}

function getGPS(){

if(!navigator.geolocation){

document.getElementById('gps').textContent =
'❌ GPS unavailable';

return;

}

navigator.geolocation.getCurrentPosition(

function(position){

lat =
position.coords.latitude;

lng =
position.coords.longitude;

acc =
position.coords.accuracy;

document.getElementById('gps').textContent =
'📍 GPS Ready\n' +
'Lat: ' + lat.toFixed(6) + '\n' +
'Lng: ' + lng.toFixed(6) + '\n' +
'Accuracy: ' + Math.round(acc) + ' meters';

getAddress();

},

function(error){

document.getElementById('gps').textContent =
'❌ GPS error: ' +
error.message;

},

{

enableHighAccuracy:true,
maximumAge:0,
timeout:15000

}

);

}

async function getAddress(){

if(lat===null || lng===null)
return;

try{

let response =
await fetch(
'https://nominatim.openstreetmap.org/reverse?format=json&lat=' +
lat +
'&lon=' +
lng
);

let data =
await response.json();

address =
data.display_name || '';

}catch(error){

address='';

}

}

function capture(){

if(!stream){

alert('Pehle camera start karein.');

return;

}

if(lat===null || lng===null){

alert(
'GPS location milne tak wait karein.'
);

return;

}

if(acc>100){

alert(
'GPS accuracy ' +
Math.round(acc) +
' m hai.\n100 m se kam hone par capture karein.'
);

return;

}

let video =
document.getElementById('video');

let canvas =
document.getElementById('canvas');

canvas.width =
video.videoWidth;

canvas.height =
video.videoHeight;

if(!canvas.width){

alert(
'Camera frame ready nahi hai.'
);

return;

}

let ctx =
canvas.getContext('2d');

ctx.drawImage(
video,
0,
0,
canvas.width,
canvas.height
);

let fontSize =
Math.max(
18,
Math.floor(canvas.width/45)
);

let barHeight =
fontSize * 4;

ctx.fillStyle =
'rgba(0,0,0,.65)';

ctx.fillRect(
0,
canvas.height-barHeight,
canvas.width,
barHeight
);

ctx.fillStyle =
'#fff';

ctx.font =
fontSize + 'px Arial';

ctx.fillText(
employee.name +
' | ' +
new Date().toLocaleString('en-IN'),

12,
canvas.height-barHeight+fontSize
);

ctx.fillText(
'Lat: ' +
lat.toFixed(6) +
' | Lng: ' +
lng.toFixed(6),

12,
canvas.height-barHeight+fontSize*2
);

ctx.fillText(
'GPS Accuracy: ' +
Math.round(acc) +
' m',

12,
canvas.height-barHeight+fontSize*3
);

photoBase64 =
canvas.toDataURL(
'image/jpeg',
0.85
);

document.getElementById('photo').src =
photoBase64;

document.getElementById('photo').style.display =
'block';

document.getElementById('retake').style.display =
'block';

stopCamera();

check();

}

function stopCamera(){

if(stream){

stream
.getTracks()
.forEach(function(track){

track.stop();

});

stream=null;

}

document.getElementById('video').style.display =
'none';

document.getElementById('capture').style.display =
'none';

document.getElementById('stop').style.display =
'none';

if(!photoBase64){

document.getElementById('start').style.display =
'block';

}

}

function retake(){

photoBase64=null;

document.getElementById('photo').style.display =
'none';

document.getElementById('retake').style.display =
'none';

document.getElementById('start').style.display =
'block';

check();

startCamera();

}

function check(){

let declaration =
document.getElementById('decl').checked;

document.getElementById('submit').disabled =
!(
photoBase64 &&
lat!==null &&
lng!==null &&
declaration
);

}

async function submitVisit(){

let status =
document.getElementById('status');

let button =
document.getElementById('submit');

let city =
document.getElementById('city').value.trim();

let firm =
document.getElementById('firm').value.trim();

let customer =
document.getElementById('customer').value.trim();

let pin =
document.getElementById('pin').value.trim();

let remarks =
document.getElementById('remarks').value.trim();

if(
!city ||
!firm ||
!customer ||
!/^\d{6}$/.test(pin)
){

status.textContent =
'❌ सभी required fields सही भरें.';

return;

}

if(!photoBase64 || lat===null){

status.textContent =
'❌ Live photo और GPS required.';

return;

}

if(acc>100){

status.textContent =
'❌ GPS accuracy 100 m से ज्यादा है.';

return;

}

button.disabled=true;

status.textContent =
'Saving...';

try{

let response =
await fetch(
API_URL,
{

method:'POST',

headers:{
'Content-Type':
'text/plain;charset=utf-8'
},

body:JSON.stringify({

action:'saveVisit',

empId:employee.empId,

empName:employee.name,

hq:employee.hq,

visitCity:city,

firmName:firm,

pinCode:pin,

contactPerson:customer,

lat:lat,

lng:lng,

accuracy:acc,

geoLocation:
lat.toFixed(6) +
',' +
lng.toFixed(6),

address:address,

remarks:remarks,

photoBase64:
photoBase64

})

}

);

let result =
await response.json();

if(
result.status ===
'duplicate'
){

status.textContent =
'⚠️ Is customer ki entry aaj pehle se ho chuki hai.';

button.disabled=false;

return;

}

if(
result.status !==
'success'
){

throw new Error(
result.message ||
'Save failed'
);

}

status.textContent =
'✅ Visit entry saved successfully.';

clearForm();

}catch(error){

status.textContent =
'❌ ' +
error.message;

button.disabled=false;

}

}

function clearForm(){

document.getElementById('city').value='';
document.getElementById('firm').value='';
document.getElementById('customer').value='';
document.getElementById('pin').value='';
document.getElementById('remarks').value='';

document.getElementById('decl').checked=false;

photoBase64=null;
lat=null;
lng=null;
acc=null;
address='';

document.getElementById('photo').style.display =
'none';

document.getElementById('retake').style.display =
'none';

document.getElementById('start').style.display =
'block';

document.getElementById('gps').textContent =
'📍 GPS: Waiting...';

check();

}

function logout(){

stopCamera();

employee={};

photoBase64=null;
lat=null;
lng=null;
acc=null;
address='';

document.getElementById('form').style.display =
'none';

document.getElementById('login').style.display =
'block';

document.getElementById('emp').value='';

document.getElementById('ls').textContent='';

}

</script>

</body>
</html>
