
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>حضوربان</title>
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

</main>
<script>
const get=(k)=>JSON.parse(localStorage.getItem(k)||'[]');
const save=(k,v)=>localStorage.setItem(k,JSON.stringify(v));

function show(id){
document.querySelectorAll('main section').forEach(s=>s.classList.add('hidden'));
document.getElementById(id).classList.remove('hidden');
if(id==='employees') renderEmployees();
if(id==='reports') renderReports();
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
alert('همه اطلاعات را وارد کنید');return;
}
let list=get('employees');
if(list.some(x=>x.personnel===e.personnel)){
alert('این کارمند قبلاً ثبت شده است');return;
}
list.push(e);save('employees',list);
newname.value=newfamily.value=newnational.value=newpersonnel.value='';
renderEmployees();
alert('کارمند ثبت شد');
}

function renderEmployees(){
let list=get('employees');
employeeList.innerHTML=list.map(e=>
`<div class="item">${e.name} ${e.family}<br><small>کد پرسنلی: ${e.personnel}</small></div>`
).join('')||'هنوز کارمندی ثبت نشده است';
}

function attendance(type){
let p=personnel.value.trim();
let n=name.value.trim(), f=family.value.trim(), c=national.value.trim();
if(!n||!f||!c||!p){alert('لطفاً همه اطلاعات را وارد کنید');return;}
let employee=get('employees').find(e=>
e.name===n&&e.family===f&&e.national===c&&e.personnel===p
);
if(!employee){alert('اطلاعات شما در فهرست کارکنان ثبت نشده است');return;}
let records=get('records');
records.push({name:n+' '+f,type:type,time:new Date().toLocaleString('fa-IR')});
save('records',records);
alert('ثبت '+type+' با موفقیت انجام شد');
}

function renderReports(){
let list=get('records');
reportList.innerHTML=list.slice().reverse().map(r=>
`<div class="item">${r.name}<br>${r.type}<br><small>${r.time}</small></div>`
).join('')||'هنوز ترددی ثبت نشده است';
}
</script>
</body>
</html>
