---
layout: single

excerpt: ""
header:
  overlay_image: /assets/images/banner.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
#  cta_label: "More Info"
#  cta_url: "https://unsplash.com"

sidebar:
    nav: "quicklinks"

---

# Latest News

{% include feature_row_posts %}

---
<button onclick="langChange_en()">English</button>
<button onclick="langChange_es()">Español</button>
<button onclick="langChange_tr()">Türkçe</button>
<button onclick="langChange_ru()">Русский</button>
<button onclick="langChange_cn()">粵語</button>
<button onclick="langChange_tw()">中文</button>
<button onclick="langChange_ar()">العربية</button>
<button onclick="langChange_fa()">فارسی</button>

<!--Censorship and shutdowns vary from blocking specific websites to blocking or throttling entire types of traffic, like https or VoIP protocols. Pluggable Transports can keep sites and apps working as they are meant to when the network is being interfered with in this way.-->

<div id="langWelcome">Welcome to the Pluggable Transports site! Looking for a place to start? Use our Getting Started guide to help you find your way around.
<button onclick="myGuide_en()">Try me!</button> </div>
<br />

<img id="ptComic" src="/assets/images/comic_en.png">

<script>
	function langChange_en() {
       document.getElementById("langWelcome").innerHTML = "Welcome to the Pluggable Transports site! Looking for a place to start? Use our Getting Started guide to help you find your way around. <button onclick=\"myGuide_en()\">Try me!</button>";
       document.getElementById('ptComic').src='/assets/images/comic_en.png';
	}
	function langChange_es() {
       document.getElementById("langWelcome").innerHTML = "Juega a través de nuestra guía a Transportes Conectables <button onclick=\"myGuide_es()\">àqui</button>"; 
       document.getElementById('ptComic').src='/assets/images/comic_es.png';
    }
    function langChange_tr() {
       document.getElementById("langWelcome").innerHTML = "<button onclick=\"myGuide_tr()\">Buraya tıklayarak</button> Değiştirilebilir Taşıyıcılar rehberimize göz atabilirsiniz.";
       document.getElementById('ptComic').src='/assets/images/comic_tr.png';
    }
    function langChange_ru() {
       document.getElementById("langWelcome").innerHTML = "мотрите видео руководство по Подключаемым транспортам <button onclick=\"myGuide_ru()\">здесь</button>";
       document.getElementById('ptComic').src='/assets/images/comic_ru.png';
    }
    function langChange_tw() {
       document.getElementById("langWelcome").innerHTML = "透過可插拔傳輸指示<button onclick=\"myGuide_tw()\">播放</button>";
       document.getElementById('ptComic').src='/assets/images/comic_zhtw.png';
    }
    function langChange_cn() {
       document.getElementById("langWelcome").innerHTML = "我们在<button onclick=\"myGuide_cn()\">此处</button>提供了可插拔传输步骤式的指南";
       document.getElementById('ptComic').src='/assets/images/comic_zhcn.png';
    }
    function langChange_ar() {
       document.getElementById("langWelcome").innerHTML = "<p dir=\"rtl\">تصفح مرشدنا للنواقل الموصولة <button onclick=\"myGuide_ar()\">هنا</button></p>";
       document.getElementById('ptComic').src='/assets/images/comic_ar.png';
    }
    function langChange_fa() {
       document.getElementById("langWelcome").innerHTML = "<p dir=\"rtl\">با راهنمای ما برای حامل‌های جابه‌جاپذیر شروع کنید<button onclick=\"myGuide_fa()\">اینجا</button></p>";
       document.getElementById('ptComic').src='/assets/images/comic_fa.png';
    }
    function myGuide_es() {
    	window.open("/gettingstarted_es.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
    }
	function myGuide_tr() {
    	window.open("/gettingstarted_tr.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
    }
	function myGuide_ru() {
    	window.open("/gettingstarted_ru.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
	}
	function myGuide_cn() {
    	window.open("/gettingstarted_zhcn.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
	}
	function myGuide_tw() {
    	window.open("/gettingstarted_zhtw.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
	}
	function myGuide_en() {
    	window.open("/gettingstarted.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
	}
  function myGuide_ar() {
      window.open("/gettingstarted_ar.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
  }
  function myGuide_fa() {
      window.open("/gettingstarted_fa.html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
  }
</script>

---

# Using the site

If you're interested in Pluggable Transports, it's probably because you are :

- An app developer who is facing censorship and wants to [DEPLOY](/implement/) a transport
- A Transport developer who knows the basics and wants to [CREATE](/build/) a transport
- A user facing censorship, or you're curious and want to [LEARN](/about/) about Pluggable Transports

Follow the links at the top of the page to see the content most relevant for you, or follow our quick links menu to get deeper into the content.
