
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SMART DIGITAL MARKET</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f4f6f9;color:#222}
header{background:#075e45;color:white;padding:22px 15px;text-align:center}
header h1{font-size:23px;margin:0}
header p{margin:8px 0 0}
nav{display:flex;justify-content:space-around;background:white;padding:14px 5px;position:sticky;top:0;box-shadow:0 2px 6px #ddd}
nav a{color:#075e45;text-decoration:none;font-weight:bold;font-size:13px}
main{padding:15px;max-width:850px;margin:auto}
.search{width:100%;padding:13px;border:1px solid #ccc;border-radius:9px;font-size:16px}
h2{color:#075e45}
.categories{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
.category{background:white;padding:20px 8px;text-align:center;border-radius:10px;box-shadow:0 2px 7px #ddd;font-weight:bold}
.products{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
.product{background:white;padding:12px;border-radius:10px;box-shadow:0 2px 7px #ddd}
.product img{width:100%;height:120px;object-fit:contain}
.product h3{font-size:15px}
.price{color:#087b48;font-weight:bold}
button{background:#087b48;color:white;border:0;padding:11px;border-radius:7px;width:100%;font-weight:bold}
footer{text-align:center;background:#075e45;color:white;padding:20px;margin-top:25px}
</style>
</head>
<body>
<header>
<h1>SMART DIGITAL MARKET</h1>
<p>Your Trusted Online Shopping Market</p>
</header>

<nav>
<a href="#home">Home</a>
<a href="#categories">Categories</a>
<a href="#products">Products</a>
<a href="#help">Help</a>
</nav>

<main id="home">
<input class="search" id="search" placeholder="Search products..." oninput="searchProducts()">

<h2 id="categories">Shop by Category</h2>
<div class="categories">
<div class="category">🛒 Grocery</div>
<div class="category">📚 Stationery</div>
<div class="category">📱 Electronics</div>
<div class="category">📖 Library</div>
</div>

<h2 id="products">Our Products</h2>
<div id="productList" class="products">
<p>Loading products...</p>
</div>

<h2 id="help">Help Center</h2>
<p>Contact us for product and order information.</p>
<p>📞 +91 91018 73368</p>
<p>📞 +91 7635844878</p>
</main>

<footer>
<p>© 2026 SMART DIGITAL MARKET</p>
<p>Thank you for shopping with us.</p>
</footer>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
const SUPABASE_URL = "https://zflumakjreionyqgetdo.supabase.co";
const SUPABASE_KEY = "PASTE_YOUR_PUBLISHABLE_KEY_HERE";

let allProducts = [];

async function loadProducts() {
  const list = document.getElementById("productList");

  if (SUPABASE_KEY === "PASTE_YOUR_PUBLISHABLE_KEY_HERE") {
    list.innerHTML = "<p>Products will appear here after database connection.</p>";
    return;
  }

  try {
    const client = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);
    const { data, error } = await client
      .from("products")
      .select("*")
      .eq("is_active", true);

    if (error) throw error;
    allProducts = data || [];
    showProducts(allProducts);
  } catch (error) {
    list.innerHTML = "<p>Products could not be loaded. Please try again later.</p>";
  }
}

function showProducts(products) {
  const list = document.getElementById("productList");
  if (!products.length) {
    list.innerHTML = "<p>No products available yet.</p>";
    return;
  }

  list.innerHTML = products.map(p => `
    <div class="product">
      <img src="${p.image_url || ""}" alt="Product">
      <h3>${escapeHTML(p.name || "Product")}</h3>
      <p>${escapeHTML(p.description || "")}</p>
      <p class="price">₹${Number(p.price || 0).toFixed(2)}</p>
    </div>
  `).join("");
}

function escapeHTML(value) {
  return String(value).replace(/[&<>"']/g, c => ({
    "&":"&amp;","<":"&lt;",">":"&gt;",
    '"':"&quot;","'":"&#39;"
  }[c]));
}

function searchProducts() {
  const term = document.getElementById("search").value.toLowerCase();
  showProducts(allProducts.filter(p =>
    String(p.name || "").toLowerCase().includes(term)
  ));
}

loadProducts();
</script>
</body>
</html>
<section id="customer-login" style="max-width:420px;margin:30px auto;padding:20px;background:white;border-radius:12px;box-shadow:0 2px 10px #ddd">
  <h2 style="color:#075e45;text-align:center">Customer Login</h2>
  <p>Mobile Number</p>
  <div style="display:flex;gap:8px">
    <input value="+91" readonly style="width:55px;padding:12px">
    <input id="customerMobile" type="tel" maxlength="10" placeholder="10 digit mobile number" style="flex:1;min-width:0;padding:12px">
  </div>
  <button type="button" onclick="demoSendOTP()" style="margin-top:12px">Send OTP</button>
  <p id="otpMessage" style="color:#075e45"></p>
  <p>Enter OTP</p>
  <input id="customerOTP" type="text" maxlength="6" placeholder="Enter demo OTP" style="width:100%;padding:12px">
  <button type="button" onclick="demoVerifyOTP()" style="margin-top:12px">Verify OTP</button>
  <p id="loginResult"></p>
</section>

<script>
function demoSendOTP(){
  const mobile=document.getElementById("customerMobile").value.trim();
  const message=document.getElementById("otpMessage");
  if(!/^[6-9][0-9]{9}$/.test(mobile)){
    message.textContent="Enter a valid 10-digit mobile number.";
    return;
  }
  message.textContent="Demo only: Real SMS OTP is not connected yet.";
}
function demoVerifyOTP(){
  document.getElementById("loginResult").textContent=
    "Real OTP verification is not connected. Please wait for secure setup.";
}
</script>
</body>
</html>
