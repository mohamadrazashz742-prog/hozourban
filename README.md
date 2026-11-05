
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>حضوربان</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">

<style>
*{box-sizing:border-box}
body{margin:0;background:#eef2f7;font-family:Tahoma;color:#263449}
header{background:#173b67;color:white;text-align:center;padding:24px}
header h1{margin:0 0 8px}
main{max-width:550px;margin:25px auto;padding:15px}
.card{background:white;padding:22px;border-radius:15px;margin-bottom:16px;box-shadow:0 3px 12px #0001}
h3{margin-top:0;color:#173b67}
input,button{width:100%;padding:13px;margin:7px 0;border-radius:9px;font-family:Tahoma;font-size:14px}
input{border:1px solid #ccd5df}
button{border:0;background:#1767b2;color:white;font-weight:bold}
button.secondary{background:#53677e}
button.danger{background:#b52c38}
.hidden{display:none}
.item{padding:12px;border-bottom:1px solid #ddd}
small{color:#667}
#map{height:320px;border-radius:12px;margin:12px 0}
.status{background:#e8f1fa;padding:12px;border-radius:9px;margin:10px 0}
</style>
</head>

<body>

<header>
<h1>حضوربان</h1>
سامانه هوشمند حضور و غیاب
</header>

<main>

<section id="home">
<div class="card">
<h3>ورود کارکنان</h3>
<input id="name" placeholder="نام">
<input id="family" placeholder="نام خانوادگی">
<input id="national" placeholder="کد ملی">
<input id="personnel" placeholder="کد پرسنلی">
<button onclick="attendance('ورود')">ثبت ورود</button>
<button class="secondary" onclick="attendance('خروج')">ثبت خروج</button>
</div>

<div class="card">
<h3>ورود مدیریت</h3>
<button onclick="show('login')">ورود به پنل مدیریت</button>
</div>
</section>

<section id="login" class="hidden">
<div class="card">
<h3>ورود مدیر</h3>
<input id="username" placeholder="نام کاربری">
<input id="password" type="password" placeholder="رمز عبور">
<button onclick="adminLogin()">ورود</button>
<button class="secondary" onclick="show('home')">بازگشت</button>
<p id="error"></p>
</div>
</section>

<section id="admin" class="hidden">
<div class="card">
<h3>پنل مدیریت حضوربان</h3>
<button onclick="show('home')">صفحه اصلی</button>
<button class="secondary" onclick="show('employees')">مدیریت کارکنان</button>
<button class="secondary" onclick="show('reports')">گزارش تردد</button>
<button class="secondary" onclick="show('location')">تنظیم محل کار و GPS</button>
<button class="danger" onclick="logout()">خروج مدیر</button>
</div>
</section>

<section id="employees" class="hidden">
<div class="card">
<h3>افزودن کارمند</h3>
<input id="newname" placeholder="نام">
<input id="newfamily" placeholder="نام خانوادگی">
<input id="newnational" placeholder="کد ملی">
<input id="newpersonnel" placeholder="کد پرسنلی">
<button onclick="addEmployee()">ثبت کارمند</button>
<div id="employeeList"></div>
<button class="secondary" onclick="show('admin')">بازگشت</button>
</div>
</section>

<section id="reports" class="hidden">
<div class="card">
<h3>گزارش تردد کارکنان</h3>
<div id="reportList"></div>
<button class="secondary" onclick="show('admin')">بازگشت</button>
</div>
</section>

<section id="location" class="hidden">
<div class="card">
<h3>تنظیم محل کار</h3>
<div class="status">
<strong>شعاع مجاز GPS: ۱۵۰ متر</strong>
<p id="locationStatus">در حال بررسی محل ذخیره شده...</p>
</div>

<p>محل کار را روی نقشه انتخاب کن. نشانگر را روی محل دقیق قرار بده.</p>

<div id="map"></div>

<button onclick="getCurrentWorkLocation()">انتخاب موقعیت فعلی با GPS</button>
<button onclick="saveLocation()">ذخیره موقعیت محل کار</button>
<button class="secondary" onclick="show('admin')">بازگشت</button>
</div>
</section>

</main>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
const RADIUS = 150;

const get = k => JSON.parse(localStorage.getItem(k) || '[]');
const save = (k,v) => localStorage.setItem(k,JSON.stringify(v));

function show(id){
document.querySelectorAll('main section').forEach(s=>s.classList.add('hidden'));
document.getElementById(id).classList.remove('hidden');

if(id==='employees') renderEmployees();
if(id==='reports') renderReports();
if(id==='location'){
setTimeout(initMap,300);
}
}

function adminLogin(){
if(username.value==='684281' && password.value==='Fm21101372'){
show('admin');
}else{
error.innerText='نام کاربری یا رمز عبور اشتباه است';
}
}

function logout(){show('home')}

function addEmployee(){
let e={
name:newname.value.trim(),
family:newfamily.value.trim(),
national:newnational.value.trim(),
personnel:newpersonnel.value.trim()
};

if(!e.name||!e.family||!e.national||!e.personnel){
alert('همه اطلاعات را وارد کنید');
return;
}

let list=get('employees');

if(list.some(x=>x.personnel===e.personnel)){
alert('این کارمند قبلاً ثبت شده است');
return;
}

list.push(e);
save('employees',list);

newname.value='';
newfamily.value='';
newnational.value='';
newpersonnel.value='';

renderEmployees();
alert('کارمند ثبت شد');
}

function renderEmployees(){
let list=get('employees');

employeeList.innerHTML=list.map(e=>
`<div class="item">${safe(e.name)} ${safe(e.family)}
<br><small>کد پرسنلی: ${safe(e.personnel)}</small></div>`
).join('') || 'هنوز کارمندی ثبت نشده است';
}

function safe(v){
return String(v).replace(/[&<>"']/g,c=>({
'&':'&amp;','<':'&lt;','>':'&gt;',
'"':'&quot;',"'":'&#39;'
}[c]));
}

function distance(lat1,lon1,lat2,lon2){
const R=6371000;
const rad=x=>x*Math.PI/180;
const dLat=rad(lat2-lat1);
const dLon=rad(lon2-lon1);

const a=Math.sin(dLat/2)**2+
Math.cos(rad(lat1))*Math.cos(rad(lat2))*
Math.sin(dLon/2)**2;

return 2*R*Math.atan2(Math.sqrt(a),Math.sqrt(1-a));
}

// دریافت محل کار قبلی
function getLocation(){
return JSON.parse(localStorage.getItem('workLocation')||'null');
}

let map=null;
let marker=null;
let selectedLocation=null;

function initMap(){

if(!map){
let old=getLocation();

let center=old ?
[old.lat,old.lng] :
[35.6892,51.3890];

map=L.map('map').setView(center,16);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
attribution:'© OpenStreetMap'
}).addTo(map);

map.on('click',function(e){
selectedLocation={
lat:e.latlng.lat,
lng:e.latlng.lng
};
updateMarker();
});

if(old){
selectedLocation={lat:old.lat,lng:old.lng};
updateMarker();
}
}

setTimeout(()=>map.invalidateSize(),200);

updateLocationStatus();
}

function updateMarker(){
if(!selectedLocation)return;

if(marker){
marker.setLatLng([
selectedLocation.lat,
selectedLocation.lng
]);
}else{
marker=L.marker([
selectedLocation.lat,
selectedLocation.lng
]).addTo(map);
}

}

function updateLocationStatus(){
let old=getLocation();

if(old){
locationStatus.innerText=
'محل کار قبلی ذخیره شده است. شعاع مجاز: ۱۵۰ متر';
}else{
locationStatus.innerText=
'هنوز محل کاری ذخیره نشده است.';
}
}

function getCurrentWorkLocation(){

if(!navigator.geolocation){
alert('مرورگر شما GPS را پشتیبانی نمی‌کند');
return;
}

navigator.geolocation.getCurrentPosition(pos=>{
selectedLocation={
lat:pos.coords.latitude,
lng:pos.coords.longitude
};

if(map){
map.setView([
selectedLocation.lat,
selectedLocation.lng
],17);
updateMarker();
}

},()=>{
alert('دریافت موقعیت ناموفق بود. دسترسی GPS را بررسی کنید.');
},{enableHighAccuracy:true,timeout:20000});
}

function saveLocation(){

if(!selectedLocation){
alert('ابتدا محل کار را روی نقشه انتخاب کنید');
return;
}

localStorage.setItem('workLocation',JSON.stringify({
lat:selectedLocation.lat,
lng:selectedLocation.lng,
radius:RADIUS
}));

updateLocationStatus();

alert('موقعیت محل کار ذخیره شد. شعاع مجاز ۱۵۰ متر است.');
}

// کنترل GPS هنگام ورود و خروج
function attendance(type){

let p=personnel.value.trim();
let n=name.value.trim();
let f=family.value.trim();
let c=national.value.trim();

if(!n||!f||!c||!p){
alert('لطفاً همه اطلاعات را وارد کنید');
return;
}

let employee=get('employees').find(e=>
e.name===n &&
e.family===f &&
e.national===c &&
e.personnel===p
);

if(!employee){
alert('اطلاعات شما در فهرست کارکنان ثبت نشده است');
return;
}

let work=getLocation();

if(!work){
alert('محل کار توسط مدیریت تنظیم نشده است');
return;
}

if(!navigator.geolocation){
alert('GPS در مرورگر شما فعال نیست');
return;
}

navigator.geolocation.getCurrentPosition(pos=>{

let lat=pos.coords.latitude;
let lng=pos.coords.longitude;
let accuracy=pos.coords.accuracy;

if(accuracy>150){
alert('دقت GPS کافی نیست. لطفاً در فضای باز دوباره تلاش کنید.');
return;
}

let d=distance(
lat,lng,work.lat,work.lng
);

if(d>RADIUS){
alert('شما خارج از محدوده مجاز ۱۵۰ متری محل کار هستید.');
return;
}

let records=get('records');

records.push({
name:n+' '+f,
personnel:p,
type:type,
time:new Date().toLocaleString('fa-IR'),
distance:Math.round(d)
});

save('records',records);

alert('ثبت '+type+' با موفقیت انجام شد');

},err=>{

alert('دریافت موقعیت GPS ناموفق بود. لطفاً دسترسی مکان‌یابی مرورگر را فعال کنید.');

},{
enableHighAccuracy:true,
timeout:20000,
maximumAge:0
});

}

function renderReports(){

let list=get('records');

reportList.innerHTML=list.slice().reverse().map(r=>
`<div class="item">
${safe(r.name)}
<br>${safe(r.type)}
<br><small>${safe(r.time)}</small>
</div>`
).join('') || 'هنوز ترددی ثبت نشده است';
}

</script>
</body>
</html>
