<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Market Visit Entry</title>

<style>
*{box-sizing:border-box}

body{
  margin:0;
  font-family:Arial,sans-serif;
  background:#fff;
  min-height:100vh;
  padding:18px;
}

body:before{
  content:"";
  position:fixed;
  inset:0;
  background-position:center;
  background-size:cover;
  background-repeat:no-repeat;
  opacity:0;
  z-index:-2;
}

body:after{
  content:"";
  position:fixed;
  inset:0;
  background:rgba(255,255,255,.9);
  z-index:-1;
}

.container{
  width:100%;
  max-width:450px;
  margin:auto;
}

.logo{
  text-align:center;
  margin-bottom:15px;
}

.logo img{
  max-width:160px;
  max-height:100px;
}

h2,h3{
  text-align:center;
  margin:8px 0 18px;
}

label{
  display:block;
  font-weight:bold;
  font-size:14px;
  margin-top:12px;
}

input,textarea,button{
  width:100%;
  padding:11px;
  margin:7px 0;
  font-size:16px;
  font-family:Arial,sans-serif;
  border:1px solid #ccc;
  border-radius:6px;
}

button{
  background:#4285f4;
  color:#fff;
  border:none;
  cursor:pointer;
}

button:disabled{
  background:#aaa;
  cursor:not-allowed;
}

button.secondary{
  background:#eee;
  color:#333;
}

.info-box{
  background:#f2f6ff;
  padding:12px;
  border-radius:7px;
  margin-bottom:15px;
  font-size:14px;
}

.info-box div{
  padding:4px 0;
}

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
}

#canvas{
  display:none;
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

.small-note{
  font-size:12px;
  color:#666;
  margin-top:5px;
}
</style>
</head>

<body>

<div class="container">

<div class="logo">
<img id="companyLogo" alt="Company Logo">
</div>

<!-- LOGIN -->

<div id="loginSection">

<h3 id="loginTitle">Employee Login</h3>

<label>Employee ID</label>

<input
id="loginEmployee"
type="text"
placeholder="EMP001"
autocomplete="off">

<button
id="loginBtn"
type="button"
onclick="login()">
Login
</button>

<div id="loginStatus" class="status"></div>

</div>


<!-- FORM -->

<div id="formSection" style="display:none">

<h3 id="pageTitle">
Market Visit Entry
</h3>


<div class="info-box">

<div>
<strong>Employee:</strong>
<span id="infoName"></span>
(<span id="infoEmpId"></span>)
</div>

<div id="hqRow">
<strong>Head Quarter:</strong>
<span id="infoHQ"></span>
</div>

<div id="emailRow">
<strong>Email:</strong>
<span id="infoEmail"></span>
</div>

</div>


<label id="visitCityLabel">
To Visit City
</label>

<input
id="visitCity"
type="text"
placeholder="City / District / State">


<label id="firmNameLabel">
Visit Customer Firm Name
</label>

<input
id="firmName"
type="text"
placeholder="Firm name">


<label id="customerNameLabel">
Visit Customer Name
</label>

<input
id="customerName"
type="text"
placeholder="Customer name">


<label id="pinCodeLabel">
Visit Area Pin
</label>

<input
id="pinCode"
type="text"
placeholder="201301"
inputmode="numeric"
maxlength="6">


<div id="cameraSection">

<label>
Visited Customer Photo
</label>

<video
id="cameraVideo"
autoplay
playsinline>
</video>

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

<img
id="photoPreview"
alt="Captured Photo">

<button
id="retakeBtn"
type="button"
class="secondary"
onclick="retakePhoto()"
style="display:none">
🔄 Retake Photo
</button>

</div>


<div
id="gpsStatus"
class="gps-box">
📍 GPS: Waiting...
</div>


<div
id="remarksSection">

<label id="remarksLabel">
Visit Remarks / Discussion Notes
</label>

<textarea
id="remarks"
rows="3"
placeholder="Order discuss hua, samples diye...">
</textarea>

</div>


<div
id="declarationSection"
class="declaration">

<input
type="checkbox"
id="declaration"
onchange="checkSubmit()">

<span id="declarationText">
I confirm that the above attendance and
market visit details are true and correct.
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

</div>


<script>

/* =====================================================
   APPS SCRIPT URL
   ===================================================== */

