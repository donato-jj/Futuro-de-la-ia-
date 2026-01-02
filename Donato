
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>Debate: El Futuro de la IA</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- ====== ESTILOS ====== -->
  <style>
    :root {
      --bg: #0b0f1a;
      --card: rgba(255,255,255,0.08);
      --border: rgba(255,255,255,0.15);
      --text: #e8eaf0;
      --muted: #a0a4b8;
      --neon: #00e5ff;
      --violet: #8f5cff;
      --radius: 16px;
      --blur: blur(12px);
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "Segoe UI", system-ui, sans-serif;
    }

    body {
      background: radial-gradient(circle at top, #141a33, var(--bg));
      color: var(--text);
      min-height: 100vh;
    }

    header {
      position: sticky;
      top: 0;
      z-index: 10;
      backdrop-filter: var(--blur);
      background: rgba(10,15,30,0.75);
      border-bottom: 1px solid var(--border);
      padding: 16px 24px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    header h1 {
      font-size: 1.4rem;
      background: linear-gradient(90deg, var(--neon), var(--violet));
      -webkit-background-clip: text;
      color: transparent;
    }

    main {
      max-width: 900px;
      margin: 32px auto;
      padding: 0 16px;
      display: grid;
      gap: 24px;
    }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 20px;
      backdrop-filter: var(--blur);
    }

    /* ===== PERFIL ===== */
    .profile {
      display: flex;
      align-items: center;
      gap: 16px;
    }

    .profile img {
      width: 64px;
      height: 64px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid var(--neon);
    }

    .profile input {
      background: transparent;
      border: none;
      border-bottom: 1px solid var(--border);
      color: var(--text);
      padding: 6px;
      width: 100%;
    }

    /* ===== NUEVO POST ===== */
    textarea {
      width: 100%;
      background: transparent;
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px;
      color: var(--text);
      resize: none;
      min-height: 80px;
    }

    input[type="file"] {
      color: var(--muted);
    }

    button {
      background: linear-gradient(135deg, var(--neon), var(--violet));
      border: none;
      border-radius: 12px;
      padding: 10px 18px;
      color: #000;
      font-weight: 600;
      cursor: pointer;
      transition: transform .2s, opacity .2s;
    }

    button:hover {
      transform: scale(1.05);
      opacity: .9;
    }

    /* ===== FEED ===== */
    .post {
      display: grid;
      gap: 12px;
    }

    .post img.post-img {
      width: 100%;
      border-radius: 12px;
      border: 1px solid var(--border);
    }

    .post-actions {
      display: flex;
      gap: 16px;
      font-size: .9rem;
      color: var(--muted);
    }

    .post-actions span {
      cursor: pointer;
      user-select: none;
    }

    .comments {
      margin-top: 8px;
      display: grid;
      gap: 6px;
    }

    .comment {
      font-size: .85rem;
      color: var(--muted);
      border-left: 2px solid var(--neon);
      padding-left: 8px;
    }

    .comment-input {
      display: flex;
      gap: 8px;
      margin-top: 6px;
    }

    .comment-input input {
      flex: 1;
      background: transparent;
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 6px;
      color: var(--text);
    }

    @media (max-width:600px){
      header h1 { font-size: 1.1rem; }
    }
  </style>
</head>

<body>

<header>
  <h1>Debate: El Futuro de la IA</h1>
</header>

<main>

  <!-- PERFIL -->
  <div class="card profile">
    <img id="avatar" src="https://i.pravatar.cc/150?img=12">
    <div style="flex:1">
      <input id="username" placeholder="Tu nombre" />
      <input id="bio" placeholder="Bio corta sobre tu visión de la IA" />
    </div>
  </div>

  <!-- NUEVA PUBLICACIÓN -->
  <div class="card">
    <textarea id="postText" placeholder="¿Qué pensás sobre el futuro de la IA y sus miedos reales?"></textarea>
    <input type="file" id="postImage" accept="image/*">
    <br><br>
    <button onclick="createPost()">Publicar</button>
  </div>

  <!-- FEED -->
  <div id="feed"></div>

</main>

<!-- ====== JAVASCRIPT ====== -->
<script>
let posts = JSON.parse(localStorage.getItem("posts")) || [];

function savePosts() {
  localStorage.setItem("posts", JSON.stringify(posts));
}

function createPost() {
  const text = postText.value.trim();
  const imageFile = postImage.files[0];
  if (!text && !imageFile) return;

  const reader = new FileReader();
  reader.onload = () => {
    posts.unshift({
      id: Date.now(),
      user: username.value || "Usuario IA",
      bio: bio.value,
      avatar: avatar.src,
      text,
      image: reader.result || null,
      likes: 0,
      comments: []
    });
    savePosts();
    renderFeed();
    postText.value = "";
    postImage.value = "";
  };

  if (imageFile) reader.readAsDataURL(imageFile);
  else reader.onload();
}

function renderFeed() {
  feed.innerHTML = "";
  posts.forEach(post => {
    const div = document.createElement("div");
    div.className = "card post";

    div.innerHTML = `
      <strong>${post.user}</strong>
      <p>${post.text}</p>
      ${post.image ? `<img class="post-img" src="${post.image}">` : ""}
      <div class="post-actions">
        <span onclick="likePost(${post.id})">❤️ ${post.likes}</span>
        <span>💬 ${post.comments.length}</span>
      </div>
      <div class="comments">
        ${post.comments.map(c => `<div class="comment">${c}</div>`).join("")}
      </div>
      <div class="comment-input">
        <input placeholder="Comentar..." onkeydown="if(event.key==='Enter') addComment(${post.id}, this)">
      </div>
    `;
    feed.appendChild(div);
  });
}

function likePost(id) {
  const post = posts.find(p => p.id === id);
  post.likes++;
  savePosts();
  renderFeed();
}

function addComment(id, input) {
  if (!input.value.trim()) return;
  const post = posts.find(p => p.id === id);
  post.comments.push(input.value);
  input.value = "";
  savePosts();
  renderFeed();
}

renderFeed();
</script>

</body>
</html>
