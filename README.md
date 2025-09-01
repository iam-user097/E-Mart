<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>eMart — LocalStorage E‑Commerce</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#0b1020; --panel:#121833; --muted:#9aa3b2; --text:#eef2ff; --accent:#6aa6ff; --accent2:#9bffcf; --danger:#ff6a6a;
      --ok:#41d695; --warn:#ffb020; --card:#0f1429; --chip:#1b2344; --line:#243056;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,Ubuntu,sans-serif;background:radial-gradient(1200px 800px at 10% -10%,#1b2a52 0%,#0b1020 50%,#070b18 100%);color:var(--text)}
    a{color:inherit}
    .container{max-width:1100px;margin:0 auto;padding:20px}
    .nav{position:sticky;top:0;z-index:40;backdrop-filter:saturate(160%) blur(10px);background:#0b1020cc;border-bottom:1px solid var(--line)}
    .nav-inner{display:flex;gap:12px;align-items:center;justify-content:space-between;padding:12px 20px}
    .brand{display:flex;align-items:center;gap:10px;font-weight:800;letter-spacing:.5px}
    .brand .logo{width:34px;height:34px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#7bd0ff); display:grid;place-items:center;color:#001a33;font-weight:900}
    .tabs{display:flex;gap:8px;flex-wrap:wrap}
    .tab{padding:10px 14px;border:1px solid var(--line);border-radius:12px;background:linear-gradient(180deg,#0f1733,#0c1327);cursor:pointer;transition:.25s transform,.25s background,.25s box-shadow}
    .tab:hover{transform:translateY(-2px);box-shadow:0 10px 24px #00000040}
    .tab.active{background:linear-gradient(180deg,#12214a,#0d1a38);border-color:#2b3c70}

    .bar{display:flex;gap:8px;align-items:center}
    .btn{padding:10px 14px;border:none;border-radius:12px;background:linear-gradient(180deg,#2a3d76,#1d2d58);color:var(--text);cursor:pointer;transition:.25s transform,.25s box-shadow}
    .btn:hover{transform:translateY(-1px);box-shadow:0 10px 20px #00000040}
    .btn.ghost{background:transparent;border:1px solid var(--line)}
    .btn.accent{background:linear-gradient(180deg,var(--accent),#4b7ed1);color:#071027}
    .btn.danger{background:linear-gradient(180deg,#ff8686,#ff6a6a);color:#3b0b0b}

    .grid{display:grid;gap:16px}
    .grid.cols-3{grid-template-columns:repeat(3,minmax(0,1fr))}
    @media (max-width:960px){.grid.cols-3{grid-template-columns:repeat(2,minmax(0,1fr))}}
    @media (max-width:640px){.grid.cols-3{grid-template-columns:1fr}}

    .card{background:linear-gradient(180deg,#121a37,#0f162f);border:1px solid var(--line);border-radius:18px;overflow:hidden;box-shadow:0 12px 30px #00000040;transition:.3s transform,.3s box-shadow}
    .card:hover{transform:translateY(-3px);box-shadow:0 16px 40px #00000066}
    .card-header{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;border-bottom:1px solid var(--line);background:linear-gradient(180deg,#14224a,#101a3b)}
    .card-body{padding:16px}

    .chip{display:inline-flex;align-items:center;gap:8px;padding:6px 10px;border-radius:999px;background:linear-gradient(180deg,#1b254d,#151d3b);border:1px solid var(--line);font-size:12px;color:var(--muted)}

    .field{display:grid;gap:8px;margin-bottom:12px}
    .field label{font-size:12px;text-transform:uppercase;letter-spacing:.08em;color:#c6d3ff}
    .field input,.field select,.field textarea{padding:12px 12px;border-radius:12px;background:#0b1330;border:1px solid #27325e;color:var(--text);outline:none;transition:.2s border,.2s box-shadow}
    .field input:focus,.field select:focus,.field textarea:focus{border-color:#3f5bb5;box-shadow:0 0 0 4px #3f5bb533}

    .row{display:flex;gap:12px}
    .row > *{flex:1}

    .product-card{display:grid;grid-template-rows:180px auto;overflow:hidden}
    .product-card img{width:100%;height:100%;object-fit:cover;background:#0c1735}
    .product-title{font-weight:700}
    .price{display:flex;align-items:center;gap:10px}
    .price .now{font-size:18px;font-weight:800;color:var(--accent2)}
    .price .mrp{color:#9fb0d7;text-decoration:line-through;font-size:13px}
    .price .disc{color:#8bdfff;font-size:12px}

    .hidden{display:none !important}

    /* Transitions */
    .fade-in{opacity:0;transform:translateY(8px);animation:fadeIn .4s ease forwards}
    @keyframes fadeIn{to{opacity:1;transform:none}}
    .slide-up{opacity:0;transform:translateY(16px);animation:slideUp .45s ease forwards}
    @keyframes slideUp{to{opacity:1;transform:none}}

    footer{color:var(--muted);text-align:center;padding:40px 0}
    .muted{color:var(--muted)}

    .tag{padding:4px 10px;border-radius:999px;border:1px solid var(--line);background:#0f1733;font-size:12px}
    .stack{display:flex;gap:8px;flex-wrap:wrap}

    .table{width:100%;border-collapse:separate;border-spacing:0 8px}
    .table th{font-size:12px;color:#c6d3ff;text-align:left;padding:0 10px}
    .table td{background:#0f1630;border:1px solid var(--line);padding:10px;border-left:none;border-right:none}
    .table tr{border-radius:12px}
  </style>
</head>
<body>
  <nav class="nav">
    <div class="nav-inner container">
      <div class="brand">
        <div class="logo">e</div>
        <div>E-Mart</div>
        <span class="chip" id="sessionChip">Guest</span>
      </div>
      <div class="tabs" id="navTabs">
        <button class="tab active" data-view="home">Home</button>
        <button class="tab" data-view="products">Products</button>
        <button class="tab" data-view="cart">Cart</button>
        <button class="tab" data-view="orders">Orders</button>
        <button class="tab" data-view="admin">Admin</button>
      </div>
      <div class="bar">
        <button class="btn ghost" id="btnLogin">Login</button>
        <button class="btn" id="btnLogout" title="Sign out">Logout</button>
      </div>
    </div>
  </nav>

  <main class="container">
    <!-- Home -->
    <section id="view-home" class="fade-in">
      <div class="grid cols-3">
        <div class="card">
          <div class="card-header"><span>Welcome</span><span class="chip">Multi‑User</span></div>
          <div class="card-body">
            <p class="muted">Customers can register/login, add items to cart, and place orders with a delivery address.</p>
            <div class="stack" style="margin-top:10px">
              <span class="tag">Auto price calc</span>
              <span class="tag">Image upload</span>
              <span class="tag">Categories</span>
              <span class="tag">Cart + Orders</span>
            </div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span>Quick Actions</span></div>
          <div class="card-body">
            <div class="stack">
              <button class="btn accent" onclick="go('products')">Browse Products</button>
              <button class="btn" onclick="openLogin()">Login / Register</button>
              <button class="btn ghost" onclick="go('admin')">Open Admin</button>
            </div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span>Status</span></div>
          <div class="card-body">
            <div>Users: <b id="statUsers">0</b></div>
            <div>Products: <b id="statProducts">0</b></div>
            <div>Orders: <b id="statOrders">0</b></div>
          </div>
        </div>
      </div>
    </section>

    <!-- Products -->
    <section id="view-products" class="hidden">
      <div class="row" style="margin-bottom:12px">
        <input id="searchBox" class="field-inp" placeholder="Search products…" style="flex:1;padding:12px 14px;border-radius:12px;background:#0b1330;border:1px solid #27325e;color:var(--text)">
        <select id="filterCategory" class="field-inp" style="width:220px;padding:12px;border-radius:12px;background:#0b1330;border:1px solid #27325e;color:var(--text)"></select>
      </div>
      <div class="grid cols-3" id="productsGrid"></div>
    </section>

    <!-- Cart -->
    <section id="view-cart" class="hidden">
      <div class="card">
        <div class="card-header"><span>Your Cart</span><span class="chip" id="cartUserChip">Guest</span></div>
        <div class="card-body">
          <div id="cartList"></div>
          <div id="cartTotals" style="margin-top:12px"></div>
          <div class="row" style="margin-top:12px">
            <button class="btn ghost" onclick="clearCart()">Clear Cart</button>
            <button class="btn accent" onclick="checkout()">Checkout</button>
          </div>
        </div>
      </div>
    </section>

    <!-- Orders -->
    <section id="view-orders" class="hidden">
      <div class="card">
        <div class="card-header"><span>My Orders</span></div>
        <div class="card-body" id="ordersList"></div>
      </div>
    </section>

    <!-- Admin -->
    <section id="view-admin" class="hidden">
      <div class="grid cols-3">
        <div class="card" style="grid-column:span 1 / span 1">
          <div class="card-header"><span>Add / Edit Product</span><span class="chip" id="adminChip">Admin Only</span></div>
          <div class="card-body">
            <form id="productForm" onsubmit="saveProduct(event)">
              <input type="hidden" id="prodId">
              <div class="field"><label>Name</label><input id="prodName" required></div>
              <div class="row">
                <div class="field"><label>Price (₹)</label><input id="prodPrice" type="number" min="0" step="0.01" required></div>
                <div class="field"><label>Discount (%)</label><input id="prodDisc" type="number" min="0" max="95" step="1" value="0" required></div>
              </div>
              <div class="field"><label>Total (Auto)</label><input id="prodTotal" type="number" readonly></div>
              <div class="field"><label>Category</label>
                <select id="prodCat" required></select>
              </div>
              <div class="field"><label>Image</label>
                <input id="prodImg" type="file" accept="image/*">
                <small class="muted">Leave empty to keep existing image.</small>
                <div style="margin-top:8px"><img id="imgPreview" alt="Preview" style="max-width:100%;border:1px solid var(--line);border-radius:12px;display:none"/></div>
              </div>
              <div class="row" style="margin-top:8px">
                <button class="btn accent" type="submit">Save Product</button>
                <button class="btn ghost" type="button" onclick="resetForm()">Reset</button>
              </div>
            </form>
          </div>
        </div>
        <div class="card" style="grid-column:span 2 / span 2">
          <div class="card-header"><span>Products (Admin View)</span></div>
          <div class="card-body" id="adminProducts"></div>
        </div>
      </div>
    </section>
  </main>

  <footer>
    Built with ❤️ Heart, Sell With 🛒 E-Mart.
  </footer>

  <!-- Auth Modal -->
  <div id="authModal" class="hidden" style="position:fixed;inset:0;background:#0008;display:grid;place-items:center">
    <div class="card" style="width:min(560px,92vw)" onclick="event.stopPropagation()">
      <div class="card-header"><span>Login / Register</span><button class="btn ghost" onclick="closeLogin()">✕</button></div>
      <div class="card-body">
        <div class="grid" style="grid-template-columns:1fr 1fr;gap:16px">
          <div>
            <div class="chip" style="margin-bottom:10px">Admin Login</div>
            <form onsubmit="adminLogin(event)">
              <div class="field"><label>Username</label><input id="adminUser" placeholder="" required></div>
              <div class="field"><label>Password</label><input id="adminPass" type="password" placeholder="" required></div>
              <button class="btn accent" type="submit">Login as Admin</button>
            </form>
          </div>
          <div>
            <div class="chip" style="margin-bottom:10px">Customer</div>
            <div class="row" style="margin-bottom:10px">
              <button class="btn" onclick="switchAuth('login')" id="btnAuthLogin">Login</button>
              <button class="btn ghost" onclick="switchAuth('register')" id="btnAuthReg">Register</button>
            </div>
            <form id="custLoginForm" class="" onsubmit="custLogin(event)">
              <div class="field"><label>Username</label><input id="custUser" required></div>
              <div class="field"><label>Password</label><input id="custPass" type="password" required></div>
              <button class="btn accent" type="submit">Login</button>
            </form>
            <form id="custRegForm" class="hidden" onsubmit="custRegister(event)">
              <div class="field"><label>Username</label><input id="regUser" required></div>
              <div class="field"><label>Contact</label><input id="regContact" placeholder="Phone or Email" required></div>
              <div class="field"><label>Password</label><input id="regPass" type="password" required></div>
              <button class="btn accent" type="submit">Register</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    // ===== Utilities & Storage Layer =====
    const LS={
      productsKey:'emart_products',
      usersKey:'emart_users',
      ordersKey:'emart_orders',
      sessionKey:'emart_session',
      cartKey:(u)=>`emart_cart_${u}`,
    };
    const CATEGORIES=['All','Electronics','Fashion','Home & Kitchen','Beauty','Sports','Books','Grocery'];

    const $=sel=>document.querySelector(sel);
    const el=(tag,attrs={},...kids)=>{const n=document.createElement(tag);Object.entries(attrs||{}).forEach(([k,v])=>{
      if(k==='class') n.className=v; else if(k==='html') n.innerHTML=v; else if(k==='text') n.textContent=v; else n.setAttribute(k,v);
    }); kids.forEach(k=>{if(k==null)return; if(typeof k==='string') n.appendChild(document.createTextNode(k)); else n.appendChild(k)}); return n};

    const read=(k,def)=>{try{const v=localStorage.getItem(k);return v?JSON.parse(v):def}catch(e){return def}}
    const write=(k,v)=>localStorage.setItem(k,JSON.stringify(v))

    function initStorage(){
      if(!read(LS.productsKey)) write(LS.productsKey,[]);
      if(!read(LS.usersKey)) write(LS.usersKey,[]);
      if(!read(LS.ordersKey)) write(LS.ordersKey,[]);
    }

    function now(){return new Date().toISOString()}

    // ===== Session =====
    function session(){return read(LS.sessionKey,null)}
    function setSession(obj){write(LS.sessionKey,obj); paintSession()}
    function clearSession(){localStorage.removeItem(LS.sessionKey); paintSession()}

    function paintSession(){
      const s=session();
      const chip=$('#sessionChip');
      const btnLogin=$('#btnLogin');
      const btnLogout=$('#btnLogout');
      if(s){
        chip.textContent = s.type==='admin' ? 'Admin' : `@${s.username}`;
        btnLogin.classList.add('hidden');
        btnLogout.classList.remove('hidden');
      }else{
        chip.textContent='Guest';
        btnLogin.classList.remove('hidden');
        btnLogout.classList.add('hidden');
      }
      // Session‑guarded views
      $('#adminChip').textContent = 'Admin Only';
      if(s?.type==='admin') $('#adminChip').textContent='Logged as Admin';
      $('#cartUserChip').textContent = s?.type==='customer'?`@${s.username}`:'Guest';
    }

    // ===== Auth Modal =====
    function openLogin(){ $('#authModal').classList.remove('hidden') }
    function closeLogin(){ $('#authModal').classList.add('hidden') }
    $('#btnLogin').onclick=openLogin; $('#btnLogout').onclick=()=>{clearSession(); go('home')}

    function adminLogin(e){ e.preventDefault();
      const u=$('#adminUser').value.trim(); const p=$('#adminPass').value;
      if(u==='emart' && p==='emart1232'){ setSession({type:'admin',username:'emart'}); closeLogin(); go('admin') }
      else alert('Invalid admin credentials');
    }

    function switchAuth(which){
      if(which==='login'){ $('#custLoginForm').classList.remove('hidden'); $('#custRegForm').classList.add('hidden'); }
      else { $('#custRegForm').classList.remove('hidden'); $('#custLoginForm').classList.add('hidden'); }
      $('#btnAuthLogin').classList.toggle('ghost',which!=='login');
      $('#btnAuthReg').classList.toggle('ghost',which!=='register');
    }

    function custRegister(e){ e.preventDefault();
      const users=read(LS.usersKey,[]);
      const username=$('#regUser').value.trim();
      const contact=$('#regContact').value.trim();
      const password=$('#regPass').value;
      if(users.some(u=>u.username.toLowerCase()===username.toLowerCase())) return alert('Username already exists');
      users.push({username,contact,password,createdAt:now()});
      write(LS.usersKey,users);
      alert('Registered! Please login.');
      switchAuth('login');
      $('#custUser').value=username;
    }

    function custLogin(e){ e.preventDefault();
      const users=read(LS.usersKey,[]); const u=$('#custUser').value.trim(); const p=$('#custPass').value;
      const found=users.find(x=>x.username===u && x.password===p);
      if(found){ setSession({type:'customer',username:found.username}); closeLogin(); go('products') }
      else alert('Invalid credentials');
    }

    // ===== Products =====
    function computeTotal(price,disc){ price=parseFloat(price||0); disc=parseFloat(disc||0); return Math.max(0, +(price - (price*disc/100)).toFixed(2)); }

    function onPriceInputs(){ const t=computeTotal($('#prodPrice').value,$('#prodDisc').value); $('#prodTotal').value=t }
    $('#prodPrice').addEventListener('input',onPriceInputs); $('#prodDisc').addEventListener('input',onPriceInputs)

    function loadCategories(){
      const sel=$('#prodCat'); sel.innerHTML=''; CATEGORIES.filter(c=>c!=='All').forEach(c=>sel.appendChild(el('option',{value:c,text:c})));
      const filter=$('#filterCategory'); filter.innerHTML=''; CATEGORIES.forEach(c=>filter.appendChild(el('option',{value:c,text:c})));
      filter.value='All';
    }

    function resetForm(){
      $('#prodId').value=''; $('#prodName').value=''; $('#prodPrice').value=''; $('#prodDisc').value=0; $('#prodTotal').value=''; $('#prodCat').value=CATEGORIES[1]; $('#prodImg').value=''; $('#imgPreview').style.display='none';
    }

    function readImageAsDataURL(file){return new Promise((res,rej)=>{const fr=new FileReader(); fr.onload=()=>res(fr.result); fr.onerror=rej; fr.readAsDataURL(file)})}

    async function saveProduct(e){ e.preventDefault();
      const s=session(); if(s?.type!=='admin') return alert('Admin only');
      const id=$('#prodId').value || crypto.randomUUID();
      const name=$('#prodName').value.trim();
      const price=+$('#prodPrice').value; const discount=+$('#prodDisc').value; const total=computeTotal(price,discount);
      const category=$('#prodCat').value; let image=null;
      const file=$('#prodImg').files[0]; if(file) image=await readImageAsDataURL(file);

      const products=read(LS.productsKey,[]);
      const existing=products.find(p=>p.id===id);
      if(existing){ existing.name=name; existing.price=price; existing.discount=discount; existing.total=total; existing.category=category; if(image) existing.image=image; existing.updatedAt=now(); }
      else { products.push({id,name,price,discount,total,category,image,createdAt:now()}); }
      write(LS.productsKey,products);
      alert('Saved');
      resetForm();
      renderProducts(); renderAdminProducts();
    }

    function renderProducts(){
      const q=$('#searchBox').value?.toLowerCase()||''; const cat=$('#filterCategory').value||'All';
      const wrap=$('#productsGrid'); wrap.innerHTML='';
      const items=read(LS.productsKey,[]).filter(p=> (cat==='All'||p.category===cat) && (!q||p.name.toLowerCase().includes(q)) );
      items.forEach(p=>{
        const card=el('div',{class:'card product-card slide-up'});
        const img=el('img',{src:p.image||'data:image/svg+xml;utf8,'+encodeURIComponent(`<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 300"><rect width="100%" height="100%" fill="#0c1735"/><text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" fill="#7aa2ff" font-family="Inter" font-size="20">No Image</text></svg>`)});
        const body=el('div',{class:'card-body'});
        const title=el('div',{class:'product-title'},p.name);
        const meta=el('div',{class:'muted'},p.category);
        const price=el('div',{class:'price'});
        price.append(el('div',{class:'now'},'₹'+p.total));
        price.append(el('div',{class:'mrp'},'₹'+p.price));
        if(p.discount>0) price.append(el('div',{class:'disc'},`-${p.discount}%`));
        const add=el('div',{class:'row',style:'margin-top:10px'},
          el('button',{class:'btn',onclick:()=>addToCart(p.id)},'Add to Cart'),
          el('button',{class:'btn ghost',onclick:()=>quickBuy(p.id)},'Buy Now')
        );
        body.append(title,meta,price,add);
        card.append(img,body); wrap.append(card);
      })
      $('#statProducts').textContent=read(LS.productsKey,[]).length;
    }

    function renderAdminProducts(){
      const wrap=$('#adminProducts'); wrap.innerHTML='';
      const prods=read(LS.productsKey,[]);
      if(prods.length===0){ wrap.innerHTML='<div class="muted">No products yet.</div>'; return }
      const table=el('table',{class:'table'});
      table.append(el('thead',{},el('tr',{}, el('th',{},'Image'), el('th',{},'Name'),el('th',{},'Category'), el('th',{},'Price'), el('th',{},'Discount'), el('th',{},'Total'), el('th',{},'Actions') )));
      const tb=el('tbody',{});
      prods.forEach(p=>{
        const tr=el('tr',{});
        tr.append(
          el('td',{}, el('img',{src:p.image||'', alt:'', style:'width:60px;height:60px;object-fit:cover;border-radius:8px;background:#0c1735'})),
          el('td',{}, p.name),
          el('td',{}, p.category),
          el('td',{}, '₹'+p.price),
          el('td',{}, p.discount+'%'),
          el('td',{}, '₹'+p.total),
          el('td',{}, el('div',{class:'stack'},
            el('button',{class:'btn',onclick:()=>editProduct(p.id)},'Edit'),
            el('button',{class:'btn danger',onclick:()=>deleteProduct(p.id)},'Delete')
          ))
        );
        tb.append(tr);
      });
      table.append(tb); wrap.append(table);
    }

    function editProduct(id){ const p=read(LS.productsKey,[]).find(x=>x.id===id); if(!p) return; go('admin');
      $('#prodId').value=p.id; $('#prodName').value=p.name; $('#prodPrice').value=p.price; $('#prodDisc').value=p.discount; $('#prodTotal').value=p.total; $('#prodCat').value=p.category; $('#imgPreview').src=p.image||''; $('#imgPreview').style.display=p.image?'block':'none'; window.scrollTo({top:0,behavior:'smooth'});
    }
    function deleteProduct(id){ if(!confirm('Delete this product?')) return; const list=read(LS.productsKey,[]).filter(p=>p.id!==id); write(LS.productsKey,list); renderProducts(); renderAdminProducts() }

    // ===== Cart & Orders =====
    function requireCustomer(){ const s=session(); if(!s||s.type!=='customer'){ alert('Please login as customer to continue'); openLogin(); switchAuth('login'); return false } return true }

    function getCart(){ const s=session(); if(!s||s.type!=='customer') return []; return read(LS.cartKey(s.username),[]) }
    function saveCart(items){ const s=session(); if(!s||s.type!=='customer') return; write(LS.cartKey(s.username),items) }

    function addToCart(id){ if(!requireCustomer()) return; const p=read(LS.productsKey,[]).find(x=>x.id===id); if(!p) return;
      const cart=getCart(); const line=cart.find(i=>i.id===id); if(line) line.qty+=1; else cart.push({id,qty:1}); saveCart(cart); alert('Added to cart'); renderCart() }

    function quickBuy(id){ addToCart(id); go('cart') }

    function renderCart(){
      const wrap=$('#cartList'); wrap.innerHTML='';
      const s=session(); if(!s||s.type!=='customer'){ wrap.innerHTML='<div class="muted">Login to view your cart.</div>'; $('#cartTotals').innerHTML=''; return }
      const prods=read(LS.productsKey,[]); let cart=getCart();
      if(cart.length===0){ wrap.innerHTML='<div class="muted">Cart is empty.</div>'; $('#cartTotals').innerHTML=''; return }

      let subtotal=0; cart.forEach(line=>{ const p=prods.find(x=>x.id===line.id); if(!p) return; const lineTotal=p.total*line.qty; subtotal+=lineTotal;
        const row=el('div',{class:'card',style:'margin-bottom:10px'});
        const inner=el('div',{class:'card-body'});
        inner.append(
          el('div',{class:'row'},
            el('div',{}, el('div',{class:'product-title'},p.name), el('div',{class:'muted'},p.category)),
            el('div',{}, `Qty: `, el('input',{type:'number',min:'1',value:line.qty,style:'width:80px',oninput:(e)=>{ line.qty=Math.max(1, +e.target.value); saveCart(cart); renderCart(); }})),
            el('div',{}, `₹${(p.total*line.qty).toFixed(2)}`),
            el('div',{}, el('button',{class:'btn danger',onclick:()=>{ cart=cart.filter(i=>i.id!==line.id); saveCart(cart); renderCart(); }},'Remove'))
          )
        );
        row.append(inner); wrap.append(row);
      })
      const shipping = subtotal>0 ? 49 : 0;
      const grand = subtotal + shipping;
      $('#cartTotals').innerHTML = `<div class="chip">Subtotal: ₹${subtotal.toFixed(2)}</div> <div class="chip">Shipping: ₹${shipping.toFixed(2)}</div> <div class="chip">Grand Total: <b>₹${grand.toFixed(2)}</b></div>`;
    }

    function clearCart(){ if(!requireCustomer()) return; if(!confirm('Clear cart?')) return; saveCart([]); renderCart() }

    function checkout(){ if(!requireCustomer()) return; const cart=getCart(); if(cart.length===0) return alert('Cart is empty');
      // Address prompt (simple form via prompt)
      const address = prompt('Enter delivery address (House, Street, City, Pincode):');
      if(!address) return;
      const s=session(); const prods=read(LS.productsKey,[]);
      const items=cart.map(line=>{ const p=prods.find(x=>x.id===line.id); return {id:p.id,name:p.name,price:p.total,qty:line.qty,subtotal:+(p.total*line.qty).toFixed(2)} });
      const amount=items.reduce((a,b)=>a+b.subtotal,0) + 49;
      const order={id:crypto.randomUUID(),username:s.username,address,items,amount,createdAt:now(),status:'Placed'};
      const orders=read(LS.ordersKey,[]); orders.push(order); write(LS.ordersKey,orders);
      saveCart([]);
      alert('Order placed!');
      renderOrders(); go('orders');
    }

    function renderOrders(){
      const wrap=$('#ordersList'); const s=session(); wrap.innerHTML='';
      if(!s||s.type!=='customer'){ wrap.innerHTML='<div class="muted">Login to see your orders.</div>'; return }
      const all=read(LS.ordersKey,[]).filter(o=>o.username===s.username);
      if(all.length===0){ wrap.innerHTML='<div class="muted">No orders yet.</div>'; return }
      all.slice().reverse().forEach(o=>{
        const card=el('div',{class:'card',style:'margin-bottom:12px'});
        card.append(el('div',{class:'card-header'}, el('span',{},`#${o.id.slice(0,8)} • ₹${o.amount.toFixed(2)}`), el('span',{class:'chip'},o.status)));
        const body=el('div',{class:'card-body'});
        body.append(el('div',{}, el('b',{},'Deliver to:'), ' ', el('span',{class:'muted'},o.address)));
        const list=el('ul',{});
        o.items.forEach(it=> list.append(el('li',{}, `${it.name} × ${it.qty} — ₹${it.subtotal.toFixed(2)}`)) );
        body.append(list); card.append(body); wrap.append(card);
      })
    }

    // ===== Navigation =====
    function go(view){
      document.querySelectorAll('.tab').forEach(t=>t.classList.toggle('active', t.dataset.view===view));
      ['home','products','cart','orders','admin'].forEach(v=> $('#view-'+v).classList.toggle('hidden', v!==view));
      if(view==='products') renderProducts();
      if(view==='cart') renderCart();
      if(view==='orders') renderOrders();
      if(view==='admin') { const s=session(); if(!s||s.type!=='admin'){ alert('Admin only'); return go('home') } renderAdminProducts(); }
      window.scrollTo({top:0,behavior:'smooth'});
    }

    // Tab clicks
    document.querySelectorAll('.tab').forEach(t=> t.addEventListener('click',()=> go(t.dataset.view)) )

    // Search & Filter
    $('#searchBox').addEventListener('input',renderProducts);
    $('#filterCategory').addEventListener('change',renderProducts);

    // Image preview
    $('#prodImg').addEventListener('change', async (e)=>{ const f=e.target.files?.[0]; if(!f) return; const url=await readImageAsDataURL(f); const im=$('#imgPreview'); im.src=url; im.style.display='block' })

    // ===== Stats =====
    function paintStats(){ $('#statUsers').textContent=read(LS.usersKey,[]).length; $('#statProducts').textContent=read(LS.productsKey,[]).length; $('#statOrders').textContent=read(LS.ordersKey,[]).length }

    // ===== Boot =====
    function boot(){ initStorage(); loadCategories(); paintSession(); paintStats();
      const s=session(); if(s){ if(s.type==='admin') go('admin'); else go('products'); } else go('home');
    }
    document.addEventListener('DOMContentLoaded',boot);
  </script>
</body>
</html>
