# Adv-prs-6
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Recommendation System</title>
<style>
body{font-family:Arial;margin:0;background:#f2f4f8;}
header{background:#1e40af;color:white;padding:15px;text-align:center;}
.container{padding:20px;}
.controls{display:flex;flex-wrap:wrap;gap:10px;margin-bottom:20px;}
input,select{padding:8px;}
.products{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:20px;}
.card{background:white;padding:15px;border-radius:10px;box-shadow:0 0 10px rgba(0,0,0,0.1);}
.card img{width:100%;height:180px;object-fit:cover;border-radius:8px;}
.price{color:green;font-weight:bold;}
button{width:100%;padding:8px;background:#2563eb;color:white;border:none;border-radius:5px;cursor:pointer;}
button:hover{background:#1e3a8a;}
</style>
</head>
<body>

<header>
<h2>Smart Product Recommendation System</h2>
</header>

<div class="container">

<div class="controls">
<input type="text" id="search" placeholder="Search products..." onkeyup="displayProducts()">

<select id="categoryFilter" onchange="displayProducts()">
<option value="All">All Categories</option>
<option value="Electronics">Electronics</option>
<option value="Beauty">Beauty</option>
<option value="Home">Home Appliances</option>
<option value="Sports">Sports</option>
<option value="Medicine">Medicine</option>
</select>

<select id="priceFilter" onchange="displayProducts()">
<option value="All">All Prices</option>
<option value="low">Below ₹1000</option>
<option value="mid">₹1000 - ₹10000</option>
<option value="high">Above ₹10000</option>
</select>

<select id="sortOption" onchange="displayProducts()">
<option value="default">Sort By</option>
<option value="lowToHigh">Price: Low to High</option>
<option value="highToLow">Price: High to Low</option>
</select>
</div>

<div class="products" id="productList"></div>

</div>

<script>

const products = [

/* Electronics */
{name:"iPhone 14",category:"Electronics",brand:"Apple",price:70000,keywords:"iphone apple mobile smartphone ios",image:"https://m.media-amazon.com/images/I/61bK6PMOC3L._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Samsung Galaxy S23",category:"Electronics",brand:"Samsung",price:65000,keywords:"samsung galaxy android mobile smartphone",image:"https://m.media-amazon.com/images/I/71goZuIha-L._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"HP Laptop",category:"Electronics",brand:"HP",price:50000,keywords:"hp laptop computer notebook",image:"https://m.media-amazon.com/images/I/71vFKBpKakL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Sony Headphones",category:"Electronics",brand:"Sony",price:2999,keywords:"sony headphones music audio",image:"https://m.media-amazon.com/images/I/61CGHv6kmWL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"OnePlus Smartwatch",category:"Electronics",brand:"OnePlus",price:5999,keywords:"oneplus smartwatch watch wearable",image:"https://m.media-amazon.com/images/I/61nYz6YH1+L._SL1500_.jpg",link:"https://www.amazon.in/"},

/* Beauty */
{name:"Lakme Face Cream",category:"Beauty",brand:"Lakme",price:299,keywords:"lakme cream skincare beauty face",image:"https://m.media-amazon.com/images/I/61X6LxFhPZL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Dove Shampoo",category:"Beauty",brand:"Dove",price:350,keywords:"dove shampoo hair beauty",image:"https://m.media-amazon.com/images/I/61W0u5t1MFL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Nivea Face Wash",category:"Beauty",brand:"Nivea",price:199,keywords:"nivea face wash skincare",image:"https://m.media-amazon.com/images/I/51VqU+9zj5L._SL1000_.jpg",link:"https://www.amazon.in/"},

/* Home Appliances */
{name:"LG Refrigerator",category:"Home",brand:"LG",price:25000,keywords:"lg refrigerator fridge home appliance",image:"https://m.media-amazon.com/images/I/61z6f9XJ5HL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"IFB Washing Machine",category:"Home",brand:"IFB",price:18000,keywords:"ifb washing machine home appliance",image:"https://m.media-amazon.com/images/I/71Vn1bR3vOL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Philips Mixer Grinder",category:"Home",brand:"Philips",price:3500,keywords:"philips mixer grinder kitchen appliance",image:"https://m.media-amazon.com/images/I/61S6l8w3uVL._SL1500_.jpg",link:"https://www.amazon.in/"},

/* Sports */
{name:"Cosco Football",category:"Sports",brand:"Cosco",price:799,keywords:"cosco football sports ball",image:"https://m.media-amazon.com/images/I/71Qd4+3xR-L._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Yonex Badminton Racket",category:"Sports",brand:"Yonex",price:1500,keywords:"yonex badminton racket sports",image:"https://m.media-amazon.com/images/I/61F6L9t6pQL._SL1500_.jpg",link:"https://www.amazon.in/"},
{name:"Yoga Mat",category:"Sports",brand:"Generic",price:499,keywords:"yoga mat exercise fitness sports",image:"https://m.media-amazon.com/images/I/61qQpF6N5GL._SL1500_.jpg",link:"https://www.amazon.in/"},

/* Medicine */
{name:"Dolo 650",category:"Medicine",brand:"Dolo",price:30,keywords:"dolo paracetamol tablet medicine fever",image:"https://m.media-amazon.com/images/I/51vN6R9xTQL._SL1000_.jpg",link:"https://www.amazon.in/"},
{name:"Vitamin C Tablets",category:"Medicine",brand:"HealthKart",price:199,keywords:"vitamin c tablets supplement medicine immunity",image:"https://m.media-amazon.com/images/I/61nJf7pPZAL._SL1500_.jpg",link:"https://www.amazon.in/"}
];

function displayProducts(){

const search=document.getElementById("search").value.toLowerCase();
const category=document.getElementById("categoryFilter").value;
const priceFilter=document.getElementById("priceFilter").value;
const sort=document.getElementById("sortOption").value;

let filtered=products.filter(product=>{

let smartText=(product.name+" "+product.category+" "+product.brand+" "+product.keywords).toLowerCase();
let matchSearch=smartText.includes(search);

let matchCategory=category==="All"||product.category===category;

let matchPrice=true;
if(priceFilter==="low") matchPrice=product.price<1000;
if(priceFilter==="mid") matchPrice=product.price>=1000&&product.price<=10000;
if(priceFilter==="high") matchPrice=product.price>10000;

return matchSearch&&matchCategory&&matchPrice;

});

if(sort==="lowToHigh") filtered.sort((a,b)=>a.price-b.price);
if(sort==="highToLow") filtered.sort((a,b)=>b.price-a.price);

const productList=document.getElementById("productList");
productList.innerHTML="";

filtered.forEach(product=>{
productList.innerHTML+=`
<div class="card">
<img src="${product.image}">
<h3>${product.name}</h3>
<p class="price">₹${product.price}</p>
<a href="${product.link}" target="_blank">
<button>Buy Now</button>
</a>
</div>
`;
});

}

displayProducts();

</script>

</body>
</html>
