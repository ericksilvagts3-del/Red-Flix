<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RedFlix — Filmes & Séries</title>
<style>
:root{--red:#e50914;--dark:#080808;--card:#151515;--muted:#aaa;--white:#fff}
*{box-sizing:border-box} body{margin:0;background:var(--dark);color:var(--white);font-family:Arial,Helvetica,sans-serif}
header{position:sticky;top:0;z-index:10;background:rgba(8,8,8,.96);border-bottom:1px solid #252525;padding:14px 5%;display:flex;align-items:center;gap:20px}
.logo{font-size:25px;font-weight:900;color:var(--red);letter-spacing:-1px}.logo span{color:#fff}
nav{display:flex;gap:10px;flex-wrap:wrap}button,.btn{border:0;border-radius:8px;padding:10px 15px;background:#242424;color:#fff;cursor:pointer;font-weight:700}
button:hover,.btn:hover{background:#333}.primary{background:var(--red)}.primary:hover{background:#b80710}
.search{margin-left:auto;background:#171717;border:1px solid #333;color:#fff;border-radius:8px;padding:10px 12px;width:min(280px,42vw)}
main{padding:30px 5% 60px}.hero{background:linear-gradient(100deg,#220306,#0c0c0c);border:1px solid #3a1012;border-radius:18px;padding:35px;margin-bottom:30px}
.hero h1{font-size:clamp(30px,5vw,54px);margin:0 0 10px}.hero p{color:#ddd;max-width:650px;line-height:1.6}
h2{margin-top:30px}.categories{display:flex;gap:10px;overflow:auto;padding-bottom:5px}.categories button.active{background:var(--red)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:18px;margin-top:18px}
.card{background:var(--card);border:1px solid #272727;border-radius:12px;overflow:hidden;transition:.2s}.card:hover{transform:translateY(-3px);border-color:#555}
.poster{height:245px;background:linear-gradient(145deg,#3a070a,#171717);display:flex;align-items:center;justify-content:center;font-size:55px}
.info{padding:14px}.info h3{margin:0 0 7px}.meta{color:var(--muted);font-size:13px}.price{color:#ff4b52;font-weight:800;margin:10px 0}.locked{font-size:12px;color:#ddd}
.creator{background:#111;border:1px solid #2d2d2d;border-radius:14px;padding:20px;margin-top:40px}.creator form{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}.creator input,.creator select,.creator textarea{width:100%;background:#181818;border:1px solid #333;color:#fff;border-radius:8px;padding:11px}.creator textarea{min-height:90px}.full{grid-column:1/-1}
.modal{position:fixed;inset:0;background:rgba(0,0,0,.78);display:none;align-items:center;justify-content:center;padding:20px;z-index:20}.modal.open{display:flex}.box{background:#151515;border:1px solid #3a3a3a;border-radius:16px;padding:24px;width:min(520px,100%);position:relative}.close{position:absolute;right:14px;top:12px;background:transparent;font-size:20px}.pix{background:#090909;border:1px dashed var(--red);padding:16px;border-radius:10px;margin:15px 0;word-break:break-all}.note{color:#bbb;font-size:13px;line-height:1.5}
footer{text-align:center;color:#777;border-top:1px solid #222;padding:25px}
@media(max-width:650px){header{flex-wrap:wrap}.search{order:3;width:100%;margin-left:0}.creator form{grid-template-columns:1fr}.full{grid-column:auto}.poster{height:220px}}
</style>
</head>
<body>
<header>
  <div class="logo">RED<span>FLIX</span></div>
  <nav>
    <button onclick="filter('Todos')">Início</button>
    <button onclick="filter('Filmes')">Filmes</button>
    <button onclick="filter('Séries')">Séries</button>
  </nav>
  <input id="search" class="search" placeholder="🔎 Buscar título..." oninput="render()">
</header>

<main>
<section class="hero">
  <h1>Seu catálogo de filmes e séries</h1>
  <p>Uma interface em vermelho e preto para publicar e organizar seu catálogo. O acesso ao conteúdo pode ser liberado após confirmação do pagamento.</p>
  <button class="primary" onclick="document.getElementById('catalogo').scrollIntoView({behavior:'smooth'})">Explorar catálogo</button>
</section>

<h2 id="catalogo">Categorias</h2>
<div class="categories">
  <button class="active" onclick="filter('Todos',this)">Todos</button>
  <button onclick="filter('Anime',this)">Anime</button>
  <button onclick="filter('Terror',this)">Terror</button>
  <button onclick="filter('Romance',this)">Romance</button>
  <button onclick="filter('Aventura',this)">Aventura</button>
</div>

<div id="grid" class="grid"></div>

<section class="creator">
  <h2>🎬 Área do criador</h2>
  <p class="note">Este formulário salva itens apenas neste navegador. Para um aplicativo real, conecte-o a um servidor/banco de dados e publique somente filmes e séries para os quais você tenha autorização de distribuição.</p>
  <form id="form">
    <input id="title" placeholder="Nome do filme/série" required>
    <select id="type"><option>Filmes</option><option>Séries</option></select>
    <select id="genre"><option>Anime</option><option>Terror</option><option>Romance</option><option>Aventura</option></select>
    <input id="price" type="number" min="0" step="0.01" placeholder="Preço (R$)" required>
    <input id="year" type="number" placeholder="Ano">
    <input id="emoji" placeholder="Emoji da capa (ex.: 🎬)">
    <textarea id="description" class="full" placeholder="Descrição"></textarea>
    <button class="primary full" type="submit">＋ Publicar no catálogo</button>
  </form>
</section>
</main>

<div id="modal" class="modal">
 <div class="box">
   <button class="close" onclick="closeModal()">×</button>
   <h2 id="modalTitle">Liberar conteúdo</h2>
   <p id="modalDesc"></p>
   <strong>Pagamento via Pix</strong>
   <div class="pix">ericksilvagts3@gmail.com</div>
   <p class="note">Depois de realizar o Pix, o criador precisa confirmar o pagamento antes de liberar o acesso. Esta versão é uma demonstração: ela não confirma pagamentos automaticamente.</p>
   <button class="primary" onclick="alert('Pagamento enviado para conferência. Nesta versão, a liberação é manual.')">Já fiz o Pix</button>
 </div>
</div>

<footer>RED<span style="color:#fff">FLIX</span> • Protótipo HTML • Use somente conteúdo legalmente autorizado.</footer>

<script>
const defaultItems=[
 {title:"Aventura Épica",type:"Filmes",genre:"Aventura",price:3.00,year:2026,emoji:"🏔️",description:"Uma aventura cheia de desafios."},
 {title:"Noite Sombria",type:"Filmes",genre:"Terror",price:2.50,year:2026,emoji:"👻",description:"Uma história de terror."},
 {title:"Corações",type:"Séries",genre:"Romance",price:2.00,year:2026,emoji:"❤️",description:"Uma série romântica."},
 {title:"Anime Galaxy",type:"Séries",genre:"Anime",price:3.50,year:2026,emoji:"⚡",description:"Uma jornada em um universo de anime."}
];
let items=JSON.parse(localStorage.getItem("redflix_items")||"null")||defaultItems;
let current="Todos";

function filter(value,btn){
 current=value;
 document.querySelectorAll(".categories button").forEach(b=>b.classList.remove("active"));
 if(btn) btn.classList.add("active");
 render();
}
function render(){
 const q=document.getElementById("search").value.toLowerCase();
 const grid=document.getElementById("grid");
 let list=items.filter(x=>(current==="Todos"||x.genre===current||x.type===current)&&x.title.toLowerCase().includes(q));
 grid.innerHTML=list.map((x,i)=>`
 <article class="card">
  <div class="poster">${x.emoji||"🎬"}</div>
  <div class="info">
   <h3>${escapeHtml(x.title)}</h3>
   <div class="meta">${escapeHtml(x.type)} • ${escapeHtml(x.genre)}${x.year?" • "+x.year:""}</div>
   <div class="price">R$ ${Number(x.price).toFixed(2)}</div>
   <div class="locked">🔒 Conteúdo pago</div>
   <button class="primary" style="margin-top:10px;width:100%" onclick="openModal(${items.indexOf(x)})">Assistir</button>
  </div>
 </article>`).join("") || "<p>Nenhum título encontrado.</p>";
}
function openModal(i){
 const x=items[i];
 document.getElementById("modalTitle").textContent="Assistir: "+x.title;
 document.getElementById("modalDesc").textContent=`Valor para liberar: R$ ${Number(x.price).toFixed(2)}`;
 document.getElementById("modal").classList.add("open");
}
function closeModal(){document.getElementById("modal").classList.remove("open")}
function escapeHtml(s){return String(s??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"}[c]))}
document.getElementById("form").addEventListener("submit",e=>{
 e.preventDefault();
 const x={
  title:title.value,type:type.value,genre:genre.value,
  price:Number(price.value),year:year.value,emoji:emoji.value||"🎬",
  description:description.value
 };
 items.unshift(x); localStorage.setItem("redflix_items",JSON.stringify(items));
 e.target.reset(); render(); alert("Título publicado no catálogo deste navegador.");
});
render();
</script>
</body>
</html>

