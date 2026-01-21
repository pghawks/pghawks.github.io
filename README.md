<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mini YouTube Viewer</title>

<style>
    body {
        background:#000;
        color:#fff;
        font-family:Arial, sans-serif;
        margin:0;
        padding:20px;
        box-sizing:border-box;
    }
    .controls {
        display:flex;
        flex-wrap:wrap;
        gap:10px;
        margin-bottom:20px;
    }
    .controls input {
        padding:8px;
        border-radius:4px;
        border:1px solid #444;
        background:#111;
        color:#fff;
    }
    .controls button {
        padding:8px 12px;
        border-radius:4px;
        border:none;
        background:#e62117;
        color:#fff;
        cursor:pointer;
    }
    .controls button:hover {
        background:#c01b12;
    }
    iframe {
        width:100%;
        max-width:960px;
        aspect-ratio:16/9;
        border:0;
        display:block;
        margin:0 auto;
    }
</style>
</head>

<body>

<h1>Mini YouTube Viewer</h1>

<div class="controls">
    <input id="videoInput" type="text" placeholder="Paste YouTube URL or ID">
    <button id="loadBtn">Load Video</button>

    <input id="searchInput" type="text" placeholder="Search YouTube">
    <button id="searchBtn">Search</button>
</div>

<iframe id="player"
    src="https://www.youtube-nocookie.com/embed/gvxPSOTWcdo"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
</iframe>

<script>
const player = document.getElementById('player');
const videoInput = document.getElementById('videoInput');
const loadBtn = document.getElementById('loadBtn');
const searchInput = document.getElementById('searchInput');
const searchBtn = document.getElementById('searchBtn');

function extractVideoId(text) {
    try {
        if (text.includes('youtube.com') || text.includes('youtu.be')) {
            const url = new URL(text);
            if (url.searchParams.get('v')) {
                return url.searchParams.get('v');
            }
            const parts = url.pathname.split('/');
            return parts.pop() || parts.pop();
        }
    } catch (e) {}
    return text.trim();
}

loadBtn.addEventListener('click', () => {
    const raw = videoInput.value.trim();
    if (!raw) return;
    const id = extractVideoId(raw);
    player.src = 'https://www.youtube-nocookie.com/embed/' + encodeURIComponent(id);
});

searchBtn.addEventListener('click', () => {
    const q = searchInput.value.trim();
    if (!q) return;
    const url = 'https://www.youtube.com/results?search_query=' + encodeURIComponent(q);
    window.open(url, '_blank');
});
</script>

</body>
</html>
