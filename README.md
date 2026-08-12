<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Market Visit</title>

<style>
*{box-sizing:border-box}
body{
font-family:Arial,sans-serif;
max-width:450px;
margin:auto;
padding:18px;
background:#fff;
background-size:cover;
background-position:center;
background-attachment:fixed;
}
body:before{
content:"";
position:fixed;
inset:0;
z-index:-1;
background:rgba(255,255,255,.92);
}
h3{margin-top:5px}
input,textarea,button{
width:100%;
padding:11px;
margin:7px 0;
font-size:16px;
font-family:Arial,sans-serif;
}
button{
background:#4285f4;
color:#fff;
border:0;
border-radius:6px;
cursor:pointer;
}
button:disabled{background:#aaa}
button.secondary{background:#eee;color:#333}
label{
display:block;
font-weight:bold;
font-size:14px;
margin-top:12px;
}
.logo{text-align:center;margin-bottom:12px}
.logo img{max-width:160px;max-height:100px}
.info-box{
background:#f2f6ff;
border-radius:7px;
padding:12px;
margin-bottom:15px;
font-size:14px;
}
.info-box div{padding:3px 0}
#cameraVideo{
width:100%;
background:#000;
border-radius:8px;
display:none;
}
#photoPreview{
width:100%;
border-radius:8px;
display:none;
margin-top:10px;
border:1px solid #ddd;
}
.status{
margin-top:10px;
font-weight:bold;
white-space:pre-line;
}
.gps-box{
background:#f5f5f5;
padding:10px;
border-radius:6px;
font-size:13px;
margin-top:10px;
white-space:pre-line;
}
.declaration{
display:flex;
gap:8px;
align-items:flex-start;
margin-top:15px;
background:#fff8e1;
padding:10px;
border-radius:6px;
}
.declaration input{
width:auto;
margin-top:4px;
}
#canvas{display:none}
.small-note{
font-size:12px;
color:#666;
margin-top:5px;
}
.hidden{display:none!important}
</style>
</head>

<body>

<div class="logo">
<img id="companyLogo" src="" alt="Company Logo">
</div>

<div id="loginSection">

<h3 id="loginTitle">Employee Login</h3>

<label id="employeeIdLabel">Employee ID</label>

<input
type="text"
id="loginEmployee"
placeholder="EMP001"
autocomplete="off">

<button
type="button"
onclick="login()">
Login
</button>

<div id="loginStatus" class="status"></div>

</div>

<div id="formSection" class="hidden">

<h3 id="pageTitle">Market Visit Entry</h3>

<div class="info-box">

<div>
<strong id="employeeNameHeading">Employee Name:</strong>
<span id="infoName"></span>
(<span id="infoEmpId"></span>)
</div>

<div id="hqRow">
<strong id="hqHeading">Head Quarter:</strong>
<span id="infoHQ"></span>
</div>

<div id="emailRow">
<strong>Email:</strong>
<span id="infoEmail"></span>
</div>

</div>

<label id="visitCityLabel">To Visit City / District / State</label>

<input
type="text"
id="visitCity"
placeholder="e.g. Noida, UP">

<label id="firmNameLabel">Visit Customer Firm Name</label>

<input
type="text"
id="firmName"
placeholder="Firm name">

<label id="customerNameLabel">Visit Customer Name</label>

<input
type="text"
id="customerName"
placeholder="Customer name">

<label id="pinCodeLabel">Visit Area Pin Code</label>

<input
type="text"
id="pinCode"
placeholder="201301"
inputmode="numeric"
maxlength="6">

<div id="cameraSection">

<label id="photoLabel">
Visited Customer Photo - Live Camera Only
</label>

<video id="cameraVideo" autoplay playsinline></video>

<button
id="startCameraBtn"
type="button"
onclick="startCamera()">
📷 Start Live Camera
</button>

<button
id="captureBtn"
type="button"
onclick="capturePhoto()"
style="display:none">
📸 Capture Live Photo
</button>

<button
id="stopCameraBtn"
type="button"
class="secondary"
onclick="stopCamera()"
style="display:none">
Stop Camera
</button>

<canvas id="canvas"></canvas>

<img id="photoPreview" alt="Captured Photo">

<button
id="retakeBtn"
type="button"
class="secondary"
onclick="retakePhoto()"
style="display:none">
🔄 Retake Photo
</button>

</div>

<div id="gpsStatus" class="gps-box">
📍 GPS: Waiting...
</div>

<div class="small-note">
Photo केवल live camera frame से capture होगी।
Gallery upload का विकल्प नहीं है।
</div>

<div id="remarksSection">

<label id="remarksLabel">
Visit Remarks / Discussion Notes
</label>

<textarea
id="remarks"
rows="3"
placeholder="Order discuss hua, samples diye...">
</textarea>

</div>

