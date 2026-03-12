<!DOCTYPE html>
<html>
<head>
  <title>Rickroll ASCII Animation</title>
  <style>
    body {
      background: black;
      color: lime;
      font-family: monospace;
      white-space: pre;
      font-size: 8px;
    }
  </style>
</head>
<body>
<pre id="ascii">Loading...</pre>

<script>
// ============================
// KONFIGURATION
// ============================
const width = 64; // bredd i pixlar
const chars = "012233332211"; // tecken för ASCII

// Rickroll video frames (för enkelhet, vi kan simulera med 1 bild upprepade gånger)
const frameUrls = [
  "https://img.unocero.com/2021/08/rickroll_4k-1024x768.jpeg",
  // du kan lägga till fler frames om du vill animera flera bilder
];

// Rickroll-länk som öppnas efter animation
const rickrollLink = "https://www.youtube.com/watch?v=dQw4w9WgXcQ";

// ============================
// FUNKTIONER
// ============================
const canvas = document.createElement("canvas");
const ctx = canvas.getContext("2d");
const asciiElement = document.getElementById("ascii");

canvas.width = width;

function loadImage(url){
  return new Promise((resolve) => {
    const img = new Image();
    img.crossOrigin = "anonymous";
    img.src = url;
    img.onload = () => resolve(img);
  });
}

function imageToASCII(img){
  const ratio = img.height / img.width;
  const height = Math.floor(width * ratio * 0.55);
  canvas.height = height;
  ctx.drawImage(img, 0, 0, width, height);
  const data = ctx.getImageData(0,0,width,height).data;

  let ascii = "";
  for(let y=0; y<height; y++){
    for(let x=0; x<width; x++){
      const i = (y*width + x)*4;
      const r = data[i];
      const g = data[i+1];
      const b = data[i+2];
      const brightness = (r+g+b)/3;
      const index = Math.floor(brightness/256*chars.length);
      ascii += chars[index];
    }
    ascii += "\n";
  }
  return ascii;
}

// ============================
// ANIMATION
// ============================
async function startAnimation(){
  const frames = [];
  for(const url of frameUrls){
    const img = await loadImage(url);
    frames.push(imageToASCII(img));
  }

  let frameIndex = 0;
  const totalFrames = frames.length;

  const interval = setInterval(() => {
    asciiElement.textContent = frames[frameIndex];
    frameIndex = (frameIndex + 1) % totalFrames;
  }, 200); // 200ms per frame

  // Kör i 10 sekunder sen öppna Rickroll-länk
  setTimeout(()=>{
    clearInterval(interval);
    window.open(rickrollLink, "_blank");
    asciiElement.textContent += "\n\nYOU JUST GOT RICKROLLED!";
  }, 10000);
}

// Starta
startAnimation();

</script>
</body>
</html>
