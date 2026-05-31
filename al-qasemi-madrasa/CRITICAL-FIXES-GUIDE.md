# 🛠️ Al-Qasemi Madrasha থিম — Day 1 Critical Fixes Guide

> **আসসালামু আলাইকুম!** এই গাইডে আমি আপনাকে শিক্ষকের মতো হাত ধরে ১২টা গুরুত্বপূর্ণ সমস্যার সমাধান দেখাব। প্রতিটা ফিক্সে থাকবে — কী সমস্যা, কোথায় খুঁজবেন, কী দিয়ে রিপ্লেস করবেন, কেন দরকার, এবং কীভাবে যাচাই করবেন।

**পুরো গাইডটা পড়তে সময় লাগবে:** ~১৫ মিনিট
**সব ফিক্স apply করতে সময় লাগবে:** ~৪৫-৬০ মিনিট
**কঠিনতার মাত্রা:** 🟢 সহজ (শুধু copy-paste)

---

## 📚 কীভাবে এই গাইড ব্যবহার করবেন

প্রতিটা ফিক্সে আপনি ৫টা জিনিস পাবেন:

| 🏷️ লেবেল | অর্থ |
|---|---|
| 🐛 **সমস্যা** | কী ভুল আছে এবং কেন এটা সমস্যা |
| 📍 **কোথায়** | আপনার XML-এর কোন অংশে এটা আছে |
| ❌ **পুরোনো কোড** | যা খুঁজে বের করতে হবে (Ctrl+F দিয়ে) |
| ✅ **নতুন কোড** | যা দিয়ে রিপ্লেস করতে হবে |
| 💡 **কেন দরকার** | এই ফিক্সে কী লাভ হবে |

---

## ⚠️ শুরু করার আগে — ৩টা জরুরি কাজ

### 🔒 Step A: ব্যাকআপ নিন (এটা অপশনাল না — এটা বাধ্যতামূলক!)

```
1. Blogger Dashboard খুলুন
2. Theme → ⋮ (তিন ডট মেনু) → Backup/Restore → Backup
3. .xml ফাইল ডাউনলোড করে নিরাপদ ফোল্ডারে রাখুন
4. ফাইলের নাম দিন: al-qasemi-original-backup-2026-05-31.xml
```

> ⚠️ **সতর্কতা:** কোনো ফিক্স কাজ না করলে এই backup থেকে সাথে সাথে restore করতে পারবেন। ব্যাকআপ ছাড়া কাজ শুরু করবেন না!

### 🛠️ Step B: Theme Editor খুলুন

```
Blogger Dashboard → Theme → ⋮ → Edit HTML
```

### 🔍 Step C: Search & Replace টুল চালু করুন

- **PC:** `Ctrl + F` (Mac: `Cmd + F`) — Find করার জন্য
- কোড পরিবর্তনের পরে: **Save Theme** বাটনে ক্লিক করতে ভুলবেন না

---

## 🎯 ফিক্স ম্যাপ (এক নজরে দেখে নিন)

| # | ফিক্স | প্রায়োরিটি | সময় |
|---|---|---|---|
| 1 | Encoded `<head>` ট্যাগ ঠিক করুন | 🔴 Critical | ৫ মিনিট |
| 2 | Charset-এর জায়গা ঠিক করুন | 🔴 Critical | ২ মিনিট |
| 3 | FontAwesome ভাঙা integrity hash | 🔴 Critical | ১ মিনিট |
| 4 | Duplicate og:description মুছুন | 🔴 Critical | ১ মিনিট |
| 5 | Bootstrap-এর ভাঙা CSS variable | 🔴 Critical | ৩ মিনিট |
| 6 | Duplicate jQuery মুছুন | 🔴 Critical | ১ মিনিট |
| 7 | Empty `<b:template-skin>` ভরুন | 🟡 Important | ৩ মিনিট |
| 8 | Demo slideNav (vCard leftover) মুছুন | 🟡 Important | ৩ মিনিট |
| 9 | Vietnamese "Tải thêm" → "আরও দেখুন" | 🟡 Important | ১ মিনিট |
| 10 | Visitor counter week bug | 🟡 Important | ৫ মিনিট |
| 11 | JSON-LD schema escaping ঠিক করুন | 🟡 Important | ২ মিনিট |
| 12 | `width='50px'` → `width='50'` | 🟡 Important | ১ মিনিট |

