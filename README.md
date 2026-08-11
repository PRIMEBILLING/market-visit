<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Market Visit Entry</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 450px;
      margin: auto;
      padding: 18px;
      background: #ffffff;
    }

    h3 {
      margin-top: 5px;
    }

    input, textarea, button {
      width: 100%;
      box-sizing: border-box;
      padding: 11px;
      margin: 7px 0;
      font-size: 16px;
      font-family: Arial, sans-serif;
    }

    button {
      background: #4285f4;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:disabled {
      background: #aaa;
      cursor: not-allowed;
    }

    button.secondary {
      background: #eeeeee;
      color: #333333;
    }

    label {
      display: block;
      font-weight: bold;
      font-size: 14px;
      margin-top: 12px;
    }

    .logo {
      text-align: center;
      margin-bottom: 12px;
    }

    .logo img {
      max-width: 140px;
      max-height: 90px;
    }

    .info-box {
      background: #f2f6ff;
      border-radius: 7px;
      padding: 12px;
      margin-bottom: 15px;
      font-size: 14px;
    }

    .info-box div {
      padding: 3px 0;
    }

    #cameraVideo {
      width: 100%;
      background: #000000;
      border-radius: 8px;
      display: none;
    }

    #photoPreview {
      width: 100%;
      border-radius: 8px;
      display: none;
      margin-top: 10px;
      border: 1px solid #dddddd;
    }

    .status {
      margin-top: 10px;
      font-weight: bold;
      white-space: pre-line;
    }

    .gps-box {
      background: #f5f5f5;
      padding: 10px;
      border-radius: 6px;
      font-size: 13px;
      margin-top: 10px;
      white-space: pre-line;
    }

    .declaration {
      display: flex;
      gap: 8px;
      align-items: flex-start;
      margin-top: 15px;
      background: #fff8e1;
      padding: 10px;
      border-radius: 6px;
    }

    .declaration input {
      width: auto;
      margin-top: 4px;
    }

    #canvas {
      display: none;
    }

    .small-note {
      font-size: 12px;
      color: #666666;
      margin-top: 5px;
    }
  </style>
</head>

<body>

  <div class="logo">
    <img
      src="PASTE_LOGO_IMAGE_URL_HERE"
      alt="Company Logo"
      onerror="this.style.display='none'">
  </div>


  <!-- LOGIN -->

  <div id="loginSection">

    <h3>Employee Login</h3>

    <label>Employee ID</label>

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

    <div
      id="loginStatus"
      class="status">
    </div>

  </div>


  <!-- FORM -->

  <div
    id="formSection"
    style="display:none;">

    <h3>Market Visit Entry</h3>


    <div class="info-box">

      <div>
        <strong>Employee:</strong>
        <span id="infoName"></span>
        (<span id="infoEmpId"></span>)
      </div>

      <div>
        <strong>Head Quarter:</strong>
        <span id="infoHQ"></span>
      </div>

      <div>
        <strong>Email:</strong>
        <span id="infoEmail"></span>
      </div>

    </div>


    <label>
      To Visit City / District / State
    </label>

    <input
      type="text"
      id="visitCity"
      placeholder="e.g. Noida, UP">


    <label>
      Visit Customer Firm Name
    </label>

    <input
      type="text"
      id="firmName"
      placeholder="Firm name">


    <label>
      Visit Customer Name
    </label>

    <input
      type="text"
      id="customerName"
      placeholder="Customer name">


    <label>
      Visit Area Pin Code
    </label>

    <input
      type="text"
      id="pinCode"
      placeholder="201301"
      inputmode="numeric"
      maxlength="6">


    <label>
      Visited Customer Photo - Live Camera Only
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
      style="display:none;">
      📸 Capture Live Photo
    </button>


    <button
      id="stopCameraBtn"
      type="button"
      class="secondary"
      onclick="stopCamera()"
      style="display:none;">
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
      style="display:none;">
      🔄 Retake Photo
    </button>


    <div
      id="gpsStatus"
      class="gps-box">
      📍 GPS: Waiting...
    </div>


    <div class="small-note">
      Photo केवल live camera frame से capture होगी।
      Gallery upload का विकल्प नहीं है।
    </div>


    <label>
      Visit Remarks / Discussion Notes
    </label>

    <textarea
      id="remarks"
      rows="3"
      placeholder="Order discuss hua, samples diye..."></textarea>


    <div class="declaration">

      <input
        type="checkbox"
        id="declaration"
        onchange="checkSubmit()">

      <span>
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


    <div
      id="status"
      class="status">
    </div>


    <button
      type="button"
      class="secondary"
      onclick="logout()">
      Logout
    </button>

  </div>


