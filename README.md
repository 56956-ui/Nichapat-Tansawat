<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<title>แปลภาษามือเป็นภาษาไทย</title>
<style>
body { text-align:center; font-family:sans-serif; }
video { width: 400px; border:2px solid black; }
#result { font-size: 24px; color: blue; margin-top: 10px; }
</style>
</head>
<body>

<h2>ระบบแปลภาษามือ → ภาษาไทย</h2>

<video id="video" autoplay playsinline></video>
<p id="result">กำลังตรวจจับ...</p>

<script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands/hands.js"></script>

<script>
const video = document.getElementById("video");
const result = document.getElementById("result");

async function startCamera() {
  const stream = await navigator.mediaDevices.getUserMedia({ video: true });
  video.srcObject = stream;
}
startCamera();

const hands = new Hands({
  locateFile: file =>
    `https://cdn.jsdelivr.net/npm/@mediapipe/hands/${file}`
});

hands.setOptions({
  maxNumHands: 1,
  minDetectionConfidence: 0.7
});

hands.onResults(results => {
  if (results.multiHandLandmarks.length > 0) {
    const lm = results.multiHandLandmarks[0];

    if (lm[8].y < lm[6].y) {
      result.innerText = "สวัสดี";
    } else {
      result.innerText = "กำลังตรวจจับ...";
    }
  }
});

async function detect() {
  await hands.send({ image: video });
  requestAnimationFrame(detect);
}

video.onloadeddata = () => detect();
</script>

</body>
</html>