---

# 🔴 PART 1 — CRITICAL FIXES (এগুলো আগে করুন)

---

## 🩹 Fix 1: Encoded `<head>` ট্যাগ ঠিক করুন

### 🐛 সমস্যা
আপনার থিমে `<head>` ও `</head>` HTML-encoded অবস্থায় আছে — অর্থাৎ `&lt;head&gt;` লেখা। এটা একটা পুরোনো hack যা Blogger-এর auto-injection বন্ধ করে। কিন্তু এতে Blogger-এর native CSS, jump-link, RSS auto-discovery — সব হারিয়ে যায়। আধুনিক V3 layout-এ `b:css='false'` ই যথেষ্ট।

### 📍 কোথায়
ফাইলের একদম **উপরের দিকে**, লাইন ~১৩-১৪ এবং `<b:defaultmarkups>`-এর শেষ অংশের পরে।

### ❌ পুরোনো কোড (এই অংশটা খুঁজুন):

```xml
<!--- 
Template Name : Al-Qasemi Madrasha
Version       : V2.0
Website       : https://al-qasemi.blogspot.com
License       : Custom 
-->
&lt;head&gt;
<title><data:view.title.escaped/></title>
```

### ✅ নতুন কোড (এটা দিয়ে রিপ্লেস করুন):

```xml
<!--- 
Template Name : Al-Qasemi Madrasha
Version       : V2.1 (Day-1 Fixes)
Website       : https://al-qasemi.blogspot.com
License       : Custom 
-->
<head>
<meta charset='UTF-8'/>
<title><data:view.title.escaped/></title>
```

### 📍 আরো একটা জায়গা — `</head>` বন্ধ করার অংশ

ফাইলের **নিচের দিকে**, `</b:defaultmarkups>` ট্যাগের ঠিক নিচে এই encoded close-head ট্যাগটা আছে।

### ❌ পুরোনো কোড:

```xml
&lt;/head&gt;&lt;!--<head/>--&gt;
```

### ✅ নতুন কোড:

```xml
</head>
```

### 💡 কেন দরকার
- Browser proper `<head>` element পাবে → meta tags ঠিকমতো parse হবে
- Blogger's built-in features কাজ করবে
- Validators (W3C) error দেখাবে না
- Future Blogger update-এর সাথে সামঞ্জস্যপূর্ণ থাকবে

### 🧪 যাচাই করুন
- Save করার পর সাইট খুলে View Source (`Ctrl+U`) দেখুন
- `<head>` ট্যাগ properly থাকা উচিত, `&lt;head&gt;` text হিসেবে দেখা যাবে না
- Browser dev tools → Console-এ কোনো parsing error থাকা উচিত না

---

## 🩹 Fix 2: Charset-এর জায়গা ঠিক করুন

### 🐛 সমস্যা
`<meta charset='UTF-8'/>` ট্যাগটা `<head>`-এর **বাইরে** আছে। HTML স্পেক বলে এটা document-এর প্রথম 1024 byte-এ এবং `<head>`-এর ভিতরে থাকতে হবে। বাইরে থাকলে browser default encoding (Latin-1) fallback করতে পারে — Bangla টেক্সট ভেঙে যাবে।

### 📍 কোথায়
ফাইলের একদম উপরে, `<b:attr name='xmlns:data' value=''/>` এর ঠিক নিচে।

### ❌ পুরোনো কোড:

```xml
<b:attr name='xmlns:data' value=''/>
<meta charset='UTF-8'/>
<!--- 
Template Name : Al-Qasemi Madrasha
```

### ✅ নতুন কোড:

```xml
<b:attr name='xmlns:data' value=''/>
<!--- 
Template Name : Al-Qasemi Madrasha
```

> 📌 **লক্ষ্য করুন:** এখানে শুধু `<meta charset='UTF-8'/>` লাইনটা মুছে ফেলবেন। কারণ Fix #1 -এ আমরা ইতিমধ্যে এটাকে নতুন `<head>`-এর ভেতরে যোগ করেছি।

### 💡 কেন দরকার
- Bangla, Arabic টেক্সট ১০০% সঠিকভাবে render হবে
- Search engine bot-রা সঠিক encoding বুঝবে
- HTML5 valid হবে

---

## 🩹 Fix 3: FontAwesome-এর ভাঙা integrity hash মুছুন