const API_URL =
'https://script.google.com/macros/s/AKfycbyALO95cTRsYWt4yA5P1bijCghETY8G1MsxRyZpMpsmP6ChkRqqPsqoEo3oAUJZ2H7tUA/exec';


/* =====================================================
   GLOBALS
   ===================================================== */

let settings={};

let employee={};

let cameraStream=null;

let photoBase64=null;

let latitude=null;

let longitude=null;

let accuracy=null;

let address='';


/* =====================================================
   LOAD SETTINGS
   ===================================================== */

async function loadSettings(){

  try{

    const response=
      await fetch(
        API_URL+
        '?action=getSettings'
      );

    if(!response.ok){

      throw new Error(
        'Settings server error'
      );
    }

    settings=
      await response.json();

    if(!settings.success){

      throw new Error(
        'Settings load failed'
      );
    }

    applySettings();

  }catch(error){

    console.error(error);

    document.title=
      'Market Visit Entry';

  }

}


/* =====================================================
   APPLY SETTINGS
   ===================================================== */

function applySettings(){

  document.title=
    settings.pageTitle ||
    'Market Visit Entry';


  document
    .getElementById('pageTitle')
    .innerText=
      settings.pageTitle ||
      'Market Visit Entry';


  document
    .getElementById('loginTitle')
    .innerText=
      settings.loginTitle ||
      'Employee Login';


  document
    .getElementById('submitBtn')
    .innerText=
      settings.submitButtonText ||
      'Submit Visit';


  /* COMPANY LOGO */

  const logo=
    document
      .getElementById('companyLogo');

  if(settings.logoUrl){

    logo.src=
      settings.logoUrl;

    logo.style.display=
      'inline-block';

  }else{

    logo.style.display=
      'none';
  }


  /* BACKGROUND */

  if(settings.backgroundUrl){

    const style=
      document.createElement('style');

    const opacity=
      Number(
        settings.backgroundOpacity || 0
      );

    style.innerHTML=
      'body:before{'+
      'background-image:url("'+
      settings.backgroundUrl+
      '");'+
      'opacity:'+opacity+
      ';}';

    document.head.appendChild(style);
  }


  /* EMAIL */

  document
    .getElementById('emailRow')
    .style.display=
      settings.showEmail
      ? 'block'
      : 'none';


  /* HQ */

  document
    .getElementById('hqRow')
    .style.display=
      settings.showHQ
      ? 'block'
      : 'none';


  /* REMARKS */

  document
    .getElementById('remarksSection')
    .style.display=
      settings.showRemarks
      ? 'block'
      : 'none';


  /* DECLARATION */

  document
    .getElementById('declarationSection')
    .style.display=
      settings.declarationRequired
      ? 'flex'
      : 'none';


  /* CAMERA */

  document
    .getElementById('cameraSection')
    .style.display=
      settings.liveCameraRequired ||
      settings.photoRequired
      ? 'block'
      : 'none';


  /* HEADINGS */

  const h=
    settings.headings || {};


  setText(
    'visitCityLabel',
    h.visitCity
  );

  setText(
    'firmNameLabel',
    h.firmName
  );

  setText(
    'pinCodeLabel',
    h.pinCode
  );

  setText(
    'customerNameLabel',
    h.customerName
  );

  setText(
    'remarksLabel',
    settings.showRemarks
      ? 'Visit Remarks / Discussion Notes'
      : ''
  );


  /* REQUIRED MARKERS */

  if(settings.remarksRequired){

    document
      .getElementById('remarksLabel')
      .innerText=
      (h.remarks || 'Remarks')+
      ' *';
  }


  if(settings.addressRequired){

    /* Address is generated automatically
       from GPS reverse geocoding. */

  }

}


/* =====================================================
   TEXT HELPER
   ===================================================== */

function setText(id,value){

  if(
    value !== undefined &&
    value !== null &&
    value !== ''
  ){

    document
      .getElementById(id)
      .innerText=
        value;
  }

}


/* =====================================================
   LOGIN
   ===================================================== */

