# research-web
Personal scientific journal publishing website
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Research Web</title>

<!-- ===== STYLE ===== -->
<style>
body{
  font-family: Georgia, serif;
  background:#f4f6f8;
  margin:0;
}
header{
  background:#0a1f44;
  color:white;
  padding:20px;
}
nav a{
  color:white;
  margin-right:15px;
  text-decoration:none;
  font-weight:bold;
}
section{
  padding:20px;
}
.card{
  background:white;
  padding:15px;
  margin:15px 0;
  border-radius:6px;
}
input, textarea{
  width:100%;
  padding:10px;
  margin:8px 0;
}
button{
  background:#0a1f44;
  color:white;
  padding:10px 20px;
  border:none;
  cursor:pointer;
}
.hidden{display:none;}
footer{
  background:#ddd;
  padding:10px;
  text-align:center;
}
</style>
</head>

<body>

<header>
  <h1>Research Web</h1>
  <p>Scientific Journals by Md. Safin Al-Jubair</p>
  <nav>
    <a href="#" onclick="show('home')">Home</a>
    <a href="#" onclick="show('papers')">Publications</a>
    <a href="#" onclick="show('login')">Admin</a>
  </nav>
</header>

<!-- ===== HOME ===== -->
<section id="home">
  <div class="card">
    <h2>Research Fields</h2>
    <ul>
      <li>Astrophysics</li>
      <li>Genetical Engineering</li>
      <li>Quantum Mechanics</li>
    </ul>
    <p>This platform publishes original scientific research journals.</p>
  </div>
</section>

<!-- ===== PUBLICATIONS ===== -->
<section id="papers" class="hidden">
  <h2>Published Journals</h2>
  <div id="paperList"></div>
</section>

<!-- ===== LOGIN ===== -->
<section id="login" class="hidden">
  <div class="card">
    <h2>Admin Login</h2>
    <input id="email" type="email" placeholder="Email">
    <input id="password" type="password" placeholder="Password">
    <button onclick="login()">Login</button>
  </div>
</section>

<!-- ===== DASHBOARD ===== -->
<section id="dashboard" class="hidden">
  <div class="card">
    <h2>Publish New Journal</h2>
    <input id="title" placeholder="Paper Title">
    <textarea id="abstract" placeholder="Abstract"></textarea>
    <textarea id="content" placeholder="Full Paper"></textarea>
    <button onclick="publish()">Publish</button>
    <button onclick="logout()">Logout</button>
  </div>
</section>

<footer>© 2026 Md. Safin Al-Jubair</footer>

<!-- ===== FIREBASE ===== -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>

<script>
/* 🔥 REPLACE WITH YOUR FIREBASE CONFIG */
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID"
};

firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

/* ===== PAGE SWITCH ===== */
function show(id){
  ["home","papers","login","dashboard"].forEach(s=>{
    document.getElementById(s).classList.add("hidden");
  });
  document.getElementById(id).classList.remove("hidden");
}

/* ===== LOGIN ===== */
function login(){
  auth.signInWithEmailAndPassword(
    email.value,
    password.value
  ).then(()=>{
    show("dashboard");
  }).catch(e=>alert(e.message));
}

/* ===== LOGOUT ===== */
function logout(){
  auth.signOut().then(()=>show("home"));
}

/* ===== PUBLISH ===== */
function publish(){
  db.collection("papers").add({
    title:title.value,
    abstract:abstract.value,
    content:content.value,
    date:new Date()
  }).then(()=>{
    alert("Published");
    title.value=abstract.value=content.value="";
  });
}

/* ===== LOAD PAPERS ===== */
db.collection("papers").orderBy("date","desc")
.onSnapshot(snap=>{
  paperList.innerHTML="";
  snap.forEach(doc=>{
    let p=doc.data();
    paperList.innerHTML+=`
      <div class="card">
        <h3>${p.title}</h3>
        <p><b>Abstract:</b> ${p.abstract}</p>
        <p>${p.content}</p>
      </div>`;
  });
});
</script>

</body>
</html>