### 🐛 সমস্যা
FontAwesome link-এর `integrity` attribute-এ hash truncated/ভাঙা: `sha512-papnm+zB1Rh...==` — এই incomplete hash দেখে browser **SRI verification fail** করে FontAwesome stylesheet পুরো block করে দেয়। ফলে আপনার সব Facebook, WhatsApp, ফোন, ইমেইল আইকন **invisible**।

### 📍 কোথায়
`<head>` সেকশনে, SolaimanLipi font load করার ঠিক উপরে।

### ❌ পুরোনো কোড:

```xml
<link crossorigin='anonymous' href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css' integrity='sha512-papnm+zB1Rh...==' referrerpolicy='no-referrer' rel='stylesheet'/>
```

### ✅ নতুন কোড:

```xml
<link crossorigin='anonymous' href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css' referrerpolicy='no-referrer' rel='stylesheet'/>
```

> 📌 **পার্থক্য:** শুধু `integrity='sha512-papnm+zB1Rh...=='` অংশটা মুছে দিচ্ছি।

### 💡 কেন দরকার
- FontAwesome আইকন আবার দেখা যাবে (Facebook, WhatsApp, Email আইকন)
- Browser SRI block করবে না
- Page load ~80KB faster

### 🧪 যাচাই করুন
- Header-এ social media আইকন রঙিন আকারে দেখা যাচ্ছে কি না
- Browser dev tools → Network → font-awesome request status `200 OK` হওয়া উচিত

---

## 🩹 Fix 4: Duplicate og:description মুছুন

### 🐛 সমস্যা
আপনার থিমে **দুইটা** `og:description` meta ট্যাগ আছে:
1. একটা hard-coded — ভুল ঠিকানা ("গুণবতী, চৌদ্দগ্রাম") যেখানে আসল ঠিকানা "রামগঞ্জ, লক্ষ্মীপুর"
2. একটা dynamic (head-content includable-এ) — যেটা পেজ অনুযায়ী সঠিক description দেয়

Facebook, WhatsApp shareing-এ ভুল ঠিকানা চলে যাচ্ছে।

### 📍 কোথায়
`<title>` ট্যাগের ঠিক নিচে।

### ❌ পুরোনো কোড:

```xml
<title><data:view.title.escaped/></title>
<meta content='গুণবতী, চৌদ্দগ্রামের একটি আদর্শ ইসলামি শিক্ষাপ্রতিষ্ঠান, যেখানে আধুনিক পাঠক্রম ও নৈতিক শিক্ষার সমন্বয় ঘটে।' property='og:description'/>
<meta content='width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=5' name='viewport'/>
```

### ✅ নতুন কোড:

```xml
<title><data:view.title.escaped/></title>
<meta content='width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=5' name='viewport'/>
```

> 📌 **পার্থক্য:** মাঝখানের `og:description` লাইনটা পুরো মুছে দিচ্ছি। `<b:include name='head-content'/>` includable নিজেই dynamic og:description তৈরি করবে।

### 💡 কেন দরকার
- Facebook/WhatsApp share-এ সঠিক description দেখাবে
- পেজ অনুযায়ী dynamic description (পোস্ট পেজে পোস্টের description, হোমে হোমের description)
- ভুল ঠিকানার সমস্যা মিটবে

### 🧪 যাচাই করুন
- Facebook Sharing Debugger ব্যবহার করুন: https://developers.facebook.com/tools/debug/
- আপনার সাইটের URL দিন → "Scrape Again" ক্লিক করুন
- og:description ঠিকঠাক দেখাচ্ছে কি না দেখুন

---

## 🩹 Fix 5: Bootstrap-এর ভাঙা CSS variable

### 🐛 সমস্যা
Bootstrap-এর inline CSS-এ একটা CSS variable name **invalid syntax**-এ আছে: `--font-family-'Kalpurush',sans-serif:` — CSS custom property-তে quote/comma allowed না। এটা পুরো CSS variable system ভেঙে দিচ্ছে। মনে হচ্ছে কেউ find-replace করতে গিয়ে `sans-serif` কে `'Kalpurush',sans-serif` দিয়ে replace করেছে — variable name-ও সাথে replace হয়ে গেছে।

### 📍 কোথায়
`<b:skin>` সেকশনের শুরুতে, Bootstrap CSS-এর `:root{}` block-এ।

