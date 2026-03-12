<!DOCTYPE html>
<html>
<head>
    <title>Rickroll Surprise 🎵</title>
</head>
<body>
    <h1>Välkommen! Klicka inte… 😏</h1>

    <script>
        // Din video-länk
        const videoUrl = "https://www.youtube.com/embed/dQw4w9WgXcQ?autoplay=1";

        // Skapar iframe automatiskt
        const iframe = document.createElement("iframe");
        iframe.width = "800";
        iframe.height = "450";
        iframe.src = videoUrl;
        iframe.frameBorder = "0";
        iframe.allow = "autoplay; encrypted-media";
        iframe.allowFullscreen = true;

        // Lägg till i body
        document.body.appendChild(iframe);
    </script>
</body>
</html>
