# DIP
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DIPVERSE.WEAR</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
<style>
body { font-family:'Poppins',sans-serif; margin:0; padding:0; background:#e0f0ff; }
nav { background: linear-gradient(90deg,#0b3d91,#2196F3); padding:15px 30px; display:flex; justify-content: space-between; align-items:center; color:white; }
nav h1 { margin:0; font-weight:600; }
nav a { color:white; text-decoration:none; margin-left:15px; font-weight:500; }
nav a:hover { text-decoration:underline; }

.container { max-width:1200px; margin:20px auto; display:grid; grid-template-columns: repeat(auto-fit,minmax(250px,1fr)); gap:20px; padding:0 20px; }

.card { background:#fff; border-radius:15px; box-shadow:0 10px 25px rgba(0,0,0,0.1); overflow:hidden; transition: all 0.4s ease; position:relative; }
.card:hover { transform: translateY(-8px); box-shadow:0 15px 30px rgba(0,0,0,0.15); }
.card img { width:100%; height:280px; object-fit:cover; }
.discount { position:absolute; top:10px; left:10px; background:#e60000; color:#fff; padding:5px 10px; border-radius:8px; font-weight:600; font-size:12px; }
.rating { margin:5px 0; color:#ffb400; }

.card-body { padding:15px; }
.card-body h3 { margin:0; font-weight:600; color:#0b3d91; }
.card-body p { margin:5px 0; font-weight:500; color:#333; }

.btn { display:inline-block; padding:10px 15px; margin-top:8px; border:none; border-radius:10px; cursor:pointer; font-weight:600; color:#fff; transition:0.3s ease; text-decoration:none; }
.btn-bkash {background:#e60000;} .btn-bkash:hover {background:#b30000;}
.btn-whatsapp {background:#25D366;} .btn-whatsapp:hover {background:#1ebe5d;}

.login-form, .upload-form { max-width:600px; margin:30px auto; background:#fff; padding:25px; border-radius:20px; box-shadow:0 10px 25px rgba(0,0,0,0.1);}
.login-form input, .upload-form input, .upload-form button, .login-form button { width:100%; margin:10px 0; padding:12px; border-radius:12px; border:1px solid #ccc;}
.upload-form button, .login-form button { background:#0b3d91; color:#fff; border:none; cursor:pointer; font-weight:600; transition:0.3s;}
.upload-form button:hover, .login-form button:hover {background:#06408f;}

.product-list { max-width:600px; margin:20px auto; background:#fff; padding:20px; border-radius:15px; box-shadow:0 8px 20px rgba(0,0,0,0.1);}
.product-item { display:flex; justify-content:space-between; align-items:center; padding:10px 0; border-bottom:1px solid #ddd;}
.product-item:last-child { border-bottom:none; }

footer { background:#0b3d91; color:white; text-align:center; padding:20px; margin-top:30px; font-size:14px;}
footer a { color:#25D366; text-decoration:none; font-weight:600; }
footer a:hover { text-decoration:underline; }

#adminPanel, #loginForm { display:none; }
</style>
</head>
<body>

<nav>
  <h1>DIPVERSE.WEAR</h1>
  <div>
    <a href="#products">Products</a>
    <a href="#" id="adminLink" onclick="showLogin()">Admin</a>
  </div>
</nav>

<!-- Admin Login -->
<div class="login-form" id="loginForm">
  <h2>Admin Login</h2>
  <input type="text" id="adminId" placeholder="Admin ID">
  <input type="password" id="adminPass" placeholder="Password">
  <button onclick="loginAdmin()">Login</button>
</div>

<!-- Admin Panel -->
<div id="adminPanel" class="upload-form">
  <h2>Upload New Dress</h2>
  <input type="text" id="productName" placeholder="Dress Name">
  <input type="number" id="productPrice" placeholder="Price">
  <input type="text" id="productDiscount" placeholder="Discount e.g., 10%">
  <input type="file" id="productImageFile" accept="image/*">
  <button onclick="addProduct()">Add Dress</button>

  <h3 style="margin-top:20px;">Current Products</h3>
  <div class="product-list" id="adminProductList"></div>
</div>

<!-- Product Container -->
<div class="container" id="productContainer">
</div>

<footer>
  &copy; 2025 DIPVERSE.WEAR | WhatsApp: <a href="https://wa.me/01947312109" target="_blank">01947312109</a> | bKash Payment Available
</footer>

<script>
const productContainer = document.getElementById('productContainer');
const adminPanel = document.getElementById('adminPanel');
const loginForm = document.getElementById('loginForm');
const adminProductList = document.getElementById('adminProductList');

function showLogin() { loginForm.style.display='block'; }

function loginAdmin(){
  const id = document.getElementById('adminId').value;
  const pass = document.getElementById('adminPass').value;
  if(id==='XYMIHUL' && pass==='MIHUL0000'){
    loginForm.style.display='none';
    adminPanel.style.display='block';
    alert('Welcome Admin!');
    loadAdminList();
  } else { alert('Wrong ID or Password!'); }
}

// Load products from LocalStorage
function loadProducts(){
  const products = JSON.parse(localStorage.getItem('dipverseProducts')||'[]');
  productContainer.innerHTML='';
  products.forEach(p=>{
    createCard(p.name,p.price,p.discount,p.imgData);
  });
}

// Create Product Card for customers
function createCard(name,price,discount,imgData){
  const card = document.createElement('div');
  card.className='card';
  card.innerHTML=`
    ${discount?`<div class="discount">${discount} OFF</div>`:''}
    <img src="${imgData}" alt="${name}">
    <div class="card-body">
      <h3>${name}</h3>
      <div class="rating">⭐⭐⭐⭐☆</div>
      <p>Price: ৳${price}</p>
      <a href="https://www.bkash.com/payment" target="_blank" class="btn btn-bkash">Pay with bKash</a>
      <a href="https://wa.me/01947312109?text=I am interested in ${encodeURIComponent(name)}" target="_blank" class="btn btn-whatsapp">Message on WhatsApp</a>
    </div>
  `;
  productContainer.appendChild(card);
}

// Admin Panel List
function loadAdminList(){
  adminProductList.innerHTML='';
  const products = JSON.parse(localStorage.getItem('dipverseProducts')||'[]');
  products.forEach((p,index)=>{
    const item = document.createElement('div');
    item.className='product-item';
    item.innerHTML=`${p.name} - ৳${p.price} <button onclick="deleteProduct(${index})" style="background:red;color:white;border:none;padding:5px 10px;border-radius:8px;cursor:pointer;">Delete</button>`;
    adminProductList.appendChild(item);
  });
}

// Add Product
function addProduct(){
  const name = document.getElementById('productName').value;
  const price = document.getElementById('productPrice').value;
  const discount = document.getElementById('productDiscount').value;
  const fileInput = document.getElementById('productImageFile');
  if(!name||!price||!fileInput.files[0]){ alert('Fill all fields'); return; }
  const reader = new FileReader();
  reader.onload=function(e){
    const imgData = e.target.result;
    const products = JSON.parse(localStorage.getItem('dipverseProducts')||'[]');
    products.push({name,price,discount,imgData});
    localStorage.setItem('dipverseProducts',JSON.stringify(products));
    createCard(name,price,discount,imgData);
    loadAdminList();
    // Clear form
    document.getElementById('productName').value='';
    document.getElementById('productPrice').value='';
    document.getElementById('productDiscount').value='';
    document.getElementById('productImageFile').value='';
  }
  reader.readAsDataURL(fileInput.files[0]);
}

// Delete Product
function deleteProduct(index){
  let products = JSON.parse(localStorage.getItem('dipverseProducts')||'[]');
  products.splice(index,1);
  localStorage.setItem('dipverseProducts',JSON.stringify(products));
  loadProducts();
  loadAdminList();
}

// Initial load
loadProducts();
</script>

</body>
</html>