async function login(){

  const empId=
    document
      .getElementById(
        'loginEmployee'
      )
      .value
      .trim();

  const status=
    document
      .getElementById(
        'loginStatus'
      );


  if(!empId){

    status.innerText=
      'Employee ID bharein.';

    return;
  }


  status.innerText=
    'Checking...';


  try{

    const response=
      await fetch(
        API_URL+
        '?' +
        new URLSearchParams({

          action:
            'verifyEmployee',

          empId:
            empId

        })
      );


    const result=
      await response.json();


    if(!result.success){

      status.innerText=
        '❌ Employee ID nahi mila.';

      return;
    }


    employee=
      result;


    document
      .getElementById(
        'loginSection'
      )
      .style.display=
        'none';


    document
      .getElementById(
        'formSection'
      )
      .style.display=
        'block';


    document
      .getElementById(
        'infoName'
      )
      .innerText=
        result.name || '';


    document
      .getElementById(
        'infoEmpId'
      )
      .innerText=
        result.empId || '';


    document
      .getElementById(
        'infoHQ'
      )
      .innerText=
        result.hq || '';


    document
      .getElementById(
        'infoEmail'
      )
      .innerText=
        result.email || '';


    if(settings.gpsRequired){

      document
        .getElementById(
          'gpsStatus'
        )
        .innerText=
          '📍 GPS प्राप्त हो रहा है...';

      getGPS();

    }else{

      document
        .getElementById(
          'gpsStatus'
        )
        .innerText=
          '📍 GPS Not Required';
    }


  }catch(error){

    status.innerText=
      '❌ Error: '+
      error.message;
  }

}


/* =====================================================
   CAMERA
   ===================================================== */

async function startCamera(){

  if(
    !settings.liveCameraRequired &&
    !settings.photoRequired
  ){

    return;
  }


  const video=
    document
      .getElementById(
        'cameraVideo'
      );


  try{

    cameraStream=
      await navigator
        .mediaDevices
        .getUserMedia({

          video:{
            facingMode:{
              ideal:'environment'
            },

            width:{
              ideal:1280
            },

            height:{
              ideal:720
            }
          },

          audio:false

        });


    video.srcObject=
      cameraStream;

    video.style.display=
      'block';


    document
      .getElementById(
        'startCameraBtn'
      )
      .style.display=
        'none';


    document
      .getElementById(
        'captureBtn'
      )
      .style.display=
        'block';


    document
      .getElementById(
        'stopCameraBtn'
      )
      .style.display=
        'block';


  }catch(error){

    document
      .getElementById(
        'gpsStatus'
      )
      .innerText=
        '❌ Camera error: '+
        error.message;
  }

}


/* =====================================================
   GPS
   ===================================================== */

function getGPS(){

  if(!navigator.geolocation){

    document
      .getElementById(
        'gpsStatus'
      )
      .innerText=
        '❌ GPS available nahi hai.';

    return;
  }


  navigator
    .geolocation
    .getCurrentPosition(

      function(position){

        latitude=
          position.coords.latitude;

        longitude=
          position.coords.longitude;

        accuracy=
          position.coords.accuracy;


        document
          .getElementById(
            'gpsStatus'
          )
          .innerText=

          '📍 GPS Ready\n'+
          'Lat: '+
          latitude.toFixed(6)+
          '\nLng: '+
          longitude.toFixed(6)+
          '\nAccuracy: '+
          Math.round(accuracy)+
          ' meters';


        getAddress();

        checkSubmit();

      },


      function(error){

        document
          .getElementById(
            'gpsStatus'
          )
          .innerText=
            '❌ GPS error: '+
            error.message;
      },


      {

        enableHighAccuracy:true,

        maximumAge:0,

        timeout:15000

      }

    );

}


/* =====================================================
   ADDRESS
   ===================================================== */

async function getAddress(){

  if(
    latitude===null ||
    longitude===null
  ){
    return;
  }


  const url=
    'https://nominatim.openstreetmap.org/reverse'+
    '?format=json'+
    '&lat='+
    latitude+
    '&lon='+
    longitude;


  try{

    const response=
      await fetch(url);

    const data=
      await response.json();

    address=
      data.display_name || '';

  }catch(error){

    address='';
  }

}


/* =====================================================
   CAPTURE PHOTO
   ===================================================== */

