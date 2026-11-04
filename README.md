
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>حضوربان | سامانه هوشمند حضور و غیاب</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Tahoma,Arial;background:#eef2f7;color:#243247}
header{background:#12345a;color:white;padding:22px;text-align:center}
header h1{margin:0 0 8px;font-size:25px}
header p{margin:0;color:#d5e6f7}
main{max-width:850px;margin:25px auto;padding:12px}
.card{background:white;padding:22px;border-radius:14px;margin-bottom:18px;box-shadow:0 3px 14px #0000000d}
h2{font-size:19px;color:#173e68;margin-top:0}
input,select{width:100%;padding:13px;margin:8px 0 15px;border:1px solid #ccd5df;border-radius:8px;font-family:Tahoma;font-size:14px}
button{border:0;border-radius:9px;padding:13px 18px;background:#1765a5;color:white;font-family:Tahoma;font-size:14px;cursor:pointer;width:100%;margin-top:8px}
button.secondary{background:#e7eef6;color:#173e68}
button.danger{background:#a93636}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.stat{background:#edf4fb;border-radius:10px;padding:16px;text-align:center}
.stat strong{display:block;font-size:23px;color:#155b96;margin-top:8px}
.hidden{display:none}
.note{font-size:12px;color:#69788a;line-height:2}
.menu{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:15px}
.menu button{width:auto;flex:1;min-width:100px}
table{width:100%;border-collapse:collapse;font-size:13px}
td,th{padding:11px;border-bottom:1px solid #e4e9ef;text-align:right}
th{background:#edf4fb}
footer{text-align:center;color:#8491a0;font-size:12px;padding:20px}
@media(max-width:550px){.grid{grid-template-columns:1fr}main{margin:12px auto}.card{padding:17px}}
</style>
</head>
<body>

<header>
<h1>حضوربان</h1>
<p>سامانه هوشمند حضور و غیاب کارکنان</p>
</header>

<main>

<section id="employeePage" class="card">
<h2>ورود کارکنان</h2>
<p class="note">لطفاً اطلاعات شناسایی خود را وارد کنید.</p>

<label>نام و نام خانوادگی</label>
<input id="employeeName" placeholder="نام و نام خانوادگی">

<label>کد ملی</label>
<input id="nationalCode" inputmode="numeric" maxlength="10" placeholder="کد ملی">

<label>کد پرسنلی</label>
<input id="personnelCode" placeholder="کد پرسنلی">

<button onclick="employeeLogin()">ورود به سامانه</button>

<p class="note">
ثبت واقعی ورود و خروج پس از راه‌اندازی پایگاه داده، احراز هویت و کنترل GPS فعال خواهد شد.
</p>
</section>

<section class="card">
<h2>مدیریت سامانه</h2>
<p class="note">
پیش‌نمایش محیط مدیریتی حضوربان. این بخش فعلاً نمایشی است و ورود امن مدیران هنوز به سرور متصل نشده است.
</p>
<button onclick="showAdmin()">مشاهده پنل مدیریت</button>
</section>

<section id="adminPage" class="hidden">

<div class="card">
<h2>پنل مدیریت حضوربان</h2>
<p>مدیران تعریف‌شده در سامانه:</p>
<div class="grid">
<div class="stat">مدیر اول<strong>محمدرضا فیوضی</strong></div>
<div class="stat">مدیر دوم<strong>مصطفی بشردوست</strong></div>
</div>
<p class="note">هر دو مدیر باید در نسخه نهایی حساب مستقل و سطح دسترسی یکسان داشته باشند.</p>
</div>

<div class="card">
<div class="menu">
<button onclick="showSection('dashboard')">داشبورد</button>
<button onclick="showSection('staff')">مدیریت کارکنان</button>
<button onclick="showSection('attendance')">گزارش تردد</button>
<button onclick="showSection('gps')">محدوده GPS</button>
</div>

<div id="dashboard">
<h2>داشبورد مدیریتی</h2>
<div class="grid">
<div class="stat">تعداد کارکنان<strong>۰</strong></div>
<div class="stat">ورودهای امروز<strong>۰</strong></div>
<div class="stat">خروج‌های امروز<strong>۰</strong></div>
<div class="stat">غیبت امروز<strong>۰</strong></div>
</div>
<p class="note">آمار واقعی پس از اتصال پایگاه داده نمایش داده خواهد شد.</p>
</div>

<div id="staff" class="hidden">
<h2>مدیریت کارکنان</h2>
<p class="note">فرم ثبت اولیه کارکنان در نسخه نهایی به پایگاه داده متصل می‌شود.</p>
<label>نام و نام خانوادگی</label>
<input placeholder="نام کارمند">
<label>کد ملی</label>
<input placeholder="کد ملی">
<label>کد پرسنلی</label>
<input placeholder="کد پرسنلی">
<label>منطقه کاری</label>
<select>
<option>انتخاب منطقه کاری</option>
<option>منطقه اول</option>
<option>منطقه دوم</option>
<option>منطقه سوم</option>
</select>
<button onclick="alert('ثبت واقعی کارکنان پس از اتصال سرور فعال می‌شود.')">ثبت کارمند</button>
</div>

<div id="attendance" class="hidden">
<h2>گزارش ورود و خروج</h2>
<table>
<tr><th>نام کارمند</th><th>ورود</th><th>خروج</th><th>وضعیت</th></tr>
<tr><td colspan="4">هنوز اطلاعاتی ثبت نشده است.</td></tr>
</table>
</div>

<div id="gps" class="hidden">
<h2>مدیریت محدوده GPS</h2>
<p class="note">در نسخه نهایی برای هر کارمند محدوده جغرافیایی اختصاصی تعیین خواهد شد.</p>
<label>نام منطقه</label>
<input placeholder="نام محل کار">
<label>شعاع مجاز (متر)</label>
<input type="number" placeholder="مثلاً 100">
<button onclick="alert('تنظیم واقعی GPS نیازمند اتصال سرور و ذخیره مختصات است.')">ثبت محدوده</button>
</div>

<button class="secondary" onclick="hideAdmin()">بازگشت به صفحه کارکنان</button>
</div>
</section>

<div class="card">
<h2>امکانات نسخه نهایی</h2>
<ul class="note">
<li>احراز هویت کارکنان با اطلاعات پرسنلی</li>
<li>ثبت ورود و خروج با GPS</li>
<li>تعیین محدوده مجاز برای هر کارمند</li>
<li>دو حساب مدیریتی مستقل</li>
<li>گزارش تأخیر، غیبت و اضافه‌کاری</li>
<li>خروجی گزارش‌ها و مدیریت پرسنل</li>
</ul>
</div>

</main>

<footer>حضوربان | سامانه هوشمند حضور و غیاب</footer>

<script>
function employeeLogin(){
alert('سامانه هنوز به پایگاه داده و سیستم احراز هویت متصل نشده است.');
}
function showAdmin(){
document.getElementById('adminPage').classList.remove('hidden');
document.getElementById('adminPage').scrollIntoView({behavior:'smooth'});
}
function hideAdmin(){
document.getElementById('adminPage').classList.add('hidden');
window.scrollTo({top:0,behavior:'smooth'});
}
function showSection(id){
['dashboard','staff','attendance','gps'].forEach(x=>{
document.getElementById(x).classList.add('hidden');
});
document.getElementById(id).classList.remove('hidden');
}
</script>
</body>
</html>