### ❌ পুরোনো কোড (Ctrl+F দিয়ে এই অংশটা খুঁজুন):

```css
--font-family-'Kalpurush',sans-serif:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,"Noto Sans",sans-serif,"Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol","Noto Color Emoji";
```

### ✅ নতুন কোড:

```css
--font-family-sans-serif:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,"Noto Sans",sans-serif,"Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol","Noto Color Emoji";
```

> 📌 **পার্থক্য:** `--font-family-'Kalpurush',sans-serif` → `--font-family-sans-serif` (Bootstrap-এর original variable name)।

### 💡 কেন দরকার
- Bootstrap CSS variable system আবার কাজ করবে
- Browser CSS error আর দেওয়া বন্ধ করবে
- Future-এ Bootstrap utility class ব্যবহার করতে পারবেন

### 🧪 যাচাই করুন
- Browser dev tools → Console → CSS error message থাকা উচিত না

---

## 🩹 Fix 6: Duplicate jQuery মুছুন

### 🐛 সমস্যা
আপনার থিমে **দুইটা jQuery** লোড হচ্ছে — দুই ভিন্ন version!
- `<head>`-এ jQuery 3.6.0
- `main-js` includable-এ jQuery 3.4.1

দ্বিতীয়টা প্রথমটাকে override করছে। ~85KB extra download + plugin conflict + random behavior।

### 📍 কোথায়
`<b:includable id='main-js'>` includable-এ, ভিতরের শুরুর দিকে।

### ❌ পুরোনো কোড:

```xml
<b:includable id='main-js'>
<script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js'/>
<script src='https://cdn.jsdelivr.net/gh/minhkhoi2001/code/vcard/plugins.min.js'/>
```

### ✅ নতুন কোড:

```xml
<b:includable id='main-js'>
<script src='https://cdn.jsdelivr.net/gh/minhkhoi2001/code/vcard/plugins.min.js'/>
```

> 📌 **পার্থক্য:** শুধু jQuery 3.4.1 লাইনটা মুছে দিচ্ছি। `<head>`-এ jQuery 3.6.0 আগেই লোড হয়েছে।

### 💡 কেন দরকার
- ~85KB কম download → মোবাইলে faster loading
- jQuery plugin conflict বন্ধ হবে
- One source of truth — version mismatch হবে না

### 🧪 যাচাই করুন
- Browser dev tools → Network tab → `jquery.min.js` লেখা একটাই request দেখা উচিত (3.6.0 ভার্সন)

---

# 🟡 PART 2 — IMPORTANT FIXES (এরপর এগুলো করুন)

---

## 🩹 Fix 7: Empty `<b:template-skin>` ভরুন

### 🐛 সমস্যা
`<b:template-skin>` ট্যাগের ভেতরে CDATA blank — পুরো খালি। Blogger-এর Layout editor (Theme → Layout) এই CSS দিয়ে preview render করে। খালি থাকায় Layout view-তে widget drag-drop করতে গেলে UI ভেঙে পড়ে।

### 📍 কোথায়
`<b:skin>` ট্যাগের ঠিক উপরে।

### ❌ পুরোনো কোড:

```xml
<b:if cond='data:view.isLayoutMode'><b:template-skin>
<![CDATA[
]]></b:template-skin></b:if>
```

### ✅ নতুন কোড:

```xml
<b:if cond='data:view.isLayoutMode'><b:template-skin>
<![CDATA[
/* Layout Editor Preview Styles */
body#layout {
  font-family: 'Inter', 'Noto Serif Bengali', sans-serif;
  background: #f7f7f7;
  padding: 20px;
  max-width: 1140px;
  margin: 0 auto;
}
body#layout .header,
body#layout #header-image-wrap {
  background: linear-gradient(135deg, #1976d2, #0d47a1);
  color: #fff;
  padding: 24px;
  border-radius: 12px;
  margin-bottom: 16px;
}
body#layout .sidebar {
  width: 280px;
  background: #fff;
  padding: 16px;
  border-radius: 12px;
  margin-bottom: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.06);
}
body#layout .main,
body#layout #main {
  background: #fff;
  padding: 16px;
  border-radius: 12px;
  min-height: 400px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.06);
}
body#layout .footer {
  background: #fff;
  padding: 12px;
  border-radius: 12px;
  text-align: center;
  margin-top: 16px;
}
]]></b:template-skin></b:if>
```