<script>

  /* =====================================================
     IMPORTANT
     =====================================================

     Put your deployed Google Apps Script Web App URL here.

     Example:
     https://script.google.com/macros/s/XXXXXXXX/exec
  */

  const API_URL =
    'https://script.google.com/macros/s/AKfycbyALO95cTRsYWt4yA5P1bijCghETY8G1MsxRyZpMpsmP6ChkRqqPsqoEo3oAUJZ2H7tUA/exec';


  /* =====================================================
     GLOBAL VARIABLES
     ===================================================== */

  let employee = {};

  let cameraStream = null;

  let photoBase64 = null;

  let latitude = null;

  let longitude = null;

  let accuracy = null;

  let address = '';


  /* =====================================================
     API HELPER
     ===================================================== */

  async function apiGet(params) {

    const url =
      API_URL +
      '?' +
      new URLSearchParams(params).toString();

    const response =
      await fetch(url);

    if (!response.ok) {
      throw new Error(
        'Server error: ' +
        response.status
      );
    }

    return await response.json();
  }


  /* =====================================================
     LOGIN
     ===================================================== */

  async function login() {

    const empId =
      document
        .getElementById('loginEmployee')
        .value
        .trim();

    const status =
      document
        .getElementById('loginStatus');


    if (!empId) {

      status.innerText =
        'Employee ID bharein.';

      return;
    }


    if (
      !API_URL ||
      API_URL.indexOf('PASTE_') === 0
    ) {

      status.innerText =
        '❌ Apps Script Web App URL set karein.';

      return;
    }


    status.innerText =
      'Checking...';


    try {

      const result =
        await apiGet({
          action: 'verifyEmployee',
          empId: empId
        });


      if (!result.success) {

        status.innerText =
          '❌ Employee ID nahi mila.';

        return;
      }


      employee = result;


      document
        .getElementById('loginSection')
        .style.display =
        'none';


      document
        .getElementById('formSection')
        .style.display =
        'block';


      document
        .getElementById('infoName')
        .innerText =
        result.name;


      document
        .getElementById('infoEmpId')
        .innerText =
        result.empId;


      document
        .getElementById('infoHQ')
        .innerText =
        result.hq;


      document
        .getElementById('infoEmail')
        .innerText =
        result.email || '';


    } catch (error) {

      status.innerText =
        '❌ Error: ' +
        error.message;

    }

  }


  /* =====================================================
     CAMERA
     ===================================================== */

  async function startCamera() {

    const video =
      document
        .getElementById('cameraVideo');

    const gpsStatus =
      document
        .getElementById('gpsStatus');


    if (
      !navigator.mediaDevices ||
      !navigator.mediaDevices.getUserMedia
    ) {

      gpsStatus.innerText =
        '❌ Camera API available nahi hai.\n' +
        'Page ko HTTPS Chrome me directly open karein.';

      return;
    }


    try {

      cameraStream =
        await navigator
          .mediaDevices
          .getUserMedia({

            video: {

              facingMode: {
                ideal: 'environment'
              },

              width: {
                ideal: 1280
              },

              height: {
                ideal: 720
              }

            },

            audio: false

          });


      video.srcObject =
        cameraStream;


      video.style.display =
        'block';


      document
        .getElementById('startCameraBtn')
        .style.display =
        'none';


      document
        .getElementById('captureBtn')
        .style.display =
        'block';


      document
        .getElementById('stopCameraBtn')
        .style.display =
        'block';


      gpsStatus.innerText =
        '📍 Camera ready.\nGPS प्राप्त हो रहा है...';


      getGPS();


    } catch (error) {

      gpsStatus.innerText =
        '❌ Camera access error: ' +
        error.message +
        '\n\nChrome में इस page को सीधे खोलें और Camera = Allow करें.';

    }

  }


  /* =====================================================
     GPS
     ===================================================== */

  function getGPS() {

    if (!navigator.geolocation) {

      document
        .getElementById('gpsStatus')
        .innerText =
        '❌ GPS available nahi hai.';

      return;
    }


    navigator
      .geolocation
      .getCurrentPosition(

        function(position) {

          latitude =
            position.coords.latitude;

          longitude =
            position.coords.longitude;

          accuracy =
            position.coords.accuracy;


          document
            .getElementById('gpsStatus')
            .innerText =

            '📍 GPS Ready\n' +
            'Lat: ' +
            latitude.toFixed(6) +
            '\nLng: ' +
            longitude.toFixed(6) +
            '\nAccuracy: ' +
            Math.round(accuracy) +
            ' meters';


          getAddress();

        },


        function(error) {

          document
            .getElementById('gpsStatus')
            .innerText =
            '❌ GPS error: ' +
            error.message;

        },


        {

          enableHighAccuracy: true,

          maximumAge: 0,

          timeout: 15000

        }

      );

  }


  /* =====================================================
     ADDRESS
     ===================================================== */

  async function getAddress() {

    if (
      latitude === null ||
      longitude === null
    ) {
      return;
    }


    const url =
      'https://nominatim.openstreetmap.org/reverse' +
      '?format=json' +
      '&lat=' +
      latitude +
      '&lon=' +
      longitude;


    try {

      const response =
        await fetch(url);

      const data =
        await response.json();

      address =
        data.display_name || '';

    } catch (error) {

      address = '';

    }

  }


  /* =====================================================
     CAPTURE
     ===================================================== */

  function capturePhoto() {

    if (!cameraStream) {

      alert(
        'Pehle camera start karein.'
      );

      return;
    }


    if (
      latitude === null ||
      longitude === null
    ) {

      alert(
        'GPS location milne tak wait karein.'
      );

      return;
    }


    if (
      accuracy !== null &&
      accuracy > 100
    ) {

      alert(
        'GPS accuracy abhi ' +
        Math.round(accuracy) +
        ' meters hai.\n\n' +
        'Open area me jaakar dobara try karein.'
      );

      return;
    }


    const video =
      document
        .getElementById('cameraVideo');


    const canvas =
      document
        .getElementById('canvas');


    if (
      !video.videoWidth ||
      !video.videoHeight
    ) {

      alert(
        'Camera frame ready nahi hai.'
      );

      return;
    }


    canvas.width =
      video.videoWidth;

    canvas.height =
      video.videoHeight;


    const ctx =
      canvas.getContext('2d');


    /* LIVE VIDEO FRAME */

    ctx.drawImage(
      video,
      0,
      0,
      canvas.width,
      canvas.height
    );


    /* =================================================
       WATERMARK
       ================================================= */

    const now =
      new Date();


    const dateTime =
      now.toLocaleString('en-IN');


    const line1 =
      employee.name +
      ' | ' +
      dateTime;


    const line2 =
      'Lat: ' +
      latitude.toFixed(6) +
      ' | Lng: ' +
      longitude.toFixed(6);


    const line3 =
      'GPS Accuracy: ' +
      Math.round(accuracy) +
      ' m';


    const fontSize =
      Math.max(
        18,
        Math.floor(
          canvas.width / 45
        )
      );


    const barHeight =
      fontSize * 4;


    ctx.fillStyle =
      'rgba(0,0,0,0.65)';


    ctx.fillRect(
      0,
      canvas.height - barHeight,
      canvas.width,
      barHeight
    );


    ctx.fillStyle =
      '#ffffff';


    ctx.font =
      fontSize +
      'px Arial';


    ctx.fillText(
      line1,
      12,
      canvas.height -
        barHeight +
        fontSize
    );


    ctx.fillText(
      line2,
      12,
      canvas.height -
        barHeight +
        fontSize * 2
    );


    ctx.fillText(
      line3,
      12,
      canvas.height -
        barHeight +
        fontSize * 3
    );


    /* JPEG */

    photoBase64 =
      canvas.toDataURL(
        'image/jpeg',
        0.85
      );


    document
      .getElementById('photoPreview')
      .src =
      photoBase64;


    document
      .getElementById('photoPreview')
      .style.display =
      'block';


    document
      .getElementById('retakeBtn')
      .style.display =
      'block';


    stopCamera();


    checkSubmit();

  }


  /* =====================================================
     STOP CAMERA
     ===================================================== */

  function stopCamera() {

    if (cameraStream) {

      cameraStream
        .getTracks()
        .forEach(function(track) {

          track.stop();

        });


      cameraStream = null;

    }


    document
      .getElementById('cameraVideo')
      .style.display =
      'none';


    document
      .getElementById('captureBtn')
      .style.display =
      'none';


    document
      .getElementById('stopCameraBtn')
      .style.display =
      'none';


    if (!photoBase64) {

      document
        .getElementById('startCameraBtn')
        .style.display =
        'block';

    }

  }


  /* =====================================================
     RETAKE
     ===================================================== */

  function retakePhoto() {

    photoBase64 = null;


    document
      .getElementById('photoPreview')
      .style.display =
      'none';


    document
      .getElementById('retakeBtn')
      .style.display =
      'none';


    document
      .getElementById('startCameraBtn')
      .style.display =
      'block';


    checkSubmit();


    startCamera();

  }


  /* =====================================================
     SUBMIT ENABLE
     ===================================================== */

  function checkSubmit() {

    const declaration =
      document
        .getElementById('declaration')
        .checked;


    document
      .getElementById('submitBtn')
      .disabled =

      !(
        photoBase64 &&
        latitude !== null &&
        longitude !== null &&
        declaration
      );

  }


  /* =====================================================
     SUBMIT
     ===================================================== */

  async function submitVisit() {

    const status =
      document
        .getElementById('status');


    const btn =
      document
        .getElementById('submitBtn');


    const visitCity =
      document
        .getElementById('visitCity')
        .value
        .trim();


    const firmName =
      document
        .getElementById('firmName')
        .value
        .trim();


    const customerName =
      document
        .getElementById('customerName')
        .value
        .trim();


    const pinCode =
      document
        .getElementById('pinCode')
        .value
        .trim();


    const remarks =
      document
        .getElementById('remarks')
        .value
        .trim();


    if (
      !visitCity ||
      !firmName ||
      !customerName ||
      !pinCode
    ) {

      status.innerText =
        '❌ Saari required fields bharein.';

      return;
    }


    if (!photoBase64) {

      status.innerText =
        '❌ Live photo capture karein.';

      return;
    }


    if (
      latitude === null ||
      longitude === null
    ) {

      status.innerText =
        '❌ GPS required.';

      return;
    }


    btn.disabled = true;

    status.innerText =
      'Saving...';


    /*
      IMPORTANT:
      This version expects the Apps Script backend
      to provide a POST endpoint.

      The final backend will validate the employee,
      GPS, duplicate visit and save the photo.
    */

    try {

      const response =
        await fetch(
          API_URL,
          {
            method: 'POST',

            headers: {
              'Content-Type':
                'text/plain;charset=utf-8'
            },

            body: JSON.stringify({

              action: 'saveVisit',

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
                latitude.toFixed(6) +
                ',' +
                longitude.toFixed(6),

              address:
                address,

              remarks:
                remarks,

              photoBase64:
                photoBase64

            })

          }
        );


      const result =
        await response.json();


      if (
        result.status ===
        'duplicate'
      ) {

        status.innerText =
          '⚠️ Is customer ki entry aaj pehle se ho chuki hai.';

        btn.disabled = false;

        return;
      }


      if (
        result.status !==
        'success'
      ) {

        throw new Error(
          result.message ||
          'Save failed'
        );

      }


      status.innerText =
        '✅ Visit entry saved successfully.';


      clearForm();


    } catch (error) {

      status.innerText =
        '❌ Error: ' +
        error.message;

      btn.disabled = false;

    }

  }


  /* =====================================================
     CLEAR FORM
     ===================================================== */

  function clearForm() {

    document
      .getElementById('visitCity')
      .value = '';


    document
      .getElementById('firmName')
      .value = '';


    document
      .getElementById('customerName')
      .value = '';


    document
      .getElementById('pinCode')
      .value = '';


    document
      .getElementById('remarks')
      .value = '';


    document
      .getElementById('declaration')
      .checked = false;


    photoBase64 = null;

    latitude = null;

    longitude = null;

    accuracy = null;

    address = '';


    document
      .getElementById('photoPreview')
      .style.display =
      'none';


    document
      .getElementById('retakeBtn')
      .style.display =
      'none';


    document
      .getElementById('startCameraBtn')
      .style.display =
      'block';


    document
      .getElementById('gpsStatus')
      .innerText =
      '📍 GPS: Waiting...';


    checkSubmit();

  }


  /* =====================================================
     LOGOUT
     ===================================================== */

  function logout() {

    stopCamera();


    employee = {};

    photoBase64 = null;

    latitude = null;

    longitude = null;

    accuracy = null;

    address = '';


    document
      .getElementById('formSection')
      .style.display =
      'none';


    document
      .getElementById('loginSection')
      .style.display =
      'block';


    document
      .getElementById('loginEmployee')
      .value =
      '';


    document
      .getElementById('loginStatus')
      .innerText =
      '';

  }

</script>

</body>
</html>
