<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>حضوربان | سامانه هوشمند حضور و غیاب</title>
<style>
*{box-sizing:border-box}
body{
 margin:0;
 font-family:Tahoma,Arial,sans-serif;
 background:#eef2f7;
 color:#172033;
}
header{
 background:#123c67;
 color:white;
 padding:25px 15px;
 text-align:center;
}
.logo{font-size:30px;font-weight:bold}
header p{margin-bottom:0;color:#d9e8f8}
.container{
 max-width:480px;
 margin:30px auto;
 padding:15px;
}
.card{
 background:white;
 padding:24px;
 border-radius:16px;
 box-shadow:0 5px 20px #00000012;
}
h2{text-align:center;color:#123c67}
input{
 width:100%;
 padding:14px;
 margin:8px 0;
 border:1px solid #d5dce5;
 border-radius:9px;
 font-size:15px;
}
button{
 width:100%;
 padding:14px;
 border:0;
 border-radius:9px;
 background:#1769aa;
 color:white;
 font-size:16px;
 margin-top:12px;
 cursor:pointer;
}
button.out{background:#bd4545}
.status{
 text-align:center;
 margin:15px 0;
 padding:12px;
 background:#e8f3ff;
 border-radius:9px;
}
footer{text-align:center;color:#778;margin:25px;font-size:12px}
</style>
</head>
<body>

<header>
 <div class="logo">حضوربان</div>
 <p>سامانه هوشمند حضور و غیاب کارکنان</p>
</header>

<div class="container">
 <div class="card" id="login">
  <h2>ورود کارکنان</h2>
  <input id="name" placeholder="نام">
  <input id="family" placeholder="نام خانوادگی">
  <input id="national" placeholder="کد ملی" inputmode="numeric">
  <input id="personnel" placeholder="کد پرسنلی">
  <button onclick="login()">ورود به سامانه</button>
 </div>

 <div class="card" id="panel" style="display:none">
  <h2>پنل تردد کارکنان</h2>
  <div id="welcome" class="status"></div>
  <div id="location" class="status">موقعیت مکانی بررسی نشده است</div>
  <button onclick="record('ورود')">ثبت ورود</button>
  <button class="out" onclick="record('خروج')">ثبت خروج</button>
  <div id="result" class="status">آماده ثبت تردد</div>
  <button onclick="logout()" style="background:#666">خروج از حساب</button>
 </div>
</div>

<footer>حضوربان | مدیریت هوشمند تردد کارکنان</footer>

<script>
let employee=null;

function login(){
 let name=document.getElementById('name').value.trim();
 let family=document.getElementById('family').value.trim();
 let national=document.getElementById('national').value.trim();
 let personnel=document.getElementById('personnel').value.trim();

 if(!name||!family||!national||!personnel){
  alert('لطفاً تمام اطلاعات را وارد کنید');
  return;
 }

 employee={name,family,national,personnel};

 document.getElementById('login').style.display='none';
 document.getElementById('panel').style.display='block';
 document.getElementById('welcome').innerText=
 'خوش آمدید '+name+' '+family;

 if(navigator.geolocation){
  navigator.geolocation.getCurrentPosition(
   p=>{
    document.getElementById('location').innerText=
    'موقعیت GPS دریافت شد';
   },
   ()=>document.getElementById('location').innerText=
   'دسترسی به موقعیت مکانی انجام نشد'
  );
 }
}

function record(type){
 if(!employee)return;
 let now=new Date();
 let time=now.toLocaleString('fa-IR');
 document.getElementById('result').innerText=
 type+' ثبت نمایشی شد | '+time;
}

function logout(){
 employee=null;
 document.getElementById('panel').style.display='none';
 document.getElementById('login').style.display='block';
}
</script>

</body>
</html>