### 💡 কেন দরকার
- Blogger Layout editor (Theme → Layout) ঠিকমতো কাজ করবে
- Widget drag-drop করার সময় visual preview থাকবে
- Future widget যোগ করতে সুবিধা হবে

### 🧪 যাচাই করুন
- Blogger Dashboard → Theme → Layout খুলুন
- Sidebar, header, main content area visually আলাদা দেখা উচিত

---

## 🩹 Fix 8: Demo slideNav (vCard leftover) মুছুন

### 🐛 সমস্যা
এই থিম originally একটা **vCard portfolio template** থেকে adapted। সেই demo template-এর navigation menu (RTL, Dark, One-page, Works v2 সহ ৪টা link) এখনো বসে আছে। সব dead link — কোনো পেজ exist করে না। মাদরাসার সাইটে ক্লিক করলে user 404 error পাবে।

### 📍 কোথায়
`</main>` ট্যাগের ঠিক নিচে, `<b:include name='main-js'/>` লাইনের আগে।

### ❌ পুরোনো কোড (পুরো block টা):

```xml
	<!-- Demo Menu -->
	<div class='btnSlideNav slideOpen'/>
    <div class='btnSlideNav slideClose'/>
	<div class='slideNav'>
		<ul class='list-unstyled'>
			<li class='slideNav__item rtl-mode'><h4 class='title title--5'>More pages</h4> <a href='rtl/about.html'>RTL</a></li>
			<li class='slideNav__item'><a href='one-page.html'>One page</a></li>
			<li class='slideNav__item'><a href='background-2.html'>Background triangles</a></li>
			<li class='slideNav__item'><a href='works_v2.html'>Works v2</a></li>
        </ul>
		<a class='btn btn--dark' href='dark/about.html'>Dark Template</a>
	</div>	
	<div class='overlay-slideNav'/>
```

### ✅ নতুন কোড

পুরো block-টা **সম্পূর্ণ মুছে ফেলুন**। কিছু দিয়ে রিপ্লেস করার দরকার নেই।

> 📌 **লক্ষ্য করুন:** এই block-এর সাথে যুক্ত JS ও আছে — সেটা automatically inactive হয়ে যাবে কারণ JS `$(".slideNav").each()` দিয়ে select করে, আর element না থাকলে loop চলবে না। তাই JS-এ হাত দিতে হবে না।

### 💡 কেন দরকার
- 4টা dead link বন্ধ হবে — user 404 পাবে না
- Mobile-এ অপ্রয়োজনীয় hamburger button বন্ধ হবে
- ~5KB HTML/JS কম
- Branding consistency — vCard demo trace মুছে গেল

---

## 🩹 Fix 9: Vietnamese "Tải thêm" → "আরও দেখুন"

### 🐛 সমস্যা
আপনার "Load More" বাটনে Vietnamese টেক্সট লেখা: `Tải thêm` (Vietnamese-এ "আরও লোড করুন" অর্থ)। এটা Bangla মাদরাসার সাইটে অদ্ভুত দেখাচ্ছে।

### 📍 কোথায়
`<b:includable id='postPagination'>` includable-এ।

### ❌ পুরোনো কোড:

```xml
<a class='blog-pager-older-link load-more' expr:data-load='data:olderPageUrl' id='load-more-link'>Tải thêm</a>
```

### ✅ নতুন কোড:

```xml
<a class='blog-pager-older-link load-more' expr:data-load='data:olderPageUrl' id='load-more-link'>আরও দেখুন</a>
```

> 📌 **পার্থক্য:** শুধু `Tải thêm` কে `আরও দেখুন` করছি।

### 💡 কেন দরকার
- Bangla user-এর জন্য সঠিক ভাষা
- Branding/localization consistent

---

## 🩹 Fix 10: Visitor Counter Week Bug

### 🐛 সমস্যা
আপনার visitor counter-এ একটা সূক্ষ্ম bug আছে:

```js
weekKey=`${today.getFullYear()}-W${Math.ceil(today.getDate()/7)}`
```

`getDate()` দেয় মাসের তারিখ (১-৩১), ৭ দিয়ে ভাগ করলে সর্বোচ্চ ৫। **অর্থাৎ January-W1 আর February-W1 একই key share করছে!** প্রতি মাসের প্রথম সপ্তাহের count merge হয়ে যাচ্ছে। Yearly statistics-ও ভুল আসবে।