<div id="declarationSection" class="declaration">

<input
type="checkbox"
id="declaration"
onchange="checkSubmit()">

<span id="declarationLabel">
I confirm that the above attendance and market visit details are true and correct.
</span>

</div>

<button
id="submitBtn"
type="button"
onclick="submitVisit()"
disabled>
Submit Visit
</button>

<div id="status" class="status"></div>

<button
type="button"
class="secondary"
onclick="logout()">
Logout
</button>

</div>

<script>

const API_URL='https://script.google.com/macros/s/AKfycbyALO95cTRsYWt4yA5P1bijCghETY8G1MsxRyZpMpsmP6ChkRqqPsqoEo3oAUJZ2H7tUA/exec';

let employee={};
let settings={};
let cameraStream=null;
let photoBase64=null;
let latitude=null;
let longitude=null;
let accuracy=null;
let address='';

function boolValue(v,def=true){
if(v===undefined||v===null||v==='')return def;
if(typeof v==='boolean')return v;
return['true','yes','1','on'].includes(String(v).toLowerCase());
}

async function apiGet(params){

const url=API_URL+'?'+new URLSearchParams(params).toString();

const response=await fetch(url);

if(!response.ok)
throw new Error('Server error: '+response.status);

return await response.json();
}

async function loadSettings(){

try{

const result=await apiGet({
action:'getSettings'
});

if(result.success){

settings=result;

applySettings();

}else{

console.log('Settings load failed',result);

}

}catch(error){

console.log('Settings error:',error);

}

}

function applySettings(){

document.title=
settings.pageTitle||
settings.companyName||
'Market Visit Entry';

document.getElementById('pageTitle').innerText=
settings.pageTitle||
settings.companyName||
'Market Visit Entry';

document.getElementById('loginTitle').innerText=
settings.loginTitle||
'Employee Login';

document.getElementById('submitBtn').innerText=
settings.submitButtonText||
'Submit Visit';

if(settings.logoUrl){

const logo=document.getElementById('companyLogo');

logo.src=settings.logoUrl;
logo.style.display='block';

}else{

document.getElementById('companyLogo').style.display='none';

}

if(settings.backgroundUrl){

document.body.style.backgroundImage=
"url('"+settings.backgroundUrl+"')";

}

const opacity=
parseFloat(settings.backgroundOpacity);

if(!isNaN(opacity)){

document.body.style.setProperty(
'--bg-opacity',
opacity
);

}

document.getElementById('employeeIdLabel').innerText=
settings.employeeIdHeading||
'Employee ID';

document.getElementById('employeeNameHeading').innerText=
settings.employeeNameHeading||
'Employee Name:';

document.getElementById('hqHeading').innerText=
settings.headQuarterHeading||
'Head Quarter:';

document.getElementById('visitCityLabel').innerText=
settings.visitCityHeading||
'To Visit City / District / State';

document.getElementById('firmNameLabel').innerText=
settings.firmNameHeading||
'Visit Customer Firm Name';

document.getElementById('pinCodeLabel').innerText=
settings.visitAreaPinHeading||
'Visit Area Pin Code';

document.getElementById('customerNameLabel').innerText=
settings.customerNameHeading||
'Visit Customer Name';

document.getElementById('photoLabel').innerText=
settings.photoHeading||
'Visited Customer Photo - Live Camera Only';

document.getElementById('remarksLabel').innerText=
settings.remarksHeading||
'Visit Remarks / Discussion Notes';

if(settings.declarationHeading){

document.getElementById('declarationLabel').innerText=
settings.declarationHeading;

}

if(boolValue(settings.showEmail,true)){

document.getElementById('emailRow').style.display='block';

}else{

document.getElementById('emailRow').style.display='none';

}

if(boolValue(settings.showHQ,true)){

document.getElementById('hqRow').style.display='block';

}else{

document.getElementById('hqRow').style.display='none';

}

if(boolValue(settings.showRemarks,true)){

document.getElementById('remarksSection').style.display='block';

}else{

document.getElementById('remarksSection').style.display='none';

}

if(!boolValue(settings.liveCameraRequired,true)&&
!boolValue(settings.photoRequired,true)){

document.getElementById('cameraSection').style.display='none';

}

if(!boolValue(settings.declarationRequired,true)){

document.getElementById('declarationSection').style.display='none';

}

}

async function login(){

const empId=
document.getElementById('loginEmployee').value.trim();

const status=
document.getElementById('loginStatus');

if(!empId){

status.innerText='Employee ID bharein.';

return;

}

status.innerText='Checking...';

try{

const result=await apiGet({
action:'verifyEmployee',
empId:empId
});

if(!result.success){

status.innerText='❌ Employee ID nahi mila.';

return;

}

employee=result;

document.getElementById('loginSection').style.display='none';
document.getElementById('formSection').classList.remove('hidden');

document.getElementById('infoName').innerText=
result.name||'';

document.getElementById('infoEmpId').innerText=
result.empId||'';

document.getElementById('infoHQ').innerText=
result.hq||'';

document.getElementById('infoEmail').innerText=
result.email||'';

if(boolValue(settings.gpsRequired,true)){

document.getElementById('gpsStatus').innerText=
'📍 GPS: Camera start karne par location li jayegi.';

}

}catch(error){

status.innerText='❌ Error: '+error.message;

}

}

