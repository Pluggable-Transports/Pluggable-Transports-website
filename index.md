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
<button onclick="langChange_cn()">简体中文</button>
<button onclick="langChange_tw()">繁体中文</button>
<button onclick="langChange_ar()">العربية</button>
<button onclick="langChange_fa()">فارسی</button>

<!--Censorship and shutdowns vary from blocking specific websites to blocking or throttling entire types of traffic, like https or VoIP protocols. Pluggable Transports can keep sites and apps working as they are meant to when the network is being interfered with in this way.-->

<div id="langWelcome">Welcome to the Pluggable Transports site! Looking for a place to start? Use our Getting Started guide to help you find your way around.
<button onclick="myGuide('en')">Try me!</button> </div>
<br />

\* \* \* Check out our [YouTube channel](https://www.youtube.com/channel/UCzJGT8SUVi2_j-qUm6BtZUA) to see the comic strip come alive! \* \* \*

<img id="ptComic" src="/assets/images/comic_en.png">
<a id="ytLang" href="https://youtu.be/Qzy8ubgp_JY"><img src="/assets/images/ytlogo.png" style="display: block;
  margin-left: auto; margin-right: auto;"></a>

<!--<a href="https://youtube.com/channel/UCzJGT8SUVi2_j-qUm6BtZUA"><img src="/assets/images/ytlogo.png" style="display: block;
  margin-left: auto; margin-right: auto;"></a> !-->

<div id="langParas">If you’re interested in Pluggable Transports, it’s probably because you are:

  <ul>
    <li>An app developer who is facing censorship and wants to <a href="/implement/">DEPLOY</a> a transport</li>
    <li>A Transport developer who knows the basics and wants to <a href="/build/">CREATE</a> a transport</li>
    <li>A user facing censorship, or you’re curious and want to <a href="/about/">LEARN</a> about Pluggable Transports</li>
  </ul>
  <p>Follow the links at the top of the page to see the content most relevant for you, or follow our quick links menu to get deeper into the content.</p>
</div>

<script>
  function langChange_en() {
       document.getElementById("langWelcome").innerHTML = "Welcome to the Pluggable Transports site! Looking for a place to start? Use our Getting Started guide to help you find your way around. <button onclick=\"myGuide('en')\">Try me!</button>";
       document.getElementById('langParas').innerHTML = "If you’re interested in Pluggable Transports, it’s probably because you are :</p><ul><li>An app developer who is facing censorship and wants to <a href=\"/implement/\">DEPLOY</a> a transport</li><li>A Transport developer who knows the basics and wants to <a href=\"/build/\">CREATE</a> a transport</li><li>A user facing censorship, or you’re curious and want to <a href=\"/about/\">LEARN</a> about Pluggable Transports</li></ul><p>Follow the links at the top of the page to see the content most relevant for you, or follow our quick links menu to get deeper into the content.</p>";
       document.getElementById('ptComic').src='/assets/images/comic_en.png';
       document.getElementById('ytLang').href='https://youtu.be/Qzy8ubgp_JY';
  }
  function langChange_es() {
       document.getElementById("langWelcome").innerHTML = "Juega a través de nuestra guía a Transportes Conectables <button onclick=\"myGuide('es')\">aquí</button>";
       document.getElementById("langParas").innerHTML = "Algunos de los sitios y servicios más comunes son bloqueados de maneras diferentes en diferentes redes. A veces, éstos bloqueos ocurren a nivel nacional, y son permanentes; o pueden estar ocurriendo alrededor de una elección importante. Lee el reporte anual de Freedom House <a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\" target=\"_blank\">Libertad en la Red</a> para saber más acerca de cuán extendido está ésto.<br><br>Otras veces, puede ser que tu Proveedor de Servicio de Internet (ISP) esté bloqueando acceso a contenido. Un ISP es cualquiera que provee tu conexión de red y no quiere que tengas acceso a algo - puede ser tu cortafuegos corporativo, tu red escolar, o tu compañia de telecomunicaciones.<br><br>En muchos casos, es frustrante cuando no puedes tener el contenido que quieres. Los Transportes Conectables están diseñados para ayudarte a encontrar la mejor manera de llegar al contenido, y hay varios tipos de transporte. La idea detrás de ésto es que todos hablan al sistema en una manera común, y permiten a desarrolladores de aplicaciones y administradores de sistemas implementar una solución que puede acceder múltiples transportes diferentes en un momento dado.";
       document.getElementById('ptComic').src='/assets/images/comic_es.png';
       document.getElementById('ytLang').href='https://youtu.be/MUxpzBSM19w';
    }
    function langChange_tr() {
       document.getElementById("langWelcome").innerHTML = "<button onclick=\"myGuide('tr')\">Buraya tıklayarak</button> Değiştirilebilir Taşıyıcılar rehberimize göz atabilirsiniz.";
       document.getElementById("langParas").innerHTML = "Yaygın kullanılan bazı web sitesi ve hizmetler farklı ağlar üzerinde farklı şekillerde engellenir. Bu engellemeler bazen ulusal düzeyde ve kalıcı olarak yapılırken, bazen de geçici olarak ya da önemli bir seçimden önce yapılır. Engellemelerin ne kadar yaygın olduğunu görmek için Freedom House tarafından hazırlanan yıllık <a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\">Freedom on the Net (İnternette Özgürlük)</a> raporuna bakabilirsiniz.<br><br>Bunun dışında İnternet Hizmeti Sağlayıcınız (ISP) da içeriğe erişmenizi engelleyebilir. İnternet Hizmeti Sağlayıcıları size ağ bağlantısı altyapısını sağladığı için içerik ve hizmetlere erişmenizi engelleyebilen bir kuruluştur. Bunlar, kurumunuzda bulunan güvenlik duvarı yönetimi, okul ağınızın sistem yönetimi ya da telekom şirketiniz gibi çeşitli kuruluşlar olabilir.<br><br>Çoğu zaman istediğiniz içeriğe erişememek sinir bozucu bir durumdur. Değiştirilebilir Taşıyıcılar istediğiniz içeriğe erişmeniz için en iyi yolun bulunmasına yardımcı olacak şekilde tasarlanmıştır. Farklı engellemeleri aşmak için farklı taşıyıcı türleri geliştirilmiştir. Farklı yöntemler kullanan tüm bu taşıyıcıların aynı şekilde iletişim kurması sayesinde, uygulama geliştiricileri ve sistem yöneticileri gerektiği zaman farklı taşıyıcıların kullanılabileceği tek bir çözüm geliştirebilir.";
       document.getElementById('ptComic').src='/assets/images/comic_tr.png';
       document.getElementById('ytLang').href='https://youtu.be/1h6dPAeDPXM';
    }
    function langChange_ru() {
       document.getElementById("langWelcome").innerHTML = "Посмотрите иллюстрированное руководство по Подключаемым Транспортам <button onclick=\"myGuide('ru')\">здесь</button>";
       document.getElementById("langParas").innerHTML = "Некоторые из самых обычных сайтов и сервисов блокируются различными способами в различных сетях. Иногда эти блокировки включаются на уровне государств и действуют постоянно, либо могут происходить во время важных выборов. Прочитайте ежегодный отчет \"<a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\" target=\"_blank\">Свобода в Сети</a>\" от Freedom House, чтобы узнать, насколько это распространено.<br><br>В других случаях, доступ к данным может блокировать ваш интернет-провайдер (ISP).  Провайдер, например, корпоративный файрволл, школьная сеть или телекоммуникационная компания, предоставляет вам сетевое соединение и может не желать давать вам доступ к чему-нибудь.<br><br>Когда вы не можете получить данные, которые хотите, это часто бесит. Подключаемые Транспорты разработаны, чтобы помочь вам найти лучший способ доступа к данным, и существует несколько типов транспортов. Все они взаимодействуют с системой единообразно, позволяя разработчикам и системным администраторам реализовать единое решение, дающее доступ к многим транспортам в любой момент времени.";
       document.getElementById('ptComic').src='/assets/images/comic_ru.png';
       document.getElementById('ytLang').href='https://youtu.be/zINr4iJHGKg';
    }
    function langChange_tw() {
       document.getElementById("langWelcome").innerHTML = "透過可插拔傳輸指示<button onclick=\"myGuide('zhtw')\">播放</button>";
       document.getElementById("langParas").innerHTML = "有些常見的網站與服務在不同的網路環境下以不同方式被封鎖。有時候封鎖情況是全國且永久性，或者其發生在國內重要選舉期間。請參考自由之家的年度<a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\" target=\"_blank\">網路自由</a>報告來了解網路封鎖的狀況。<br><br>其它時候，可能是網路連線服務商（ISP）屏蔽了內容訪問。ISP 指提供網路連線功能的任何成員，它們刻意讓你無法訪問某些網站，ISP 可能是公司的防火牆、學校的網路中心或是電信公司。<br><br>無法取得所要的網路內容實在令人受挫。可插拔傳輸即是從多種傳輸方式中，協助找到近用網路的最佳方法。其背後的想法是這些傳輸都是用相同方式來和系統交流，它們能讓應用程式開發人員和系統管理員在有限時間內透過一個方案的執行來取用多種不同的傳輸。";
       document.getElementById('ptComic').src='/assets/images/comic_zhtw.png';
       document.getElementById('ytLang').href='https://youtu.be/rVS9CDjWi8M';
    }
    function langChange_cn() {
       document.getElementById("langWelcome").innerHTML = "我们在<button onclick=\"myGuide('zhcn')\">此处</button>提供了可插拔传输步骤式的指南";
       document.getElementById("langParas").innerHTML = "在不同网络上，一些常规网站与服务被不同的方式屏蔽。有时，这些屏蔽发生在国家层面上，可能是永久实施，或者在重要选举时期发生。了解更多有关网络屏蔽的广度，请参阅自由之家每年的<a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\" target=\"_blank\">《网络自由》</a> 报告。<br><br>有时，可能是你的互联网服务供应商（ISP）屏蔽了内容的访问。ISP 是指为你提供互联网连接的任何公司，他们不想你访问某些内容：可能是企业防火墙、学校网络或电信公司。<br><br>大多数情况下，无法访问想要访问的内容会令人沮丧气馁。可插拔传输旨在提供访问这些内容的最佳方式，并且存在多种类型传输方式。其背后的理念在于，所有传输方式都通过共同的方式与系统进行通信，并允许应用开发人员和系统管理员部署一种方案，可随时访问多种传输方式。";
       document.getElementById('ptComic').src='/assets/images/comic_zhcn.png';
       document.getElementById('ytLang').href='https://youtu.be/C3_7ejTHWCE';
    }
    function langChange_ar() {
       document.getElementById("langWelcome").innerHTML = "<p dir=\"rtl\">تصفح مرشدنا للنواقل الموصولة <button onclick=\"myGuide('ar')\">هنا</button></p>";
       document.getElementById("langParas").innerHTML = "<div dir=\"rtl\">يتم حظر بعض من أكثر المواقع والخدمات الشائعة بطرق مختلفة، أحيانا يأتي المنع على مستوى قومي وبشكل دائم، وأحيانا يأتي في وقت انتخابات مهمه، اقرأ تقرير <a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\" target=\"_blank\">الحريه على الإنترنت</a> السنوي من فريدوم هاوس لمعرفة مدى انتشار هذه الطرق.<br><br>في أوقات أخرى، يمكن أن يكون مزوّد خدمة الإنترنت الخاص بكم هو من يمنع الوصول إلى المحتوى.المزوّد أو ال ISP هو أي جهة تقدُم لكم الاتصال بالشبكة ولا يريد وصولكم إلى شيء ما - يمكن ان يكون حائط ناري تجاري، أو شبكة مدرسة او شركه الاتصالات الخاصة بكم. <br><br>في كثير من الحالات ، تكون عدم الوصول إلى المحتوى الذين تريدونه أمراً محبطاً. تم تصميم النواقل الموصولة لمساعدتكم في العثورعلى أفضل طريقة للوصول إلى المحتوى، وهناك عدّة أنواع من النواقل. الفكرة من وراء ذلك هي أنها جميعا تتحدّث مع النظام بطرق مشتركة، وتسمح لمطوري التطبيقات و مديري الشبكات بتنفيذ حل واحد يمكنه الوصول إلى عدّة نواقل في أي وقت.</div>";
       document.getElementById('ptComic').src='/assets/images/comic_ar.png';
       document.getElementById('ytLang').href='https://youtu.be/4e0-dFZ0DiQY';
    }
    function langChange_fa() {
       document.getElementById("langWelcome").innerHTML = "<p dir=\"rtl\">با راهنمای ما برای حامل‌های جابه‌جاپذیر شروع کنید<button onclick=\"myGuide('fa')\">اینجا</button></p>";
       document.getElementById("langParas").innerHTML = "<div dir=\"rtl\">برخی از رایج‌ترین سایت‌ها و سرویس‌ها به روش‌های گوناگون در شبکه‌های مختلف مسدود شده‌اند. گاهی اوقات این مسدود شدن‌ها در سطح ملی رخ می‌دهد و به طور دائم مسدود می‌مانند؛  یا ممکن است این مسدود شدن در نزدیکی یک انتخابات مهم رخ بدهد. گزارشِ سالانه‌ی <a href=\"https://freedomhouse.org/report/freedom-net/freedom-net-2018\" target=\"_blank\">آزادی در شبکه‌ی</a> خانه‌‌ی آزادی را بخوانید تا اطلاعات بیشتری در مورد گستردگی این موضوع پیدا کنید.<br><br>در باقی اوقات، ممکن است سرویس‌دهنده‌ی اینترنت (ISP) شما دسترسی به محتوا را مسدود کرده است. یک ISP هر کسی است که اتصالِ شبکه‌ی شما را فراهم می‌کند و نمی‌خواهد شما به چیز خاصی دسترسی پیدا کنید - که ممکن است دیوار آتشینِ شرکت یا شبکه‌ی مدرسه یا شرکتِ شبکه‌‌ی مخابراتی شما باشد.<br><br>در بسیاری از موارد، عدمِ دسترسی به محتوای درخواستی بسیار ناراحت‌کننده است. حامل‌های جابه‌جاپذیر برای این طراحی شده‌اند تا به شما برای پیدا کردن بهترین راه رسیدن به محتوا کمک کنند، و چندین نوع از این حامل‌ها وجود دارد. ایده‌ی پشت این راه این است که همه‌ی آن‌ها از یک راه مشترک با سیستم‌ صحبت می‌کنند، و این به توسعه‌دهنده‌های برنامه و مدیران سیستم اجازه می‌دهد تا راه‌حلی را اجرا کنند که می‌تواند به حامل‌های متفاوت و گوناگون در هر زمان دسترسی داشته باشد.</div>";
       document.getElementById('ptComic').src='/assets/images/comic_fa.png';
       document.getElementById('ytLang').href='https://youtu.be/O-CB-3y3LdE';
    }
    function myGuide(lang) { 
      window.open("/gettingstarted_" + lang + ".html", "_blank", "toolbar=no,scrollbars=yes,resizable=no");
    }
</script>