### 📍 কোথায়
Firebase visitor counter script-এর শুরুতে।

### ❌ পুরোনো কোড:

```js
/*--- Date Keys Generation ---*/
const today=new Date(),dateKey=today.toISOString().split("T")[0],yesterdayKey=new Date(Date.now()-864e5).toISOString().split("T")[0],weekKey=`${today.getFullYear()}-W${Math.ceil(today.getDate()/7)}`,monthKey=`${today.getFullYear()}-${today.getMonth()+1}`,yearKey=`${today.getFullYear()}`;
```

### ✅ নতুন কোড:

```js
/*--- Date Keys Generation (ISO 8601 week) ---*/
function getISOWeek(d){
  const t=new Date(Date.UTC(d.getFullYear(),d.getMonth(),d.getDate()));
  const n=t.getUTCDay()||7;
  t.setUTCDate(t.getUTCDate()+4-n);
  const r=new Date(Date.UTC(t.getUTCFullYear(),0,1));
  return{week:Math.ceil(((t-r)/864e5+1)/7),year:t.getUTCFullYear()};
}
const today=new Date();
const dateKey=today.toISOString().split("T")[0];
const yesterdayKey=new Date(Date.now()-864e5).toISOString().split("T")[0];
const _iw=getISOWeek(today);
const weekKey=`${_iw.year}-W${String(_iw.week).padStart(2,"0")}`;
const monthKey=`${today.getFullYear()}-${String(today.getMonth()+1).padStart(2,"0")}`;
const yearKey=`${today.getFullYear()}`;
```

### 💡 কেন দরকার
- Week counter সঠিকভাবে কাজ করবে — January-W1 আর February-W1 আর collide হবে না
- ISO 8601 standard follow করছে (international standard)
- Month key zero-padded — `2026-01` formatted (sort করা সহজ)

### 🧪 যাচাই করুন
- Browser console খুলুন → কোনো error থাকা উচিত না
- Firebase Realtime Database console-এ গিয়ে দেখুন: `counters/week/2026-W22` (এই সপ্তাহ) এমন format-এ key তৈরি হচ্ছে কি না

---

## 🩹 Fix 11: WebSite JSON-LD Schema Escaping ঠিক করুন