async function startCamera(){

if(!boolValue(settings.liveCameraRequired,true)&&
!boolValue(settings.photoRequired,true)){

return;

}

const video=document.getElementById('cameraVideo');
const gpsStatus=document.getElementById('gpsStatus');

if(!navigator.mediaDevices||
!navigator.mediaDevices.getUserMedia){

gpsStatus.innerText=
'❌ Camera API available nahi hai. HTTPS Chrome use karein.';

return;

}

try{

cameraStream=
await navigator.mediaDevices.getUserMedia({

video:{
facingMode:{ideal:'environment'},
width:{ideal:1280},
height:{ideal:720}
},

audio:false

});

video.srcObject=cameraStream;
video.style.display='block';

document.getElementById('startCameraBtn').style.display='none';
document.getElementById('captureBtn').style.display='block';
document.getElementById('stopCameraBtn').style.display='block';

gpsStatus.innerText=
'📍 Camera ready.\nGPS प्राप्त हो रहा है...';

if(boolValue(settings.gpsRequired,true)){

getGPS();

}

}catch(error){

gpsStatus.innerText=
'❌ Camera access error: '+error.message+
'\n\nChrome में Camera = Allow करें.';

}

}

function getGPS(){

if(!navigator.geolocation){

document.getElementById('gpsStatus').innerText=
'❌ GPS available nahi hai.';

return;

}

navigator.geolocation.getCurrentPosition(

function(position){

latitude=position.coords.latitude;
longitude=position.coords.longitude;
accuracy=position.coords.accuracy;

document.getElementById('gpsStatus').innerText=
'📍 GPS Ready\nLat: '+latitude.toFixed(6)+
'\nLng: '+longitude.toFixed(6)+
'\nAccuracy: '+Math.round(accuracy)+' meters';

getAddress();

checkSubmit();

},

function(error){

document.getElementById('gpsStatus').innerText=
'❌ GPS error: '+error.message;

},

{
enableHighAccuracy:true,
maximumAge:0,
timeout:15000
}

);

}

async function getAddress(){

if(latitude===null||longitude===null)return;

const url=
'https://nominatim.openstreetmap.org/reverse'+
'?format=json&lat='+latitude+'&lon='+longitude;

try{

const response=await fetch(url);
const data=await response.json();

address=data.display_name||'';

}catch(error){

address='';

}

}

function capturePhoto(){

if(!cameraStream){

alert('Pehle camera start karein.');
return;

}

if(boolValue(settings.gpsRequired,true)&&
(latitude===null||longitude===null)){

alert('GPS location milne tak wait karein.');
return;

}

if(
accuracy!==null&&
settings.maxGPSAccuracy&&
accuracy>Number(settings.maxGPSAccuracy)
){

alert(
'GPS accuracy '+Math.round(accuracy)+
' meters hai.\nOpen area me dobara try karein.'
);

return;

}

const video=document.getElementById('cameraVideo');
const canvas=document.getElementById('canvas');

if(!video.videoWidth||!video.videoHeight){

alert('Camera frame ready nahi hai.');
return;

}

canvas.width=video.videoWidth;
canvas.height=video.videoHeight;

const ctx=canvas.getContext('2d');

ctx.drawImage(
video,
0,
0,
canvas.width,
canvas.height
);

if(boolValue(settings.watermarkRequired,true)){

addWatermark(ctx,canvas);

}

photoBase64=
canvas.toDataURL('image/jpeg',0.85);

document.getElementById('photoPreview').src=
photoBase64;

document.getElementById('photoPreview').style.display='block';

document.getElementById('retakeBtn').style.display='block';

stopCamera();

checkSubmit();

}

function addWatermark(ctx,canvas){

const now=new Date();

const dateTime=now.toLocaleString('en-IN');

const line1=
(employee.name||'')+' | '+dateTime;

const line2=
'Lat: '+latitude.toFixed(6)+
' | Lng: '+longitude.toFixed(6);

const line3=
'GPS Accuracy: '+Math.round(accuracy)+' m';

const fontSize=
Math.max(18,Math.floor(canvas.width/45));

const barHeight=fontSize*4;

ctx.fillStyle='rgba(0,0,0,.65)';

ctx.fillRect(
0,
canvas.height-barHeight,
canvas.width,
barHeight
);

ctx.fillStyle='#fff';
ctx.font=fontSize+'px Arial';

ctx.fillText(
line1,
12,
canvas.height-barHeight+fontSize
);

ctx.fillText(
line2,
12,
canvas.height-barHeight+fontSize*2
);

ctx.fillText(
line3,
12,
canvas.height-barHeight+fontSize*3
);

}