function capturePhoto(){

  if(!cameraStream){

    alert(
      'Pehle camera start karein.'
    );

    return;
  }


  if(
    settings.gpsRequired &&
    (
      latitude===null ||
      longitude===null
    )
  ){

    alert(
      'GPS location milne tak wait karein.'
    );

    return;
  }


  if(
    settings.gpsRequired &&
    accuracy >
    Number(
      settings.maxGpsAccuracy ||
      100
    )
  ){

    alert(
      'GPS accuracy abhi '+
      Math.round(accuracy)+
      ' meters hai.\n\n'+
      'Open area me dobara try karein.'
    );

    return;
  }


  const video=
    document
      .getElementById(
        'cameraVideo'
      );


  const canvas=
    document
      .getElementById(
        'canvas'
      );


  canvas.width=
    video.videoWidth;

  canvas.height=
    video.videoHeight;


  const ctx=
    canvas.getContext('2d');


  ctx.drawImage(
    video,
    0,
    0,
    canvas.width,
    canvas.height
  );


  /* WATERMARK */

  if(settings.watermarkRequired){

    const now=
      new Date();

    const dateTime=
      now.toLocaleString('en-IN');


    const line1=
      employee.name+
      ' | '+
      dateTime;


    const line2=
      settings.gpsRequired
      ? 'Lat: '+
        latitude.toFixed(6)+
        ' | Lng: '+
        longitude.toFixed(6)
      : '';


    const line3=
      settings.gpsRequired
      ? 'GPS Accuracy: '+
        Math.round(accuracy)+
        ' m'
      : '';


    const fontSize=
      Math.max(
        18,
        Math.floor(
          canvas.width/45
        )
      );


    const barHeight=
      fontSize*4;


    ctx.fillStyle=
      'rgba(0,0,0,0.65)';


    ctx.fillRect(
      0,
      canvas.height-barHeight,
      canvas.width,
      barHeight
    );


    ctx.fillStyle=
      '#ffffff';


    ctx.font=
      fontSize+
      'px Arial';


    ctx.fillText(
      line1,
      12,
      canvas.height-
      barHeight+
      fontSize
    );


    ctx.fillText(
      line2,
      12,
      canvas.height-
      barHeight+
      fontSize*2
    );


    ctx.fillText(
      line3,
      12,
      canvas.height-
      barHeight+
      fontSize*3
    );
  }


  photoBase64=
    canvas.toDataURL(
      'image/jpeg',
      0.85
    );


  document
    .getElementById(
      'photoPreview'
    )
    .src=
      photoBase64;


  document
    .getElementById(
      'photoPreview'
    )
    .style.display=
      'block';


  document
    .getElementById(
      'retakeBtn'
    )
    .style.display=
      'block';


  stopCamera();

  checkSubmit();

}


/* =====================================================
   STOP CAMERA
   ===================================================== */

function stopCamera(){

  if(cameraStream){

    cameraStream
      .getTracks()
      .forEach(
        track=>{
          track.stop();
        }
      );

    cameraStream=null;
  }


  document
    .getElementById(
      'cameraVideo'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'captureBtn'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'stopCameraBtn'
    )
    .style.display=
      'none';


  if(!photoBase64){

    document
      .getElementById(
        'startCameraBtn'
      )
      .style.display=
        'block';
  }

}


/* =====================================================
   RETAKE
   ===================================================== */

function retakePhoto(){

  photoBase64=null;

  document
    .getElementById(
      'photoPreview'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'retakeBtn'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'startCameraBtn'
    )
    .style.display=
      'block';


  checkSubmit();

  startCamera();

}


/* =====================================================
   CHECK SUBMIT
   ===================================================== */

function checkSubmit(){

  const declaration=
    settings.declarationRequired
    ? document
        .getElementById(
          'declaration'
        )
        .checked
    : true;


  const gpsOK=
    settings.gpsRequired
    ? (
        latitude!==null &&
        longitude!==null &&
        Number.isFinite(accuracy) &&
        accuracy <=
        Number(
          settings.maxGpsAccuracy ||
          100
        )
      )
    : true;


  const photoOK=
    settings.photoRequired
    ? !!photoBase64
    : true;


  document
    .getElementById(
      'submitBtn'
    )
    .disabled=
      !(
        declaration &&
        gpsOK &&
        photoOK
      );
}


/* =====================================================
   SUBMIT
   ===================================================== */

