# Futuro-de-la-ia-
Me gusta 
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>FuturaIA · Red Social del Futuro</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root {
  --bg: #0a0f1e;
  --card: #11162a;
  --neon: #00ffe1;
  --text: #e6e6e6;
}

* {
  box-sizing: border-box;
  font-family: 'Segoe UI', sans-serif;
}

body {
  margin: 0;
  background: radial-gradient(circle at top, #0f1b3d, var(--bg));
  color: var(--text);
}

.hidden { display: none; }

/* AUTH */
#auth {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.auth-box {
  background: var(--card);
  padding: 30px;
  width: 320px;
  border-radius: 12px;
  box-shadow: 0 0 30px rgba(0,255,225,.3);
}

.auth-box h1 {
  text-align: center;
}

.auth-box h1 span { color: var(--neon); }

.auth-box input,
.auth-box textarea,
.auth-box button {
  width: 100%;
  margin-top: 10px;
  padding: 10px;
  background: #0c1230;
  border: 1px solid #1f2a5a;
  color: white;
  border-radius: 8px;
}

.auth-box button {
  background: var(--neon);
  color: black;
  cursor: pointer;
  font-weight: bold;
}

/* NAV */
nav {
  display: flex;
  gap: 10px;
  padding: 15px;
  background: #0c1330;
  box-shadow: 0 0 15px rgba(0,255,225,.2);
}

nav h2 {
  margin-right: auto;
}

nav button {
  background: none;
  border: 1px solid var(--neon);
  color: var(--neon);
  padding: 6px 12px;
  border-radius: 20px;
  cursor: pointer;
}

/* MAIN */
main {
  padding: 20px;
}

/* POST BOX */
.post-box {
  background: var(--card);
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;
}

.post-box textarea,
.post-box input {
  width: 100%;
  margin-top: 10px;
  background: #0c1230;
  border: 1px solid #1f2a5a;
  color: white;
  padding: 10px;
  border-radius: 8px;
}

/* POST */
.post {
  background: #0d1535;
  padding: 15px;
  border-radius: 12px;
  margin-bottom: 15px;
  box-shadow: inset 0 0 10px rgba(0,255,225,.1);
  animation: fadeIn .4s ease;
}

.post small {
  opacity: .7;
}

.actions button {
  background: none;
  border: none;
  color: var(--neon);
  cursor: pointer;
  margin-top: 10px;
  font-size: 16px;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
</head>

<body>

<!-- LOGIN -->
<section id="auth">
  <div class="auth-box">
    <h1>Futura<span>IA</span></h1>
    <input id="username" placeholder="Usuario / Alias">
    <textarea id="bio" placeholder="Tu visión sobre la IA..."></textarea>
    <button onclick="login()">Ingresar</button>
  </div>
</section>

<!-- APP -->
<section id="app" class="hidden">

<nav>
  <h2>🧠 FuturaIA</h2>
  <button onclick="showSection('feed')">Feed</button>
  <button onclick="showSection('debates')">Debates</button>
  <button onclick="showSection('profile')">Perfil</button>
  <button onclick="logout()">Salir</button>
</nav>

<!-- FEED -->
<main id="feed">
  <div class="post-box">
    <textarea id="postText" placeholder="¿Qué pensás sobre el futuro de la IA?"></textarea>
    <button onclick="createPost()">Publicar</button>
  </div>
  <div id="posts"></div>
</main>

<!-- DEBATES -->
<main id="debates" class="hidden">
  <h3>Debates Abiertos</h3>
  <p>¿La IA reemplazará al humano o lo potenciará?</p>
  <p>¿Qué miedos reales genera la automatización?</p>
</main>

<!-- PROFILE -->
<main id="profile" class="hidden">
  <h3>Mi Perfil</h3>
  <p><strong>Usuario:</strong> <span id="profileName"></span></p>
  <p><strong>Bio:</strong></p>
  <p id="profileBio"></p>
</main>

</section>

<script>
let currentUser = null;

function login() {
  const name = username.value.trim();
  if (!name) return alert("Ingresá un usuario");

  currentUser = { name, bio: bio.value };
  localStorage.setItem("user", JSON.stringify(currentUser));

  auth.classList.add("hidden");
  app.classList.remove("hidden");
  loadProfile();
  loadPosts();
}

function logout() {
  localStorage.clear();
  location.reload();
}

function showSection(id) {
  document.querySelectorAll("main").forEach(m => m.classList.add("hidden"));
  document.getElementById(id).classList.remove("hidden");
}

function loadProfile() {
  profileName.textContent = currentUser.name;
  profileBio.textContent = currentUser.bio;
}

function createPost() {
  const text = postText.value.trim();
  if (!text) return;

  const posts = JSON.parse(localStorage.getItem("posts") || "[]");

  posts.unshift({
    user: currentUser.name,
    text,
    likes: 0,
    time: new Date().toLocaleString()
  });

  localStorage.setItem("posts", JSON.stringify(posts));
  postText.value = "";
  loadPosts();
}

function loadPosts() {
  const posts = JSON.parse(localStorage.getItem("posts") || "[]");
  const postsDiv = document.getElementById("posts");
  postsDiv.innerHTML = "";

  posts.forEach((p, i) => {
    const div = document.createElement("div");
    div.className = "post";
    div.innerHTML = `
      <strong>${p.user}</strong> · <small>${p.time}</small>
      <p>${p.text}</p>
      <div class="actions">
        <button onclick="likePost(${i})">❤️ ${p.likes}</button>
      </div>
    `;
    postsDiv.appendChild(div);
  });
}

function likePost(i) {
  const posts = JSON.parse(localStorage.getItem("posts"));
  posts[i].likes++;
  localStorage.setItem("posts", JSON.stringify(posts));
  loadPosts();
}

/* AUTO LOGIN */
const savedUser = localStorage.getItem("user");
if (savedUser) {
  currentUser = JSON.parse(savedUser);
  auth.classList.add("hidden");
  app.classList.remove("hidden");
  loadProfile();
  loadPosts();
}
</script>

</body>
</html>