function stopCamera(){

if(cameraStream){

cameraStream.getTracks().forEach(
track=>track.stop()
);

cameraStream=null;

}

document.getElementById('cameraVideo').style.display='none';
document.getElementById('captureBtn').style.display='none';
document.getElementById('stopCameraBtn').style.display='none';

if(!photoBase64){

document.getElementById('startCameraBtn').style.display='block';

}

}

function retakePhoto(){

photoBase64=null;

document.getElementById('photoPreview').style.display='none';
document.getElementById('retakeBtn').style.display='none';
document.getElementById('startCameraBtn').style.display='block';

checkSubmit();

startCamera();

}

function checkSubmit(){

const gpsRequired=
boolValue(settings.gpsRequired,true);

const photoRequired=
boolValue(settings.photoRequired,true);

const declarationRequired=
boolValue(settings.declarationRequired,true);

const declaration=
document.getElementById('declaration').checked;

let ok=true;

if(gpsRequired&&
(latitude===null||longitude===null))
ok=false;

if(photoRequired&&!photoBase64)
ok=false;

if(declarationRequired&&!declaration)
ok=false;

document.getElementById('submitBtn').disabled=!ok;

}

async function submitVisit(){

const status=document.getElementById('status');
const btn=document.getElementById('submitBtn');

const visitCity=
document.getElementById('visitCity').value.trim();

const firmName=
document.getElementById('firmName').value.trim();

const customerName=
document.getElementById('customerName').value.trim();

const pinCode=
document.getElementById('pinCode').value.trim();

const remarks=
document.getElementById('remarks').value.trim();

if(!visitCity||
!firmName||
!customerName||
!pinCode){

status.innerText='❌ Saari required fields bharein.';
return;

}

if(boolValue(settings.photoRequired,true)&&!photoBase64){

status.innerText='❌ Live photo capture karein.';
return;

}

if(boolValue(settings.gpsRequired,true)&&
(latitude===null||longitude===null)){

status.innerText='❌ GPS required.';
return;

}

if(boolValue(settings.remarksRequired,false)&&!remarks){

status.innerText='❌ Remarks required.';
return;

}

if(boolValue(settings.declarationRequired,true)&&
!document.getElementById('declaration').checked){

status.innerText='❌ Declaration accept karein.';
return;

}

btn.disabled=true;
status.innerText='Saving...';

try{

const response=await fetch(
API_URL,
{
method:'POST',
headers:{
'Content-Type':'text/plain;charset=utf-8'
},
body:JSON.stringify({

action:'saveVisit',

empId:employee.empId,
empName:employee.name,
hq:employee.hq,

visitCity:visitCity,
firmName:firmName,
pinCode:pinCode,
contactPerson:customerName,

lat:latitude,
lng:longitude,
accuracy:accuracy,

geoLocation:
latitude!==null&&longitude!==null?
latitude.toFixed(6)+','+longitude.toFixed(6):'',

address:address,
remarks:remarks,
photoBase64:photoBase64

})
}
);

const result=await response.json();

if(result.status==='duplicate'){

status.innerText=
'⚠️ Is customer ki entry aaj pehle se ho chuki hai.';

btn.disabled=false;
return;

}

if(result.status!=='success'){

throw new Error(
result.message||'Save failed'
);

}

status.innerText=
'✅ Visit entry saved successfully.';

clearForm();

}catch(error){

status.innerText=
'❌ Error: '+error.message;

btn.disabled=false;

}

}

function clearForm(){

document.getElementById('visitCity').value='';
document.getElementById('firmName').value='';
document.getElementById('customerName').value='';
document.getElementById('pinCode').value='';
document.getElementById('remarks').value='';
document.getElementById('declaration').checked=false;

photoBase64=null;
latitude=null;
longitude=null;
accuracy=null;
address='';

document.getElementById('photoPreview').style.display='none';
document.getElementById('retakeBtn').style.display='none';
document.getElementById('startCameraBtn').style.display='block';

document.getElementById('gpsStatus').innerText=
'📍 GPS: Waiting...';

checkSubmit();

}

function logout(){

stopCamera();

employee={};
photoBase64=null;
latitude=null;
longitude=null;
accuracy=null;
address='';

document.getElementById('formSection').classList.add('hidden');
document.getElementById('loginSection').style.display='block';

document.getElementById('loginEmployee').value='';
document.getElementById('loginStatus').innerText='';

}

window.addEventListener(
'load',
function(){
loadSettings();
}
);

</script>

</body>
</html>
