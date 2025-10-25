# SSTV
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>STV - Smart Learning TV</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f0f2f5;}
header{background:#1e90ff;color:white;padding:15px;text-align:center;}
header h1{margin:0;font-size:24px;}
nav{background:#0074cc;display:flex;justify-content:center;flex-wrap:wrap;}
nav button{margin:5px;padding:10px 20px;border:none;border-radius:5px;background:white;color:#0074cc;font-weight:bold;cursor:pointer;transition:0.3s;}
nav button:hover{background:#005fa3;color:white;}
.container{padding:20px;}
.video-grid{display:flex;flex-wrap:wrap;justify-content:center;}
.video-card{background:white;margin:10px;padding:10px;width:320px;border-radius:10px;box-shadow:0 0 8px rgba(0,0,0,0.1);}
.video-card iframe{width:100%;height:180px;border-radius:8px;}
.video-card h4{text-align:center;margin-top:5px;font-size:16px;}
.admin-panel{background:white;max-width:500px;margin:auto;padding:20px;border-radius:10px;box-shadow:0 0 10px rgba(0,0,0,0.1);}
input,button{width:100%;padding:10px;margin:8px 0;border-radius:5px;border:1px solid #ccc;}
button{background:#0074cc;color:white;border:none;}
button:hover{background:#005fa3;}
.hidden{display:none;}
</style>
</head>
<body>

<header>
  <h1>📺 STV - Smart Learning TV</h1>
</header>

<nav>
  <button onclick="showHome()">Home</button>
  <button onclick="showLogin()">Login</button>
  <button onclick="showSignup()">Sign Up</button>
  <button onclick="showAdmin()" id="adminBtn" style="display:none;">Admin Panel</button>
  <button onclick="logout()" id="logoutBtn" style="display:none;">Logout</button>
</nav>

<div class="container">

  <!-- Home Page -->
  <div id="homePage">
    <h2>Educational Videos</h2>
    <div id="videoList" class="video-grid"></div>
  </div>

  <!-- Login Page -->
  <div id="loginPage" class="hidden">
    <div class="admin-panel">
      <h3>Login</h3>
      <input type="text" id="loginEmail" placeholder="Email">
      <input type="password" id="loginPassword" placeholder="Password">
      <button onclick="login()">Login</button>
    </div>
  </div>

  <!-- Signup Page -->
  <div id="signupPage" class="hidden">
    <div class="admin-panel">
      <h3>Sign Up</h3>
      <input type="text" id="signupEmail" placeholder="Email">
      <input type="password" id="signupPassword" placeholder="Password">
      <button onclick="signup()">Sign Up</button>
    </div>
  </div>

  <!-- Admin Panel -->
  <div id="adminPage" class="hidden">
    <div class="admin-panel">
      <h3>Admin Panel</h3>
      <input type="text" id="videoTitle" placeholder="Video Title">
      <input type="text" id="videoURL" placeholder="YouTube Embed URL">
      <button onclick="addVideo()">Add Video</button>
      <h4>Existing Videos</h4>
      <div id="adminVideoList"></div>
    </div>
  </div>

</div>

<script>
// 20 Educational YouTube Embed Videos
let videos=[
  {title:"HTML Crash Course", url:"https://www.youtube.com/embed/qz0aGYrrlhU"},
  {title:"CSS Tutorial", url:"https://www.youtube.com/embed/yfoY53QXEnI"},
  {title:"JavaScript Basics", url:"https://www.youtube.com/embed/W6NZfCO5SIk"},
  {title:"Python for Beginners", url:"https://www.youtube.com/embed/_uQrJ0TkZlc"},
  {title:"Learn React JS", url:"https://www.youtube.com/embed/Ke90Tje7VS0"},
  {title:"Learn Node.js", url:"https://www.youtube.com/embed/TlB_eWDSMt4"},
  {title:"SQL Basics", url:"https://www.youtube.com/embed/7S_tz1z_5bA"},
  {title:"Linux Command Line", url:"https://www.youtube.com/embed/IVquJh3DXUA"},
  {title:"Git & GitHub Tutorial", url:"https://www.youtube.com/embed/RGOj5yH7evk"},
  {title:"Data Structures in Python", url:"https://www.youtube.com/embed/sVxBVvlnJsM"},
  {title:"Algorithms for Beginners", url:"https://www.youtube.com/embed/8hly31xKli0"},
  {title:"Machine Learning Basics", url:"https://www.youtube.com/embed/GwIo3gDZCVQ"},
  {title:"Artificial Intelligence Intro", url:"https://www.youtube.com/embed/2ePf9rue1Ao"},
  {title:"Blockchain Explained", url:"https://www.youtube.com/embed/SSo_EIwHSd4"},
  {title:"Cyber Security Basics", url:"https://www.youtube.com/embed/inWWhr5tnEA"},
  {title:"Networking Fundamentals", url:"https://www.youtube.com/embed/qiQR5rTSshw"},
  {title:"Docker for Beginners", url:"https://www.youtube.com/embed/fqMOX6JJhGo"},
  {title:"Kubernetes Crash Course", url:"https://www.youtube.com/embed/X48VuDVv0do"},
  {title:"Cloud Computing Basics", url:"https://www.youtube.com/embed/2LaAJq1lB1Q"},
  {title:"Digital Marketing Basics", url:"https://www.youtube.com/embed/6gGbR8R6nFM"}
];

if(localStorage.getItem('videos')) videos=JSON.parse(localStorage.getItem('videos'));
let users=JSON.parse(localStorage.getItem('users'))||[];
let currentUser=JSON.parse(localStorage.getItem('currentUser'))||null;

function showHome(){hideAll();document.getElementById('homePage').classList.remove('hidden');renderVideos();}
function showLogin(){hideAll();document.getElementById('loginPage').classList.remove('hidden');}
function showSignup(){hideAll();document.getElementById('signupPage').classList.remove('hidden');}
function showAdmin(){hideAll();document.getElementById('adminPage').classList.remove('hidden');renderAdminVideos();}
function hideAll(){document.getElementById('homePage').classList.add('hidden');document.getElementById('loginPage').classList.add('hidden');document.getElementById('signupPage').classList.add('hidden');document.getElementById('adminPage').classList.add('hidden');}

function renderVideos(){
  const list=document.getElementById('videoList');
  list.innerHTML='';
  videos.forEach(v=>{
    list.innerHTML+=`
      <div class="video-card">
        <iframe src="${v.url}" frameborder="0" allowfullscreen></iframe>
        <h4>${v.title}</h4>
      </div>`;
  });
}

function renderAdminVideos(){
  const list=document.getElementById('adminVideoList');
  list.innerHTML='';
  videos.forEach((v,i)=>{
    list.innerHTML+=`
      <div class="video-card">
        <iframe src="${v.url}" frameborder="0" allowfullscreen></iframe>
        <h4>${v.title}</h4>
        <button onclick="editVideo(${i})">Edit</button>
        <button onclick="deleteVideo(${i})">Delete</button>
      </div>`;
  });
}

function addVideo(){
  const title=document.getElementById('videoTitle').value;
  const url=document.getElementById('videoURL').value;
  if(title && url){
    videos.push({title,url});
    localStorage.setItem('videos',JSON.stringify(videos));
    renderAdminVideos();renderVideos();
    document.getElementById('videoTitle').value='';document.getElementById('videoURL').value='';
  }
}

function editVideo(i){
  const newTitle=prompt("Enter new title",videos[i].title);
  const newURL=prompt("Enter YouTube Embed URL",videos[i].url);
  if(newTitle && newURL){videos[i]={title:newTitle,url:newURL};localStorage.setItem('videos',JSON.stringify(videos));renderAdminVideos();renderVideos();}
}

function deleteVideo(i){if(confirm("Are you sure?")){videos.splice(i,1);localStorage.setItem('videos',JSON.stringify(videos));renderAdminVideos();renderVideos();}}

function signup(){
  const email=document.getElementById('signupEmail').value;
  const pass=document.getElementById('signupPassword').value;
  if(users.find(u=>u.email===email))return alert("User already exists");
  users.push({email,password:pass});
  localStorage.setItem('users',JSON.stringify(users));
  alert("Signup successful!");showLogin();
}

function login(){
  const email=document.getElementById('loginEmail').value;
  const pass=document.getElementById('loginPassword').value;
  const user=users.find(u=>u.email===email && u.password===pass);
  if(user){
    currentUser=user;localStorage.setItem('currentUser',JSON.stringify(user));
    document.getElementById('adminBtn').style.display='inline';
    document.getElementById('logoutBtn').style.display='inline';
    alert("Login successful!");showHome();
  } else alert("Invalid credentials");
}

function logout(){currentUser=null;localStorage.removeItem('currentUser');document.getElementById('adminBtn').style.display='none';document.getElementById('logoutBtn').style.display='none';showHome();}

// Init
showHome();
if(currentUser){document.getElementById('adminBtn').style.display='inline';document.getElementById('logoutBtn').style.display='inline';}
</script>