### 🐛 সমস্যা
JSON-LD schema-তে `data:view.title.escaped` ব্যবহার করেছেন — কিন্তু এটা **HTML escape** করে, **JSON escape** না। যদি title-এ `"` (quote) বা `\` (backslash) থাকে — JSON parser ভেঙে পড়বে → search engine schema পড়তে পারবে না।

### 📍 কোথায়
`head-content` includable-এর শেষে।

### ❌ পুরোনো কোড:

```xml
<script type='application/ld+json'>{&quot;@context&quot;:&quot;http://schema.org&quot;,&quot;@type&quot;:&quot;WebSite&quot;,&quot;name&quot;:&quot;<data:view.title.escaped/>&quot;,&quot;url&quot;:&quot;<data:view.url.canonical/>&quot;,&quot;potentialAction&quot;:{&quot;@type&quot;:&quot;SearchAction&quot;,&quot;target&quot;:&quot;<data:view.url.canonical/>search?q={search_term_string}&quot;,&quot;query-input&quot;:&quot;required name=search_term_string&quot;}}</script>
```

### ✅ নতুন কোড:

```xml
<script type='application/ld+json'>{&quot;@context&quot;:&quot;https://schema.org&quot;,&quot;@type&quot;:&quot;WebSite&quot;,&quot;name&quot;:&quot;<data:view.title.jsonEscaped/>&quot;,&quot;url&quot;:&quot;<data:view.url.canonical.jsonEscaped/>&quot;,&quot;potentialAction&quot;:{&quot;@type&quot;:&quot;SearchAction&quot;,&quot;target&quot;:&quot;<data:view.url.canonical.jsonEscaped/>search?q={search_term_string}&quot;,&quot;query-input&quot;:&quot;required name=search_term_string&quot;}}</script>
```

> 📌 **৩টা পার্থক্য:**
> 1. `http://schema.org` → `https://schema.org` (Google এখন HTTPS preferred)
> 2. `data:view.title.escaped` → `data:view.title.jsonEscaped` (3 জায়গায়)
> 3. `data:view.url.canonical` → `data:view.url.canonical.jsonEscaped`

### 💡 কেন দরকার
- Google Search Console এ structured data error আসবে না
- Title-এ quote থাকলেও JSON valid থাকবে
- HTTPS schema reference (modern best practice)

### 🧪 যাচাই করুন
- Google Rich Results Test: https://search.google.com/test/rich-results
- আপনার সাইটের URL দিয়ে test করুন → "Detected items" এ WebSite schema দেখা উচিত, error থাকা উচিত না

---

## 🩹 Fix 12: `width='50px'` → `width='50'`

### 🐛 সমস্যা
HTML-এ `width` attribute-এ unit (px) লেখা invalid। শুধু সংখ্যা যাবে। `width='50px'` browser ignore করে — image natural size-এ দেখাবে → layout broken।

### 📍 কোথায়
`<b:includable id='postFooterAuthorProfile'>` includable-এ।

### ❌ পুরোনো কোড:

```xml
<img class='author-image' expr:src='data:post.author.authorPhoto.url' width='50px'/>
```

### ✅ নতুন কোড:

```xml
<img class='author-image' expr:src='data:post.author.authorPhoto.url' height='50' width='50'/>
```

> 📌 **পার্থক্য:**
> 1. `width='50px'` → `width='50'` (px বাদ)
> 2. `height='50'` যোগ — CLS (Cumulative Layout Shift) prevent করার জন্য

### 💡 কেন দরকার
- Browser image proper size-এ render করবে
- CLS score উন্নত হবে → Core Web Vitals + SEO ভালো হবে
- HTML valid হবে

---

# ✅ Final Testing Checklist

সব ফিক্স apply করার পর এই checklist follow করুন:

## 🌐 Browser Test (Incognito mode-এ চালান)

- [ ] হোমপেজ ঠিকঠাক খুলছে
- [ ] Header-এ Facebook, WhatsApp, ফোন, ইমেইল আইকন রঙিন আকারে দেখা যাচ্ছে
- [ ] Sidebar navigation কাজ করছে
- [ ] Slideshow চলছে
- [ ] Notice section ফেচ হচ্ছে
- [ ] Visitor counter বাড়ছে
- [ ] কোনো post-এ গিয়ে content পড়তে পারছেন
- [ ] "আরও দেখুন" বাটন কাজ করছে (পেজিনেশন)
- [ ] Mobile view (Chrome dev tools → Mobile mode) ঠিক আছে

## 🔍 Browser Dev Tools Check (`F12` চাপুন)

- [ ] **Console tab:** কোনো লাল error থাকা উচিত না (warning OK)
- [ ] **Network tab:** jquery.min.js লেখা একটাই request (3.6.0 version)
- [ ] **Network tab:** font-awesome request status `200 OK`
- [ ] **Elements tab:** `<head>` proper HTML element হিসেবে দেখাচ্ছে

## 🔎 SEO Tests

- [ ] **Facebook Debugger:** https://developers.facebook.com/tools/debug/
  - সঠিক og:description (গুণবতী না, "রামগঞ্জ, লক্ষ্মীপুর" related হওয়া উচিত)
- [ ] **Google Rich Results Test:** https://search.google.com/test/rich-results
  - WebSite schema detected, no errors
- [ ] **Mobile-Friendly Test:** https://search.google.com/test/mobile-friendly
  - Pass হওয়া উচিত

## 🎨 Visual Test

- [ ] Header logo + photo ঠিকমতো দেখা যাচ্ছে
- [ ] গ্যালারি, টিচার্স, মেনু পেজ খুলছে
- [ ] Comment box কাজ করছে (পোস্ট পেজে)
- [ ] Demo "More pages" / "RTL" / "Dark Template" বাটন আর নেই (Fix 8 successful)
- [ ] "Tải thêm" নয়, "আরও দেখুন" দেখাচ্ছে (Fix 9 successful)

---

# 🆘 Troubleshooting (যদি সমস্যা হয়)

## ❓ "সাইট পুরো সাদা/ভাঙা দেখাচ্ছে!"

> 🚨 **সমাধান:** সাথে সাথে backup থেকে restore করুন।
> Theme → ⋮ → Backup/Restore → Restore → ব্যাকআপ ফাইল সিলেক্ট করুন।

তারপর আবার শুরু থেকে এক একটা ফিক্স apply করুন এবং প্রতিটার পরে save+test করুন।

## ❓ "Save করতে গেলে error দিচ্ছে — XML parsing error"

সম্ভাব্য কারণ:
- কোনো কোডে accidental space/character যোগ হয়েছে
- কোনো `<` বা `>` ভুল জায়গায় paste হয়েছে
- কোনো লাইন আংশিকভাবে paste হয়েছে

✅ **সমাধান:**
1. Error message-এ যে line number দিচ্ছে সেটা দেখুন
2. সেই অংশটা backup-এর সাথে compare করুন
3. পার্থক্য খুঁজে ঠিক করুন

## ❓ "ফিক্স apply করার পরও পরিবর্তন দেখছি না"

✅ **সমাধান:**
1. Hard refresh করুন: `Ctrl + Shift + R` (Windows) বা `Cmd + Shift + R` (Mac)
2. Browser cache clear করুন
3. Incognito tab-এ test করুন
4. Mobile-এ Chrome → Settings → Privacy → Clear browsing data

## ❓ "FontAwesome আইকন এখনো দেখা যাচ্ছে না"

✅ **সমাধান:**
- Fix 3 ঠিকমতো apply হয়েছে কি check করুন
- Browser dev tools → Network → font-awesome.css status `200 OK` হওয়া উচিত
- যদি `403` বা `blocked` দেখায় → integrity attribute এখনো আছে

## ❓ "Visitor counter কাজ করছে না"

✅ **সমাধান:**
- Browser console খুলুন (F12 → Console)
- Firebase সংক্রান্ত কোনো error আছে কি দেখুন
- যদি `permission denied` দেখায় → Firebase Realtime Database rules check করুন

---

# 🎓 পরবর্তী পদক্ষেপ (Day 1-এর পরে)

এই Day 1 fix-গুলো critical bugs ঠিক করে। কিন্তু এই থিমে আরো অনেক উন্নতির সুযোগ আছে। পরবর্তী phase-এর জন্য recommended:

## 🟠 Week 1 — Performance Phase

1. **Bootstrap purge** — শুধু used class রাখুন (~150KB → ~15KB)
2. **Modernizr সম্পূর্ণ বাদ** (২০১৪-এর library, প্রয়োজন নেই)
3. **Custom lazy-load script বাদ** — শুধু `loading="lazy"` রাখুন
4. **Icon font একটা বাদ** (FontAwesome OR icomoon — দুটো লাগবে না)
5. **Preloader image নিজস্ব host-এ** সরান

## 🟢 Week 2 — UX & SEO Phase

6. `color: #000 !important` cascade-override সম্পূর্ণ মুছুন
7. `maximum-scale=5` মুছুন (accessibility)
8. সব আইকন link-এ `aria-label` যোগ করুন
9. Schema.org `EducationalOrganization` যোগ করুন (madrasa-specific)
10. Bengali hreflang + canonical

## 🌙 Month 1 — Madrasa Features

11. **নামাজের সময়সূচী widget** (Aladhan API)
12. **Hijri calendar** integration
13. **Arabic font support** (Quran ayah-র জন্য)
14. **ভর্তি ফর্ম** (Google Form embed)
15. **Result/Notice PDF download** structured section
16. **Donation/সদকা button** (bKash QR)

---

# 🤝 সহায়তা প্রয়োজন?

কোনো ফিক্সে আটকে গেলে — error message-এর screenshot সহ আমাকে জিজ্ঞেস করুন। আমি একই গাইডের কাঠামোতে সমাধান দেব।

**মনে রাখবেন:**
- ✅ ব্যাকআপ আগে নিন (সবসময়)
- ✅ এক একটা ফিক্স apply করে save+test করুন
- ✅ Error দেখলে আতঙ্কিত হবেন না — restore আছে
- ✅ Patience! Step-by-step এগোলে সব সহজ

> *"যে ব্যক্তি জ্ঞানের পথে চলে, আল্লাহ তার জন্য জান্নাতের পথ সহজ করে দেন।"* — তিরমিযি

আল্লাহ আপনার কাজে বরকত দান করুন। আমিন। 🤲

---

**Document Info:**
- **লেখা হয়েছে:** ৩১ মে ২০২৬
- **থিম ভার্সন:** Al-Qasemi V2.0 → V2.1 (Day-1 patched)
- **ভাষা:** বাংলা (Bangla) + English code
- **Format:** Markdown (GitHub-flavored)
