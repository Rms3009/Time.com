<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Country & Time Detector</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      text-align: center;
      padding: 50px;
    }
    .box {
      background: white;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      display: inline-block;
    }
    h2 {
      color: #333;
    }
    #time {
      font-size: 24px;
      margin-top: 10px;
    }
    #country {
      font-size: 18px;
      color: #555;
    }
  </style>
</head>
<body>
  <div class="box">
    <h2>Your Local Time and Country</h2>
    <div id="time">Loading time...</div>
    <div id="country">Detecting country...</div>
  </div>

  <script>
    // Time display
    function updateTime() {
      const now = new Date();
      const options = { hour: '2-digit', minute: '2-digit', second: '2-digit' };
      document.getElementById("time").innerText = now.toLocaleTimeString(undefined, options);
    }

    // Country detection
    function detectCountry() {
      const timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
      fetch(`https://worldtimeapi.org/api/timezone/${timeZone}`)
        .then(res => res.json())
        .then(data => {
          const country = data.timezone.split('/')[0].replace('_', ' ');
          document.getElementById("country").innerText = "Approximate Region: " + country;
        })
        .catch(() => {
          document.getElementById("country").innerText = "Country detection failed.";
        });
    }

    // Run
    updateTime();
    setInterval(updateTime, 1000);
    detectCountry();
  </script>
</body>
</html>
