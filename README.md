<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>YouTube Viewer</title>

<style>
  body {
    background:#000;
    color:#fff;
    font-family:Arial, sans-serif;
    padding:20px;
  }
  input, button {
    padding:8px;
    border-radius:4px;
    border:none;
    margin-right:5px;
  }
  input {
    width:350px;
    background:#111;
    color:#fff;
    border:1px solid #444;
  }
  button {
    background:#e62117;
    color:#fff;
    cursor:pointer;
  }
  iframe {
    width:100%;
    max-width:900px;
    aspect-ratio:16/9;
    border:0;
    margin-top:20px;
    display:block;
  }
</style>
</head>

<body>

<h1>YouTube Paste Viewer</h1>

<input id="ytInput" placeholder="Paste YouTube link or video ID">
<button id="loadBtn">Load Video</button>

<iframe id="player"
  src=""
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; fullscreen"
  allowfullscreen>
</iframe>

<script>
  function getVideoId(input) {
    input = input.trim();

    // Raw ID
    if (/^[A-Za-z0-9_-]{11}$/.test(input)) return input;

    // Try URL
    try {
      const url = new URL(input);

      if (url.searchParams.get("v")) return url.searchParams.get("v");

      if (url.hostname.includes("youtu.be"))
        return url.pathname.replace("/", "");

      const parts = url.pathname.split("/");
      const maybeId = parts.pop();
      if (/^[A-Za-z0-9_-]{11}$/.test(maybeId)) return maybeId;
    } catch {}

    return null;
  }

  document.getElementById("loadBtn").onclick = () => {
    const raw = document.getElementById("ytInput").value;
    const id = getVideoId(raw);

    if (!id) {
      alert("Invalid YouTube link or ID");
      return;
    }

    document.getElementById("player").src =
      "https://www.youtube-nocookie.com/embed/" + id;
  };
</script>

</body>
</html>
