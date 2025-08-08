<!doctype html>

<html lang="uz">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ForgeLandsUz — Serverlar & Yangiliklar</title>
  <meta name="description" content="mcskill.net kabi soddalashtirilgan demo — serverlar ro'yxati, yangiliklar, qidiruv va oddiy admin panel (localStorage)." />
  <link rel="icon" href="data:," />
  <style>
    /* Oddiy, toza CSS — responsive */
    :root{--bg:#0b1020;--card:#0f1724;--muted:#9aa4b2;--accent:#6ee7b7}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,Arial,Helvetica,sans-serif;background:linear-gradient(180deg,#061028 0%,var(--bg) 60%);color:#e6eef8}
    header{display:flex;align-items:center;justify-content:space-between;padding:18px 24px;background:rgba(255,255,255,0.02);backdrop-filter: blur(4px)}
    .brand{display:flex;gap:12px;align-items:center}
    .logo{width:44px;height:44px;background:linear-gradient(135deg,#7de3b0,#4cc9ff);border-radius:10px;display:flex;align-items:center;justify-content:center;color:#012; font-weight:700}
    nav{display:flex;gap:12px}
    nav a{color:var(--muted);text-decoration:none;padding:8px 10px;border-radius:8px}
    nav a:hover{color:#fff;background:rgba(255,255,255,0.03)}
    .container{max-width:1100px;margin:28px auto;padding:0 18px}
    .hero{display:flex;gap:20px;align-items:center}
    .hero-left{flex:1}
    .hero h1{margin:0;font-size:28px}
    .hero p{color:var(--muted);margin-top:8px}
    .actions{margin-top:14px;display:flex;gap:10px}
    .btn{padding:10px 14px;border-radius:10px;border:0;cursor:pointer}
    .btn-primary{background:linear-gradient(90deg,#10b981,#06b6d4);color:#012}
    .btn-ghost{background:transparent;color:var(--muted);border:1px solid rgba(255,255,255,0.03)}/* layout grid */
.grid{display:grid;grid-template-columns:1fr 360px;gap:20px;margin-top:22px}
.card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:14px;border-radius:12px}

/* server list */
.server-card{display:flex;gap:12px;align-items:center;padding:10px;border-radius:10px;background:linear-gradient(180deg, rgba(0,0,0,0.12), transparent);margin-bottom:10px}
.server-thumb{width:72px;height:72px;background:#081223;border-radius:8px;display:flex;align-items:center;justify-content:center;font-weight:700}
.server-info h3{margin:0;font-size:16px}
.server-info p{margin:6px 0 0;color:var(--muted);font-size:13px}
.status{margin-left:auto;padding:6px 10px;border-radius:999px;font-weight:600}
.online{background:#052e20;color:#9ff3d7}
.offline{background:#2b1020;color:#ffb4c4}

/* news */
.news-item{margin-bottom:12px}
.news-item h4{margin:0}
.muted{color:var(--muted);font-size:13px}

/* search */
.search{display:flex;gap:8px}
.search input{flex:1;padding:8px;border-radius:10px;border:0;background:rgba(255,255,255,0.02);color:#e6eef8}

footer{margin:40px 0;color:var(--muted);text-align:center}

/* responsive */
@media (max-width:900px){.grid{grid-template-columns:1fr}.hero{flex-direction:column;align-items:flex-start}.server-thumb{width:56px;height:56px}}

  </style>
</head>
<body>
  <header>
    <div class="brand">
      <div class="logo">FL</div>
      <div>
        <div style="font-weight:700">ForgeLandsUz</div>
        <div style="font-size:12px;color:var(--muted)">Minecraft serverlar & yangiliklar</div>
      </div>
    </div>
    <nav>
      <a href="#servers">Serverlar</a>
      <a href="#news">Yangiliklar</a>
      <a href="#contact">Bog'lanish</a>
      <button class="btn btn-ghost" id="adminBtn">Admin</button>
    </nav>
  </header>  <main class="container">
    <section class="hero">
      <div class="hero-left">
        <h1>MCSkill uslubida — server ro'yxati va yangiliklar sahifasi</h1>
        <p>Bu demo sahifa — mcskill.net kabi ko'rinish va foydalanish uchun tayyorlangan bitta HTML fayl. Serverlar qo'shish, qidiruv va oddiy admin panel localStorage yordamida ishlaydi.</p>
        <div class="actions">
          <button class="btn btn-primary" onclick="document.getElementById('servers').scrollIntoView({behavior:'smooth'})">Serverlarni ko'rish</button>
          <button class="btn btn-ghost" onclick="document.getElementById('news').scrollIntoView({behavior:'smooth'})">Oxirgi yangiliklar</button>
        </div>
      </div>
      <div style="width:360px">
        <div class="card">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <div>
              <div style="font-size:13px;color:var(--muted)">Onlayn o'yinchilar</div>
              <div style="font-weight:700;font-size:22px" id="onlineCount">0</div>
            </div>
            <div style="text-align:right">
              <div class="muted">Serverlar</div>
              <div style="font-weight:700" id="serverCount">0</div>
            </div>
          </div>
          <hr style="margin:12px 0;border:0;border-top:1px dashed rgba(255,255,255,0.03)" />
          <div class="muted">Qidiruv</div>
          <div class="search" style="margin-top:8px">
            <input id="searchInput" placeholder="Server nomi yoki IP qidirish" oninput="renderServers()" />
            <button class="btn btn-primary" onclick="addDemoServer()">+ Demo</button>
          </div>
        </div>
        <div style="height:12px"></div>
        <div class="card">
          <div class="muted">Tez linklar</div>
          <ul style="padding-left:18px;margin:8px 0 0;color:var(--muted);font-size:14px">
            <li><a href="#" style="color:var(--muted)">Top serverlar</a></li>
            <li><a href="#" style="color:var(--muted)">Yangiliklar</a></li>
            <li><a href="#" style="color:var(--muted)">Qo'llab-quvvatlash</a></li>
          </ul>
        </div>
      </div>
    </section><div class="grid">
  <section id="servers">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px">
      <h2 style="margin:0">Serverlar</h2>
      <div class="muted">Filter / tartiblash</div>
    </div>

    <div id="serverList" class="card">
      <!-- Server cards injected here -->
    </div>
  </section>

  <aside>
    <div class="card" id="news">
      <h3 style="margin-top:0">So'nggi yangiliklar</h3>
      <div id="newsList">
        <!-- news injected -->
      </div>
      <button class="btn btn-ghost" style="margin-top:8px" onclick="addDemoNews()">Yangilik qo'shish</button>
    </div>

    <div style="height:14px"></div>
    <div class="card">
      <h4 style="margin:0">Kontakt</h4>
      <p class="muted">support@forgelands.uz</p>
    </div>
  </aside>
</div>

<footer>
  <div>© ForgeLandsUz — Demo. Saytni o'zingiz xohlagancha o'zgartiring.</div>
</footer>

  </main>  <!-- Admin modal -->  <div id="adminModal" style="position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(2,6,23,0.6)">
    <div style="width:720px;max-width:95%;background:var(--card);padding:18px;border-radius:12px">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <h3 style="margin:0">Admin panel (local demo)</h3>
        <button onclick="closeAdmin()" class="btn btn-ghost">Yopish</button>
      </div>
      <div style="display:flex;gap:12px">
        <div style="flex:1">
          <label class="muted">Server nomi</label>
          <input id="srv_name" style="width:100%;padding:8px;border-radius:8px;border:0;background:rgba(255,255,255,0.02);color:#e6eef8" />
          <label class="muted">IP:Port</label>
          <input id="srv_ip" style="width:100%;padding:8px;border-radius:8px;border:0;background:rgba(255,255,255,0.02);color:#e6eef8" />
          <div style="display:flex;gap:8px;margin-top:8px">
            <button class="btn btn-primary" onclick="saveServer()">Saqlash</button>
            <button class="btn btn-ghost" onclick="clearServers()">Barcha serverlarni tozalash</button>
          </div>
        </div>
        <div style="width:260px">
          <div class="muted">Quick tips</div>
          <ul style="padding-left:18px;color:var(--muted)">
            <li>Demo server qo'shish uchun "+ Demo" tugmasi.</li>
            <li>Serverlar brauzer localStorage'da saqlanadi.</li>
          </ul>
        </div>
      </div>
    </div>
  </div>  <script>
    // Demo data + storage helpers
    function load(){
      const s = localStorage.getItem('fl_servers');
      if(s) return JSON.parse(s);
      const demo = [
        {id:1,name:'ForgeLandsUz Anarxiya',ip:'play.forgelands.uz:25565',online:12,desc:'PvP, Grief ruxsat',rank:5},
        {id:2,name:'SkyBlock Paradise',ip:'sky.forgelands.uz:25565',online:3,desc:'Skyblock server',rank:4}
      ];
      localStorage.setItem('fl_servers',JSON.stringify(demo));
      return demo;
    }
    function saveAll(arr){localStorage.setItem('fl_servers',JSON.stringify(arr));}
    function getServers(){return load();}

    // Render functions
    function renderServers(){
      const q=document.getElementById('searchInput').value.toLowerCase();
      const list=getServers();
      const container=document.getElementById('serverList');
      container.innerHTML='';
      let onlineSum=0;
      list.filter(s=>s.name.toLowerCase().includes(q)||s.ip.toLowerCase().includes(q)).forEach(s=>{
        onlineSum+=s.online||0;
        const el=document.createElement('div'); el.className='server-card';
        el.innerHTML=`
          <div class="server-thumb">${s.name.split(' ').map(x=>x[0]).slice(0,2).join('')}</div>
          <div class="server-info">
            <h3>${s.name}</h3>
            <p class="muted">${s.desc} · ${s.ip}</p>
          </div>
          <div class="status ${s.online? 'online':'offline'}">${s.online? s.online+' online':'offline'}</div>
        `;
        container.appendChild(el);
      });
      document.getElementById('onlineCount').innerText=onlineSum;
      document.getElementById('serverCount').innerText=list.length;
      if(list.length===0) container.innerHTML='<div class="muted">Serverlar topilmadi — admin panel yordamida qo'shing.</div>';
    }

    // News demo
    function getNews(){
      const s=localStorage.getItem('fl_news');
      if(s) return JSON.parse(s);
      const demo=[{id:1,title:'Server ochildi — ForgeLandsUz',date:'2025-08-08',excerpt:'Bugun yangi Anarxiya serveri ochildi.'}];
      localStorage.setItem('fl_news',JSON.stringify(demo));
      return demo;
    }
    function renderNews(){
      const list=getNews();
      const el=document.getElementById('newsList'); el.innerHTML='';
      list.forEach(n=>{const d=document.createElement('div');d.className='news-item';d.innerHTML=`<h4>${n.title}</h4><div class="muted">${n.date}</div><p class="muted">${n.excerpt}</p>`;el.appendChild(d)});
    }

    // Admin
    document.getElementById('adminBtn').addEventListener('click',()=>{document.getElementById('adminModal').style.display='flex'});
    function closeAdmin(){document.getElementById('adminModal').style.display='none'}
    function saveServer(){
      const name=document.getElementById('srv_name').value.trim();
      const ip=document.getElementById('srv_ip').value.trim();
      if(!name||!ip){alert('Iltimos barcha maydonlarni to' + "'" + 'ldiring');return}
      const arr=getServers();
      arr.push({id:Date.now(),name,ip,online:Math.floor(Math.random()*20),desc:'Qo' + "'" + 'shimcha ma'lumot',rank:3});
      saveAll(arr); renderServers(); closeAdmin();
    }
    function clearServers(){if(confirm('Barchasini o'chirmoqchimisiz?')){localStorage.removeItem('fl_servers');renderServers();}}

    // Demo add
    function addDemoServer(){
      const arr=getServers();
      arr.push({id:Date.now(),name:'Demo Server '+(arr.length+1),ip:'demo'+(arr.length+1)+'.local:25565',online:Math.floor(Math.random()*30),desc:'Demo server'});
      saveAll(arr); renderServers();
    }
    function addDemoNews(){
      const n=getNews(); n.push({id:Date.now(),title:'Yangi yangilik '+(n.length+1),date:new Date().toISOString().slice(0,10),excerpt:'Bu demo yangilik.'}); localStorage.setItem('fl_news',JSON.stringify(n)); renderNews();
    }

    // Init
    renderServers(); renderNews();
  </script></body>
</html>