async function submitVisit(){

  const status=
    document
      .getElementById(
        'status'
      );


  const btn=
    document
      .getElementById(
        'submitBtn'
      );


  const visitCity=
    document
      .getElementById(
        'visitCity'
      )
      .value
      .trim();


  const firmName=
    document
      .getElementById(
        'firmName'
      )
      .value
      .trim();


  const customerName=
    document
      .getElementById(
        'customerName'
      )
      .value
      .trim();


  const pinCode=
    document
      .getElementById(
        'pinCode'
      )
      .value
      .trim();


  const remarks=
    document
      .getElementById(
        'remarks'
      )
      .value
      .trim();


  if(
    !visitCity ||
    !firmName ||
    !customerName ||
    !pinCode
  ){

    status.innerText=
      '❌ Saari required fields bharein.';

    return;
  }


  if(
    settings.remarksRequired &&
    !remarks
  ){

    status.innerText=
      '❌ Remarks required.';

    return;
  }


  if(
    settings.photoRequired &&
    !photoBase64
  ){

    status.innerText=
      '❌ Live photo capture karein.';

    return;
  }


  if(
    settings.gpsRequired &&
    (
      latitude===null ||
      longitude===null
    )
  ){

    status.innerText=
      '❌ GPS required.';

    return;
  }


  btn.disabled=true;

  status.innerText=
    'Saving...';


  try{

    const response=
      await fetch(

        API_URL,

        {

          method:'POST',

          headers:{
            'Content-Type':
              'text/plain;charset=utf-8'
          },

          body:JSON.stringify({

            action:
              'saveVisit',

            empId:
              employee.empId,

            empName:
              employee.name,

            hq:
              employee.hq,

            visitCity:
              visitCity,

            firmName:
              firmName,

            pinCode:
              pinCode,

            contactPerson:
              customerName,

            lat:
              latitude,

            lng:
              longitude,

            accuracy:
              accuracy,

            geoLocation:
              settings.gpsRequired
              ? latitude.toFixed(6)+
                ','+
                longitude.toFixed(6)
              : '',

            address:
              address,

            remarks:
              remarks,

            photoBase64:
              photoBase64

          })
        }
      );


    const result=
      await response.json();


    if(
      result.status===
      'duplicate'
    ){

      status.innerText=
        '⚠️ Is customer ki entry aaj pehle se ho chuki hai.';

      btn.disabled=false;

      return;
    }


    if(
      result.status!==
      'success'
    ){

      throw new Error(
        result.message ||
        'Save failed'
      );
    }


    status.innerText=
      '✅ Visit entry saved successfully.';


    clearForm();


  }catch(error){

    status.innerText=
      '❌ Error: '+
      error.message;

    btn.disabled=false;
  }

}


/* =====================================================
   CLEAR FORM
   ===================================================== */

function clearForm(){

  document
    .getElementById(
      'visitCity'
    )
    .value='';


  document
    .getElementById(
      'firmName'
    )
    .value='';


  document
    .getElementById(
      'customerName'
    )
    .value='';


  document
    .getElementById(
      'pinCode'
    )
    .value='';


  document
    .getElementById(
      'remarks'
    )
    .value='';


  document
    .getElementById(
      'declaration'
    )
    .checked=false;


  photoBase64=null;

  latitude=null;

  longitude=null;

  accuracy=null;

  address='';


  document
    .getElementById(
      'photoPreview'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'retakeBtn'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'startCameraBtn'
    )
    .style.display=
      'block';


  document
    .getElementById(
      'gpsStatus'
    )
    .innerText=
      settings.gpsRequired
      ? '📍 GPS: Waiting...'
      : '📍 GPS Not Required';


  document
    .getElementById(
      'submitBtn'
    )
    .disabled=true;

}


/* =====================================================
   LOGOUT
   ===================================================== */

function logout(){

  stopCamera();

  employee={};

  photoBase64=null;

  latitude=null;

  longitude=null;

  accuracy=null;

  address='';


  document
    .getElementById(
      'formSection'
    )
    .style.display=
      'none';


  document
    .getElementById(
      'loginSection'
    )
    .style.display=
      'block';


  document
    .getElementById(
      'loginEmployee'
    )
    .value='';


  document
    .getElementById(
      'loginStatus'
    )
    .innerText='';

}


/* =====================================================
   START
   ===================================================== */

loadSettings();

</script>

</body>
</html>
