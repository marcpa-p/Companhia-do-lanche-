<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Companhia do Lanche - Umuarama</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif}
body{background:#111;color:#fff}
header{background:linear-gradient(135deg,#b30000,#ff0000);padding:25px;text-align:center}
.container{max-width:1200px;margin:auto;padding:20px}

/* ABAS */
.tabs{display:flex;gap:10px;margin-bottom:20px;flex-wrap:wrap}
.tab-btn{
flex:1;
padding:10px;
background:#1c1c1c;
border:none;
color:#fff;
cursor:pointer;
border-radius:8px;
}
.tab-btn.active{background:#ff0000}
.tab-content{display:none}
.tab-content.active{display:block}

.menu-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:15px}
.card{background:#1c1c1c;padding:15px;border-radius:10px}
.price{color:#ff0000;font-weight:600;margin:8px 0}
button{background:#ff0000;border:none;padding:8px;width:100%;color:#fff;border-radius:6px;cursor:pointer}
button:hover{background:#b30000}

/* CARRINHO */
.cart{
position:fixed;
right:20px;
bottom:20px;
background:#000;
padding:15px;
border-radius:10px;
width:320px;
max-height:85vh;
overflow:auto;
box-shadow:0 0 20px rgba(0,0,0,.6)
}
.cart-item{display:flex;justify-content:space-between;font-size:13px;margin-bottom:5px}
.total{margin-top:10px;font-weight:bold;color:#ff0000}
input,select{width:100%;padding:6px;margin-top:6px;border-radius:5px;border:none}
footer{text-align:center;padding:30px;background:#000;margin-top:60px}
</style>
</head>

<body>

<header>
<h1>🍔 Companhia do Lanche</h1>
<p>Delivery e Balcão • Umuarama - PR</p>
</header>

<div class="container">

<div class="tabs">
<button class="tab-btn active" onclick="showTab('prensados',this)">Prensados</button>
<button class="tab-btn" onclick="showTab('hamburguer',this)">Hambúrguer</button>
<button class="tab-btn" onclick="showTab('bebidas',this)">Bebidas</button>
</div>

<div id="prensados" class="tab-content active">
<div class="menu-grid" id="grid-prensados"></div>
</div>

<div id="hamburguer" class="tab-content">
<div class="menu-grid" id="grid-hamburguer"></div>
</div>

<div id="bebidas" class="tab-content">
<div class="menu-grid" id="grid-bebidas"></div>
</div>

</div>

<div class="cart">
<h3>🛒 Seu Pedido</h3>
<div id="cart-items"></div>
<div class="total" id="total">Total: R$ 0.00</div>

<select id="tipo">
<option value="Delivery">Delivery</option>
<option value="Retirada no balcão">Retirada no balcão</option>
</select>

<input type="text" id="nome" placeholder="Seu nome">
<input type="text" id="endereco" placeholder="Endereço (se delivery)">
<input type="text" id="obs" placeholder="Observação">

<button onclick="finalizarPedido()">Finalizar Pedido</button>
</div>

<footer>
📞 (44) 99983-4671  
<br>
📍 Umuarama - PR  
<br><br>
© 2026 Companhia do Lanche
</footer>

<script>

const produtos = [
{categoria:"prensados", nome:"1º Simples", preco:20},
{categoria:"prensados", nome:"2º Simples c/ Batata Palha", preco:22},
{categoria:"prensados", nome:"3º Especial c/ Bacon", preco:25},
{categoria:"prensados", nome:"13º Especial BIG", preco:40},

{categoria:"hamburguer", nome:"14º X-Salada", preco:25},
{categoria:"hamburguer", nome:"15º X-Bacon", preco:31},
{categoria:"hamburguer", nome:"24º X-Tudo", preco:41},

{categoria:"bebidas", nome:"Refrigerante Lata", preco:7},
{categoria:"bebidas", nome:"Refrigerante 600ml", preco:11},
{categoria:"bebidas", nome:"Água Mineral", preco:4.50}
];

let cart = [];
let total = 0;

function renderMenu(){
produtos.forEach(p=>{
document.getElementById("grid-"+p.categoria).innerHTML += `
<div class="card">
<h4>${p.nome}</h4>
<div class="price">R$ ${p.preco.toFixed(2)}</div>
<button onclick="addItem('${p.nome}',${p.preco})">Adicionar</button>
</div>`;
});
}

function showTab(id,btn){
document.querySelectorAll(".tab-content").forEach(t=>t.classList.remove("active"));
document.querySelectorAll(".tab-btn").forEach(b=>b.classList.remove("active"));
document.getElementById(id).classList.add("active");
btn.classList.add("active");
}

function addItem(nome, preco){
cart.push({nome,preco});
total+=preco;
renderCart();
}

function renderCart(){
let div=document.getElementById("cart-items");
div.innerHTML="";
cart.forEach((item,index)=>{
div.innerHTML+=`
<div class="cart-item">
<span>${item.nome}</span>
<span>R$ ${item.preco.toFixed(2)} 
<span style="color:red;cursor:pointer" onclick="removeItem(${index})">x</span>
</span>
</div>`;
});
document.getElementById("total").innerText="Total: R$ "+total.toFixed(2);
}

function removeItem(i){
total-=cart[i].preco;
cart.splice(i,1);
renderCart();
}

function finalizarPedido(){
let nome=document.getElementById("nome").value;
let endereco=document.getElementById("endereco").value;
let tipo=document.getElementById("tipo").value;
let obs=document.getElementById("obs").value;

if(cart.length===0){alert("Adicione itens!");return;}
if(nome===""){alert("Informe seu nome!");return;}
if(tipo==="Delivery" && endereco===""){alert("Informe o endereço!");return;}

let msg=`🍔 *Pedido - Companhia do Lanche*%0A%0A`;
msg+=`👤 Nome: ${nome}%0A`;
msg+=`📦 Tipo: ${tipo}%0A`;
if(tipo==="Delivery"){msg+=`📍 Endereço: ${endereco}%0A`;}
if(obs!==""){msg+=`📝 Observação: ${obs}%0A`;}
msg+=`%0A🛒 Itens:%0A`;
cart.forEach(i=>{msg+=`- ${i.nome} - R$ ${i.preco.toFixed(2)}%0A`});
msg+=`%0A💰 Total: R$ ${total.toFixed(2)}`;

window.open(`https://wa.me/5544999834671?text=${msg}`,"_blank");
}

renderMenu();

</script>

</body>
</html>
