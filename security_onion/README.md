Editors: {
	[@meryembusra](https://github.com/meryembusra),
	[@finderskeepers](https://github.com/finderskeepers)
}

![SecurityOnionIcon](http://truica-victor.com/wp-content/uploads/2014/01/securityonion_logo_575x101.jpg)

# **Security Onion**

* <a href='#baslangic'>Başlangıç</a>
  * <a href='#onbakis'>Ön Bakış: Neden Security Onion?</a>
  * <a href='#araclar'>Araçlar</a>
* <a href='#kurulum'>Kurulum</a>
  * <a href='#sistemgereksinimleri'>Sistem Gereksinimleri</a>
  * <a href='#indirmeveyukleme'>İndirme ve Yükleme</a> 
  * <a href='#bootsorunu'>Boot Sorunu</a>
  * <a href='#surumnotlari'>Sürüm Notları</a>
* <a href='#kurulumsonrasi'>Kurulum Sonrası</a>
*  <a href='#mimari'>Mimari</a>
* <a href='#arayuzler'>Arayüzler</a>
 * <a href='#squert'>Squert</a>
 * <a href='#elsa'>ELSA</a>
 * <a href='#sguil'>Sguil</a>
 * <a href='#capme'>CapME</a>
 * <a href='#snorby'>Snorby</a>
* <a href='#datakaynaklari'>Data Kaynakları</a>
 * <a href='#nids'>NIDS</a>
 * <a href='#bro'>Bro</a>
 * <a href='#ossechids'>OSSEC HIDS</a>
 * <a href='#syslog'>Syslog</a>


<h1 id='baslangic'>Başlangıç</h1>
---
<h3 id='onbakis'>Ön Bakış: Neden Security Onion?</h3>
Basitçe ifade etmek gerekirse Network Security Monitoring (NSM), ağdaki güvenlik bazlı etkinlikleri görüntülemek için kullanılır. Zayıflıkları veya süresi dolan SSL sertifikalarını tespit etmek gibi proaktif ya da daha olay bazlı reaktif işlemler için tercih edilebilir. Herhangi bir kişiyi ya da bir zararlı yazılımı takip ediyor olup olmamanıza bakmaksızın, NSM size ağda akıllı ve içeriksel bir farkındalık sağlar. Piyasada bir çok ticari çözümleri olmasına rağmen, çok azı Security Onion gibi 	kapsamlı ve tek bir paket içerisinde bunca çözüme sahiptir.

NSM'in içindeki en önemli kısım olan "M" yani "Monitoring", ağı izlemek olarak düşünülebiliyor olsa da, hiçbir yazılım insan zekası ve farkındalığını sağlayamaz. Herhangi bir zararlı yazılım, NSM tarafından ilk bakışta zararlı yazılım olarak algılanamayabilir. Security Onion size ağ trafiğini açıkça izleme, uyarı ve anormal etkinliklere dair her türlü içeriği sağlayacak olsa da; bir noktada admin olarak sizin ya da bir başka ağ analistinin yardımına ihtiyaç duyacaktır. Security Onion; yükledikten sonra çekip gidip içinizi rahat ettirecek bir uygulama değildir, diğer tüm NSM çözümleri gibi.

**Temel Bileşenler**

Security Onion, 3 temel bileşene aynı anda kusursuzca liderlik eder:

 *  full packet capture
 * ağ ve host bazlı ids (intrusion detection systems)
 * güçlü analiz araçları
  
Full-packet capture işlemi `netsniff-ng` (http://netsniff-ng.org/) aracı tarafından yürütülür. `netsniff-ng`, Security Onion sensorleri tarafından görülen ve kaydedilen tüm trafiği yakalar.

Ağ ve host bazlı saldırı tespit sistemleri (ids); ağ/host trafiğini analiz eder, tespit edilen olay ve aktiviteler için ayrı ayrı loglar ve alert verileri üretir. Security Onion çoklu IDS opsiyonlarını desteklemektedir:

NIDS (ağ bazlı saldırı tespit sistemi):

* Kural temelli NIDS. Bu sistemler için Security Onion `Snort` (http://snort.org) ya da `Suricata` (http://suricata-ids.org) seçeneklerini sunar. Kural temelli saldırı tespit sistemleri; bilinen zararlı yazılımlar, anormal ya da şüpheli trafik tespiti için ağ trafiğindeki parmak izlerine bakar. Her ne kadar antivirüs programları ile aynı işi yapıyor gibi gözükse de, biraz daha derin ve esnektirler.

* Analiz temelli NIDS. Bu sistemler için Security Onion `The Bro Network Security Monitor (Bro IDS)` (http://bro-ids.org) çözümünü kullanır. Kural temelli tespit sistemlerinin aksine, Bro tüm verileri toplar ve gördüğü her şeyi yapmak istediğiniz analiz işlemlerinde kullanabilmeniz için size verir. Bro, trafiği izler ve her bağlantıyı loglar(DNS istekleri, SSL sertifikaları, HTTP, FTP, IRC SMTP, SSH aktiviteleri gibi...). Ayrıca genel bir dosya analizini de (MD5 gibi) içinde bulundurur.

HIDS (host bazlı saldırı tespit sistemi):

*  Host bazlı saldırı tespit sistemleri için Security Onion `OSSEC` (http://ossec.net) opsiyonunu sunmaktadır. OSSEC; ücretsiz, açık kaynak ve cross-platform (Windows, Linux ve Mac OS) bir HIDS'dir. Ağın son noktasına eklendiği zaman, burası ile ağın çıkışı arasında etkin bir izleme olanağı sağlar. Log analizi, dosya bütünlüğü kontrolü ve rootkit tespiti gibi işlemleri yapar.


**Analiz Araçları**

Full packet capture, IDS logları ve Bro verileri ile birlikte, elinizde korkunç miktarda veri birikmiş olur. Neyse ki, Security Onion bu verileri anlamlı kılmak için aşağıdaki araçlara sahip:

*  `Sguil` (http://sguil.sourceforge.net/), tcl'de (tool command language) yazılmış basit bir GUI aracılığı ile Snort veya Suricata alertlerini, OSSEC alertlerini, Bro HTTP eventlerini ve PRADS (passive real-time asset detection system) alertlerini görüntüleme işlemleri için kullanılır. En önemli kısmı ise, tetiklenen alert'e sebep olan tüm trafiği ya da etkinliği görebilme olanağı sunması. Tek bir olayın aksine, o alert ile ilişkili tüm olayları bir bütün olarak görmek büyük bir avantaj sağlar.

* `Squert`(http://www.squertproject.org/), Sguil veritabanı için kullanılan bir web application arayüzüdür. Sguil veritabanındaki verilerin zaman, yoğunluk gibi parametreler ile sıralı bir şekilde görüntülenmesini sağlar.

* `ELSA` (Enterprise Log Search and Archive), syslog-ng, MySQL ve Sphinx full-text search üzerinde yapılandırılmış bir merkezi syslog frameworküdür. Tamamiyle asenkron çalışır ve web temelli bir query arayüzü olarak logları normalize eder ve milyarlarca log üzerinden istenilen stringleri web'de arama yaparcasına hızlı ve kolayca halledebilir. Ayrıca, belli alert ve logların herkes tarafından görüntülenmesi istenmiyorsa, izin ayarlamaları da yapabilir. Security Onion tarafından toplanmış her türlü veriyi efor sarfetmeden, kolayca ve istenilen şekilde görüntüleyebildiği ve sıralayabildiği için, çok güçlü bir araçtır. Resmi dökümanı https://github.com/mcholste/elsa adresinden görüntülenebilir. 

**Uygulama Senaryoları**

Security Onion dağıtık bir server-client modeli üzerine kuruludur. Security Onion "sensor", clienti temsil eder. Server ve sensor bileşenleri tek bir fiziksel/sanal makine üzerinde çalışabileceği gibi, birden fazla sensor de belirlenmiş bir server'a hizmet vermek amacıyla kullanılabilir. Aşağıdaki 3 farklı senaryo, Security Onion'un uygulama alanlarını temsil etmektedir:

* **Standalone:** Tek bir fiziksel/sanal makinede hem server hem de sensor bileşenlerinin çalıştığı senaryodur. Farklı ağ bölümlerini izlemek için birden fazla ağ arayüzü içerebilir. Tek bir konumdan erişilebilen ağlar için en basit ve en uygun çözümdür.

* **Server-sensor:** Tek bir makine üzerinde çalışan server bileşeni ve bir veya birden fazla makinede çalışan sensor bileşenlerini içeren senaryodur. Dinleme işlemleri, yakalanan paketlerin saklanması, IDS alertleri ve ELSA ve Sguil için veritabanı işlemleri sensorler tarafından yürütülür. Analist, ayrı bir client'tan server'a bağlanır, istediği bilgiler ayrık sensorler tarafından server'a gönderilir ve buradan da client'a yönlendirilir. Bu senaryo, analistin client'ı tarafından herhangi bir bilgi istenmedikçe, toplanan veri yığınını sensorler üzerinde tutarak ağ trafiğini rahatlatır. Sensor(ler)-server ve client-server arasındaki tüm haberleşme SSH ecnrypted tunnels tarafından korunur.

* **Hybrid:** Standalone kurulum ve birden fazla ayrı sensorlerin bu standalone kurulumdaki server bileşenine geri dönüş gerçekleştirdiği senaryodur.

> En son hali ile elimizde: Full-packet capture, Snort veya Suricata kural temelli saldırı tespiti, Bro olay temelli saldırı tespiti, OSSEC host bazlı saldırı tespiti sistemleri var ve hepsi tek bir kutuda toplanmış halde tek bir Security Onion kurulumu dahilinde gelmekte. Başka türlü saatler, günler, hatta haftalar sürecek bu birbirinden tamamen farklı sistemler; çeşitli bağımlılık ve karmaşıklıklarla sorunsuz bir biçimde çalışmaktadır. Bir zamanlar imkansız gibi gözüken her şey, Security Onion sayesinde Windows işletim sistemini kurmak kadar kolay!

 

<h3 id='araclar'>Araçlar</h3>

Security Onion tarafından kullanılan tüm araçlar ve resmi internet adresleri aşağıda yer almaktadır. 


<p><strong>argus</strong> <br>
 <a href="https://nmap.org/download.html"> </a><a href="https://nmap.org/download.html">https://nmap.org/download.html</a> <br>
<strong>barnyard2</strong> <br>
 <a href="http://www.securixlive.com/barnyard2/"> </a><a href="http://www.securixlive.com/barnyard2/">http://www.securixlive.com/barnyard2/</a> <br>
<strong>bro</strong> <br>
 <a href="http://bro-ids.org/"> </a><a href="http://bro-ids.org/">http://bro-ids.org/</a> <br>
<strong>chaosreade</strong> <br>
 <a href="http://chaosreader.sourceforge.net/"> </a><a href="http://chaosreader.sourceforge.net/">http://chaosreader.sourceforge.net/</a> <br>
<strong>Daemonlogger</strong> <br>
 <a href="http://www.snort.org/snort-downloads/additional-downloads#daemonlogger"> </a><a href="http://www.snort.org/snort-downloads/additional-downloads#daemonlogger">http://www.snort.org/snort-downloads/additional-downloads#daemonlogger</a> <br>
<strong>driftnet</strong> <br>
 <a href="http://www.ex-parrot.com/~chris/driftnet/"> </a><a href="http://www.ex-parrot.com/~chris/driftnet/">http://www.ex-parrot.com/~chris/driftnet/</a> <br>
<strong>dsniff</strong> <br>
 <a href="http://www.monkey.org/~dugsong/dsniff/"> </a><a href="http://www.monkey.org/~dugsong/dsniff/">http://www.monkey.org/~dugsong/dsniff/</a> <br>
<strong>ELSA</strong> <br>
<a href="http://code.google.com/p/enterprise-log-search-and-archive/"></a><a href="http://code.google.com/p/enterprise-log-search-and-archive/">http://code.google.com/p/enterprise-log-search-and-archive/</a> <br>
<strong>hping</strong> <br>
<a href="http://www.hping.org/"></a><a href="http://www.hping.org/">http://www.hping.org/</a> <br>
<strong>hunt</strong> <br>
<strong>labrea</strong> <br>
<a href="http://labrea.sourceforge.net/labrea-info.html"></a><a href="http://labrea.sourceforge.net/labrea-info.html">http://labrea.sourceforge.net/labrea-info.html</a> <br>
<strong>mergecap</strong> <br>
<a href="http://www.wireshark.org/docs/man-pages/mergecap.html"></a><a href="http://www.wireshark.org/docs/man-pages/mergecap.html">http://www.wireshark.org/docs/man-pages/mergecap.html</a> <br>
<strong>netsed</strong> <br>
<strong>netsniff-ng</strong> <br>
<a href="http://netsniff-ng.org/"></a><a href="http://netsniff-ng.org/">http://netsniff-ng.org/</a> <br>
<strong>NetworkMiner</strong> <br>
<a href="http://www.netresec.com/?page=NetworkMiner"></a><a href="http://www.netresec.com/?page=NetworkMiner">http://www.netresec.com/?page=NetworkMiner</a> <br>
<strong>ngrep</strong> <br>
<a href="http://ngrep.sourceforge.net/"></a><a href="http://ngrep.sourceforge.net/">http://ngrep.sourceforge.net/</a> <br>
<strong>OSSEC</strong> <br>
<a href="http://www.ossec.net/"></a><a href="http://www.ossec.net/">http://www.ossec.net/</a> <br>
<strong>p0f</strong> <br>
<a href="http://lcamtuf.coredump.cx/p0f3/"></a><a href="http://lcamtuf.coredump.cx/p0f3/">http://lcamtuf.coredump.cx/p0f3/</a> <br>
<strong>Reassembler</strong> <br>
<a href="http://isc.sans.edu/diary.html?storyid=13282"></a><a href="http://isc.sans.edu/diary.html?storyid=13282">http://isc.sans.edu/diary.html?storyid=13282</a> <br>
<strong>scapy</strong> <br>
<a href="http://www.secdev.org/projects/scapy/"></a><a href="http://www.secdev.org/projects/scapy/">http://www.secdev.org/projects/scapy/</a> <br>
<strong>sguil</strong> <br>
<a href="http://sguil.sourceforge.net/"></a><a href="http://sguil.sourceforge.net/">http://sguil.sourceforge.net/</a> <br>
<strong>Sniffit</strong> <br>
<a href="http://sniffit.sourceforge.net/"></a><a href="http://sniffit.sourceforge.net/">http://sniffit.sourceforge.net/</a> <br>
<strong>Snort</strong> <br>
<a href="http://www.snort.org/"></a><a href="http://www.snort.org/">http://www.snort.org/</a> <br>
<strong>Squert</strong> <br>
<a href="http://www.squertproject.org/"></a><a href="http://www.squertproject.org/">http://www.squertproject.org/</a> <br>
<strong>ssldump</strong> <br>
<a href="http://www.rtfm.com/ssldump/"></a><a href="http://www.rtfm.com/ssldump/">http://www.rtfm.com/ssldump/</a> <br>
<strong>sslsniff</strong> <br>
<a href="http://www.thoughtcrime.org/software/sslsniff/"></a><a href="http://www.thoughtcrime.org/software/sslsniff/">http://www.thoughtcrime.org/software/sslsniff/</a> <br>
<strong>Suricata</strong> <br>
<a href="http://www.openinfosecfoundation.org/index.php/download-suricata"></a><a href="http://www.openinfosecfoundation.org/index.php/download-suricata">http://www.openinfosecfoundation.org/index.php/download-suricata</a> <br>
<strong>tcpdump</strong> <br>
<a href="http://www.tcpdump.org/"></a><a href="http://www.tcpdump.org/">http://www.tcpdump.org/</a> <br>
<strong>tcpick</strong> <br>
<a href="http://tcpick.sourceforge.net/"></a><a href="http://tcpick.sourceforge.net/">http://tcpick.sourceforge.net/</a> <br>
<strong>tcpreplay</strong> <br>
<a href="http://tcpreplay.synfin.net/"></a><a href="http://tcpreplay.synfin.net/">http://tcpreplay.synfin.net/</a> <br>
<strong>tcpslice</strong> <br>
<a href="http://sourceforge.net/projects/tcpslice/"></a><a href="http://sourceforge.net/projects/tcpslice/">http://sourceforge.net/projects/tcpslice/</a> <br>
<strong>tcpstat</strong> <br>
<a href="http://www.frenchfries.net/paul/tcpstat/"></a><a href="http://www.frenchfries.net/paul/tcpstat/">http://www.frenchfries.net/paul/tcpstat/</a> <br>
<strong>tcpxtract</strong> <br>
<a href="http://tcpxtract.sourceforge.net/"></a><a href="http://tcpxtract.sourceforge.net/">http://tcpxtract.sourceforge.net/</a> <br>
<strong>tshark</strong> <br>
<a href="http://www.wireshark.org/docs/man-pages/tshark.html"></a><a href="http://www.snort.org/">http://www.snort.org/</a> <br>
<strong>u2boat</strong> <br>
<a href="http://www.snort.org/"></a><a href="http://www.wireshark.org/docs/man-pages/tshark.html">http://www.wireshark.org/docs/man-pages/tshark.html</a> <br>
<strong>u2spewfoo</strong> <br>
<a href="http://www.snort.org/"></a><a href="http://www.snort.org/">http://www.snort.org/</a> <br>
<strong>Wireshark</strong> <br>
<a href="http://www.wireshark.org/"></a><a href="http://www.wireshark.org/">http://www.wireshark.org/</a> <br>
<strong>Xplico</strong> <br>
<a href="http://www.xplico.org/"></a><a href="http://www.xplico.org/">http://www.xplico.org/</a></p>


<h1 id='kurulum'>Kurulum</h1>
---

<h3 id='sistemgereksinimleri'>Sistem Gereksinimleri</h3>

**32-bit ve 64-bit**
Paketlerde 32-bit ve 64-bit versiyonları mevcuttur. Ancak yüksek performanslı ve güvenlikli 64-bit versiyonu  önerilmektedir.

**UPS**
IT sistemler gibi; Security Onion'da da veritabanı vardır. Güç kesintisi durumuyla karşı karşıya kalmamak için UPS kullanabilirsiniz.
 http://www.apc.com/shop/us/en/products/APC-Back-UPS-550/P-BE550G

**ANA SUNUCU**
Dağıtılan dağıtımlarda yalnızca tek ana sunucu olmalıdır. Dinleme yapmayan ve log toplamaya devam eden bu ana sunucunun, ayrı bir sensor kutusunda oturum açması gerekir. 

**CPU**
Snort, Suricata ve Bro'da yoğun CPU kullanımı bulunmaktadır. Trafik izleme sırasında daha fazla CPU çekirdeğine ihtiyaç duyulur. Örneğin; Snort, Suricata ve Bro çalıştığında her birisi için 200 Mbps bağlantıya ihtiyaç duyulacaktır. 

**RAM**
Ram kullanımı yüksek değişikliklere bağımlıdır.
> * etkinleştirme hizmetleri
> * trafik izleme çeşitleri
> * trafik izleme miktarı (örneğin 1Gbps bağlantı izleme olabilir ama çoğu zaman 200Mbps kullanılır.)
> * "kabul edilebilir" paket kaybı miktarı

Aşağıdaki RAM tahminleri kaba bir tahmindir. Snort/Suricata, Bro ve netsniff-ng(tam paket yakalama) çalıştırılacak ve minimize paket kaybını ortadan kaldırmayı hedefleyelim.

> * Hızlı bir VM Security Onion istiyorsanız, gerekli RAM miktarı 3GB kadardır. Daha fazla RAM daha iyidir!
> * 50Mbps veya daha küçük bir ağ üzerinde Security Onion dağıtımı tercih ediyorsanız, 8GB RAM veya daha fazla planlanmalıdır.
> * 50Mbps-500Mbps gibi orta ağ üzerinde Securtiy Onion dağıtımı tercih ediyorsanız, planlamanızı 16GB-128GB RAM veya daha fazlası olmalıdır.
> * 500Mbps-1000Mbps gibi büyük bir ağ üzerinde Securtiy Onion dağıtımı tercih ediyorsanız, planlamanızı 128GB-256GB RAM olarak yapmalısınız.

**Depolama**
Tam paket yakalama (ve/veya ELSA) özelliğine sahip sensorler için çokça depolama alanı gerekir. Örneğin; ortalama 50 Mbps bir ağı izlediğinizi düşünelim. 50 Mb/s = 6.25 MB/saniye = 375 MB/dakika = 22.500 MB/saat = 540.000 MB/gün. Sonuç olarak, bir gün için 540 GB pcap verisi oluşmakta. Bu sadece pcapler için ve ELSA da ayrıca tüm diskin %50sine ihtiyaç duymakta. Yani bir gün için toplamda en az 1080 GB bir diske sahip olmanız gerekmekte ve gün sayısı arttıkça bu ihtiyaç da artacaktır.

**NIC**
Yönetimi ve dinleme için en az iki kablolu ağ arayüzü gerekir. Özellikle dineleme için kaliteli kart aldığınıza emin olun. Kullanıcıların çoğu Intel kartları ile iyi dinlemeler yapmakta.

**Paketler**
Sensor arayüzüne paketleri bir şekilde almamız gerekmektedir. Sadece Security Onion için değerlendirme yapacaksanız  .pcap dosyasından yararlanabilirsiniz.

* Sheer Simplicity and Portability (USB ile):
http://www.dual-comm.com/port-mirroring-LAN_switch.htm

* Dirt Cheap and Versatile:
http://www.roc-noc.com/mikrotik/routerboard/RB260GS.html

* Netgear GS105E (konfigüre etmek için Windows app gerekir):
https://www.netgear.com/support/product/GS105E.aspx

* Another flavor of low cost SPAN type tap:
http://www.midbittech.com

* More exhaustive list of enterprise switches with port mirroring:
http://www.miarec.com/knowledge/switches-port-mirroring

<h3 id='indirmeveyukleme'>İndirme ve Yükleme</h3>

Security Onion ISO imajı kullanılarak ya da Ubuntu 14.04 kurulumu üzerine Security Onion PPA ve paketleri eklenerek iki farklı kurulum yapmak mümkündür. Göz önünde bulundurulması gereken, paketlerin sadece Ubuntu 14.04 versiyonu ile uyumlu olduğudur. 

 **İndirilen ISO imajının checksum'ını her zaman doğrulayın!**
Security Onion'u hangi şekilde yüklediğinizden bağımsız olarak, indirdiğiniz imaj dosyasının checksum'ını her zaman doğrulamanız gerekir. 

* ISO imajı için : 
https://github.com/Security-Onion-Solutions/security-onion/blob/master/Verify_ISO.md

* Ubuntu 14.04 için: 
https://help.ubuntu.com/community/VerifyIsoHowto

**UEFI**
Eğer UEFI'a sahip yeni bir makineniz varsa, ilgili adrese göz atın:
https://help.ubuntu.com/community/UEFI

**Security Onion Kurulum Rehberleri**

* [Security Onion ISO imajını indirmek ve hızlıca kurmak için](https://github.com/Security-Onion-Solutions/security-onion/wiki/QuickISOImage)

* [Ubuntu 14.04 ile Security Onion kullanımı için](https://github.com/Security-Onion-Solutions/security-onion/wiki/InstallingOnUbuntu)

<h3 id='bootsorunu'>Boot Sorunu</h3>
**ISO imajı makinemde neden boot etmiyor?**

* İndirdiğiniz ISO imajını, [indirme ve yüklem e](#indirmeveyukleme)  bölümünde anlatıldığı gibi doğruladınız mı?

* Makineniz 64-bit destekli mi? (Eğer 64-bit VM çalıştırmayı deniyorsanız, 64-bit işlemciniz sanallaştırmayı desteklemeli ve bu özelliğin BIOS'ta enable edilmiş olması gerekmektedir). Eğer değilse; 64-bit makine edinebilirsiniz ya da 32-bit makinenizi Ubuntu 14.04 32-bit kurulumu üzerine Security Onion PPA ve paketlerini ekleyerek kullanmaya devam edebilirsiniz.

* Makinenizin 64-bit destekli olduğunu düşünüyor, fakat hala 64-bit ISO imaj dosyası ile aynı problemi yaşamaya devam ediyorsanız, bir de Ubuntu 14.04 64-bit imajını indirip kurmayı deneyin. Eğer çalışmazsa, 64-bit uyumluluğunuzu doğrulamanız gerekmektedir.

* Eğer ISO imajı boot edilebiliyor, ancak Live masaüstü düzgün çalışmıyorsa, ekran kartınız ile ilgili bir problem yaşanıyor olabilir. Boot menüsünde `nomodoset` seçeneğini deneyin: 
http://blog.securityonion.net/2014/02/security-onion-12044-iso-image-now.html
https://groups.google.com/d/topic/security-onion/UKE5-dqybQ4/discussion
https://groups.google.com/d/topic/security-onion/51JZWXZfBho/discussion

  Çalışıyor fakat bu sefer de düşük çözünürlük ile kalıyorsanız ve NVIDIA chipset'iniz varsa, KURULUM SONRASINDA aşağıdaki adımları takip edin:
  

	> sudo apt-get remove nvidia-173
	> sudo apt-get remove nvidia-96
	> sudo apt-get install nvidia-current


<h3 id='surumnotlari'>Sürüm Notları</h3>
* Her zaman olduğu gibi, indirdiğiniz imaj dosyalarını doğrulamayı unutmayın!
  https://github.com/Security-Onion-Solutions/security-onion/blob/master/Verify_ISO.md

* ISO boot menüsü artık Live Desktop opsiyonunu desteklememektedir. Bunun yerine `Install` seçeneği tercih edilmelidir (ya da 10 saniye bekleyerek otomatik olarak yükleme kısmına geçebilirsiniz).

* `Installation Type` ekranında, `Use LVM`'i seçin. Bu seçenek, ileride esnek bir biçimde de kullanabileceğiniz boot partitionu otomatik olarak oluşturacaktır.

* `Keyboard Layout` ekranı, ekran çözünürlüğünüzden daha büyük olabilir ve `Continue` butonunu göremiyor olabilirsiniz, görene kadar ekranı kaydırıp butona tıklayabilirsiniz.

* Kurulum bittiği zaman ekrana `remove installation media and press ENTER` uyarısı gelir, enter'a basıp yeniden başlatın. İşe yaramazsa, kendiniz yeniden başlatmanız gerekebilir.

* Yeni yüklenmiş Security Onion'ınıza ilk giriş yaptığınızda, masaüstünde sadece Setup iconu göreceksiniz. Çalıştırdığınızda Sguil/Squert/ELSA iconları oluşturulacaktır. 

* Quick Setup artık `Evaluation Mode`, Advanced Setup da `Production Mode` olarak isimlendirilmektedir.

* Evaluation Mode seçildiğinde, bu servisler default olarak etkinleştirilir: `Snort, Bro, netsniff-ng, pcap_agent, snort_agent, barnyard2, ELSA, Xplico`. Bu servisler de default olarak devre dışı bırakılır: `argus, prads, pads_agent, sancp_agent, http_agent`.

* ELSA paneli artık offline olarak da çalışabiliyor.


<h3 id='kurulumsonrasi'>Kurulum Sonrası</h3>
---

Servislerin durumunu kontrol etmek için:

     sudo service nsm status

Hiçbir servis çalışmıyorsa:

    sudo service nsm start

**Çeşitli Ayarlamalar**

* VLAN ağlarını izliyorsanız [VLAN](https://github.com/Security-Onion-Solutions/security-onion/wiki/VLAN-Traffic) sayfasına göz atmanızda fayda var.

* Eğer özel RFC1918 adres aralığından (192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12) başka bir IP adres aralığını izlemek istiyorsanız, sensorünüzün doğru IP adres aralığı konfigürasyonunu yapmanız gerekmektedir. Sensor konfigürasyon dosyalarına `/etc/nsm/$HOSTNAME-$INTERFACE/` yolundan erişilebilir (snort.conf ya da suricata.yaml). Ardından, `/opt/bro/etc/networks.cfg` dosyasından Bro ayarlarını da konfigüre etmelisiniz. İşlemler tamamlandığında ise: `sudo nsm_sensor_ps-restart` 

* Eğer internet erişiminiz varsa, terminalden IDS alert oluşturun. `curl http://testmyids.com` 

* Security Onion default olarak, güvenlik duvarında sadece tcp/22 portunu aktif eder. Eğer OSSEC agents, syslog cihazları ya da analist VM'ler gibi şeylere ihtiyaç duyuyorsanız, `so-allow` utilitysini kullanarak ekstra kurallar ekleyebilirsiniz.

* IDS alertlerinize göz atmak için Sguil tarafında login olun. Squert ve ELSA'ya `https://server` adresinden erişilebilir.

* IDS/NSM sistemler, izledikleri ağlar için ayarlanmaya ihtiyaç duyarlar. Daha fazlası için: [ManagingAlerts](https://github.com/Security-Onion-Solutions/security-onion/wiki/ManagingAlerts)

* Eventleri günlük olarak gözden geçirip kategorize etmeniz, veritabanı/Sguil issuelarında kategorize edilmemiş event sayısının günlük bazda çok fazla kabarmasını engelleyecektir.

* Sguil veritabanının çalıştığı serverda `DAYSTOKEEP` değerini `/etc/nsm/securityonion.conf` kısmından istediğiniz değere ayarlayın (default değer 30). 

* Eğer [http_agent](https://github.com/Security-Onion-Solutions/security-onion/wiki/http_agent) aktifse, `http_agent.conf` da ayarlanmalıdır. Eğer ELSA çalışıyorsa, halihazırda Bro tüm HTTP loglarını topluyor demektir, aynı logun iki kere üretilmesini istemiyorsanız http_agent'i devre dışı bırakmak isteyebilirsiniz. 

	> \# Terminate the running http_agent
sudo nsm_sensor_ps-stop --only-http-agent
\# Disable http_agent
sudo sed -i 's|HTTP_AGENT_ENABLED="yes"|HTTP_AGENT_ENABLED="no"|g' /etc/nsm//sensor.conf

* [İhtiyaç olmayan sensorleri devre dışı bırakın.](https://github.com/Security-Onion-Solutions/security-onion/wiki/DisablingProcesses)

* Snort/Suricata ve Bro için PF_RING instance sayılarını ayarlayın: [PF_RING](https://github.com/Security-Onion-Solutions/security-onion/wiki/PF_RING)

* İsteğe bağlı: İzlediğiniz ağdaki gereksiz trafiği görmek istemiyorsanız: [BPF](https://github.com/Security-Onion-Solutions/security-onion/wiki/BPF)

* İsteğe bağlı: Yeni Sguil kullanıcı hesapları eklemek için:
 `sudo nsm_server_user-add`

* İsteğe bağlı, fakat önerilir!: Alert ve reportlar için [Email](https://github.com/Security-Onion-Solutions/security-onion/wiki/email) konfigürasyonu yapın.

* İsteğe bağlı, fakat önerilir!: `/etc` dizinini versiyon kontrol sistemine ekleyin. Eğer çalıştığınız organizasyonun versiyon kontrol sistemi yoksa, [bazaar](https://help.ubuntu.com/12.04/serverguide/bazaar.html), [git](http://git-scm.com/) ya da [etckeeper](https://help.ubuntu.com/12.04/serverguide/etckeeper.html) kullanabilirsiniz.
 `sudo apt-get install git`

<h3 id='mimari'>Mimari</h3>
---

![so_diagram](https://cloud.githubusercontent.com/assets/1659467/13701945/2f04525c-e759-11e5-8d3d-7973a3f723b6.png)

> Snort/ Suricata, Bro, netsniff-ng kurallı saldırı tespitleri ile Ossec host tabanlı saldırı tespit sistemi kurulumda birlikte gelmektedir.
> 
*  Snort/ Suricata (Kural temelli NIDS) ; bilinen zararlı yazılımları ve şüpheli ağ trafik tespiti gibi durumlar için ağ trafiğini inceler. 
* Bro (Analiz temelli NIDS) ; genel dosya analizi, ağ trafiğini izleme, bağlantı loglama gibi işlemlerde kullanılır.
* netsniff-ng (full-packet-capture) ; Security Onion sensörleri tarafından farkedilen ve kaydedilen tüm ağ trafiğini yakalar.
* OSSEC; log analizi, dosya bütünlüğü kontrolü ve rootkit tespiti gibi işlemleri yapar.

Security Onion sensörü tarafından kaydedilen ağ trafiği; Snort/ Suricata, Bro, netsniff-ng  ile Ossec tarafından incelemeye alınarak ayrı ayrı üretilen log ve alert dosyaları, ana sunucuda yeralan ELSA veritabanına dökülerek arayüze yansıtılır.

<h3 id='arayuzler'>Arayüzler</h3>
---
<h4 id='squert'>**Squert**</h5>

*  Paul Holiday tarafında geliştirilmiştir.
 http://www.squertproject.org/

*  Squil veritabanına PHP web arayüzü

* Chromium/Chrome tarayıcıları ile en iyi performans

* Sguil kullanıcı veritabanını Squert tarafından karşılanır. Bu yüzden Sguil için kullandığınız kullanıcı adı ile şifreyi Squert' e giriş yaparken kullanmalısınız.

* Aşağıda belirtilen veri türlerine erişebiliyorsanız;
 
  *  NIDS alerts
  * HIDS alerts
  *  PRADS için assert veri (Eğer PRADS ve pads_agent etkin olursa)
  *  Bro için HTTP log (Eğer http_agent etkin olursa)

* Varsayılan görünüm gün içindeki uyarıları (alert) görüntüler. Eski uyarıları görüntülemek için; INTERVAL'ı tıklayın, ardından sağ oku 2 kez tıklayın, özel tarihinizi ayarlayın ve  Squert'i yenile butonuna tıklayın.
* TCP trafiği için tam paket yakalama (full-packet capture)  transkriptine dönüştürebilirsiniz. Gerçekleştirmek için Event ID 'ye tıklamalısınız.

* Bro günlüklerini (logs) sorgulamak için ELSA'yı kullanabilrisiniz. Bunu yapmak için;  bir IP adresi, port ya da imzaya tıklatın ve sonra ELSA'yı tıklayın. Security Onion 14.04 için,  ELSA'nın gerçek linkini kullanarak Squert'u çalıştırmalıyız. Bu yüzden Squert'e bağlanmak için aynı IP ve hostaname'i kullanmalıyız. Security Onion 12.04 kullanıyorsanız , ELSA'yı döndürmek için Squert'te kullandığımız IP adresini değiştirmeye ihtiyacınız vardır. "/usr/bin/sosetup" için aşağıda kopyalanmış kodu takip edebilirsin (Gerçek IP adres veya hostaname ile $IP yerine)

<pre><code># Pivot from Squert to ELSA
URL="https://$IP:3154/?query_string=\"\${var}\"%20groupby:program"
HEXVAL=$(xxd -pu -c 256 &lt;&lt;&lt; "$URL")
sudo mysql --defaults-file=/etc/mysql/debian.cnf -Dsecurityonion_db -e "INSERT IGNORE INTO filters (type,username,global,name,notes,alias,filter) VALUES ('url','','1','454C5341','','ELSA','$HEXVAL');"
</code></pre>

<h4 id='elsa'>**ELSA**</h5>

* Martin Holste tarafından geliştirilmiştir.
https://github.com/mcholste/elsa
https://github.com/mcholste/elsa/wiki/Documentation

* Log avı için web arayüzü (Bro, Snort, OSSEC, syslog)

* Chromium/Chrome tarayıcıları ile en iyi performans

* Diğer arayüzlerden daha fazla veri tipi

* Security Onion 14.04'de ELSA, dinamik grafik çubuğuna ve panoya sahiptir.

* ELSA web arayüzü , Sguil kullanıcı veritabanına karşı doğrular. Eğer Sguil / Squert giriş yapmak için kullanılan kullanıcı adı/parola kullanılır. Aynı kullanıcı adı/parola bilgisi ELSA için de kullanılır.

* Varsayılan olarak, ELSA günlükleri son 2 gün verilerini arar. From ve To fields kullanarak kontrol edebilirsiniz.

* Tam paket yakalama (full-packet capture) için CapME kullanılabilir. TCP trafiği ile ilgili günlüklerin timestamp; src ip; source port; destination ip; ve destination port bilgisine sahip olmalıdır. CapME çalıştırmak için Info, Plugin, getPcap'e tıklayın. Kullanıcı adınızı ve şifrenizi girin, CapMe pcap almak, bir ASCII transkript olarak işlemez . Eğer ELSA getPcap eklentisi vermezse.

* Çok hızlı ve ölçeklenebilir

* ELSA web arayüzünü sorguladığı zaman, paralel ELSA veritabanını sorgular ve toplu sonuç verir.

* Aşağıdaki komut satırı versiyonu kullanılabilir.(arama kriteriniz yerine example.com kullanabilirsiniz.)
 sh /opt/elsa/contrib/securityonion/contrib/cli.sh "example.com"

*  JSON çıktısında, "jg" çıktısını istiyorsak ;
sh /opt/elsa/contrib/securityonion/contrib/cli.sh "example.com" | jq '.'

* Perl processlerinin uzun numaraları için;
https://github.com/Security-Onion-Solutions/security-onion/wiki/FAQ#why-does-sostat-show-high-loadcpu-usage-and-large-number-of-perl-processes

* ELSA bellek sınırı sorunu için;
https://github.com/Security-Onion-Solutions/security-onion/wiki/FAQ#why-does-sostat-show-a-high-number-of-elsa-buffers-in-queue

* Daha fazla bilgi için;
 https://github.com/Security-Onion-Solutions/security-onion/wiki/ELSAQueryTips
 https://github.com/Security-Onion-Solutions/security-onion/wiki/CustomELSAParsers
* ELSA ayarlamaları için;
https://github.com/mcholste/elsa/wiki/Documentation#Lowvolumeconfigurationtuning

 https://groups.google.com/forum/#!searchin/enterprise-log-search-and-archive/%22allowed_temp_percent%22/enterprise-log-search-and-archive/auUSYj77ctw/mzF-YqVa5KMJ

 https://groups.google.com/d/topic/security-onion/xLxTGQs30ho/discussion

<h4 id='sguil'>**Sguil**</h5>

* Bamm Visscher tarafından geliştirilmiştir.
http://sguil.net
http://nsmwiki.org/Sguil
http://nsmwiki.org/Sguil_Client

* tcl/tk (web tabanlı değildir)

* Yalnızca MYSQL veritabanı merkezli

* Giriş bilgisi için;
https://github.com/Security-Onion-Solutions/security-onion/wiki/Passwords#sguil

* Veri Tipleri;
 * Snort/Suricata için NIDS uyarıları (Eğer snort_agent etkinse)
 * OSSEC için HIDS uyarıları (Eğer ossec_agent etkinse)
 * PRADS için otorum verileri (Eğer PRADS and sancp_agent etkinse)
 * PRADS için assent verileri (Eğer PRADS and pads_agent etkinse)
 * Bro için HTTP günlükleri (Eğer http_agent etkinse)

* Sütun başlığına sağ tıklayarak sütunları yeniden boyutlandırma

* Alert ID'ye sağa tıklayarak; transcript/Wireshark/NetworkMiner çalıştırılır

* Alert ID'ye orta tıklayarak; otomatik ASCII transkriptini çalıştırır.

* ELSA IP döngüsünü seçmek ve IP adresi için ELSA'yı sağ tıklayark çalıştırın

* Font boyutunu değiştirmek için Font-->Change Font

* Sguil istemci ayarları için takip edilen dizin : /etc/sguil/sguil.conf

* Kuralları, Paket verilerini ve ekran detayını öğrenmek için aşağıdaki adımları takip edin.(ayrıca bkz  https://groups.google.com/d/topic/security-onion/MJaAlxgpMvU/discussion)
<pre><code>set SHOWRULE 1
set PACKETINFO 1
set DISPLAY_GENERIC 1</code></pre>
* Gerçek zamanlı olarak ağırlık seviyelerine göre ayarlamak için /etc/sguil/sguil.conf dizininde ayarlama yapılmalıdır.
<pre><code> #Number of RealTime Event Panes    
 #set RTPANES 1    
  set RTPANES 3    
 # Specify which priority events go into what pane   
 # According to the latest classification.config from snort,   
 # there are only 4 priorities. The sguil spp_portscan mod   
 # uses a priority of 5.    
 #set RTPANE_PRIORITY(0) "1 2 3 4 5"  
  set RTPANE_PRIORITY(0) "1"  
  set RTPANE_PRIORITY(1) "2 3"  
  set RTPANE_PRIORITY(2) "4 5" </code></pre>  

<h4 id='capme'>**CapME**</h5>

CapME ile;

 * tcpflow ile gerçekleştirilen pcap transkript'ini görüntülemek 
 
 * Bro ile gerçekleştirilen pcap transkript'ini görüntülemek (özellikle gzip kodlaması için çok yararlı)
 
 * pcap indirmek
 
***CapME ile Derlenenler***

 Full transkriptler için ELSA  setupıyle gelen otomatik script konfigürasyon dosyasını CapME ile çalıştırır.
 
Tarihinde yayımlanan 6/6/2016  securityonion-squert-20141015-0ubuntu0securityonion15 ile Squert şimdi CapME ile çalışabilir. Detaylı bilgi için: 
 http://blog.securityonion.net/2016/06/new-capme-and-squert-packages-resolve.html
 
***CapME Doğrulaması***
 CapME sayfasında sorulan kullanıcı adı/ parola sorularına, Sguil/Squert/ELSA'da kullanılan normal kullanıcı adı/ parola kullanılır.
 
Tarihinde yayımlanan 6/6/2016 securityonion-capme 20121213-0ubuntu0securityonion59 ile CapME de herzaman kimlik doğrulama yapmak zorunda değilsiniz. Daha fazla bilgi için: 

http://blog.securityonion.net/2016/06/new-capme-and-squert-packages-resolve.html

<h4 id='snorby'>**Snorby**</h5>

* Security Onion 14.04 ile artık Security Onion'a katılmıştır. Eski sürüm Security Onion kullanıyorsanız (örneğin 12.04) en yeni versiyonu indirmelisiniz.
https://github.com/Security-Onion-Solutions/security-onion/wiki/Upgrading-from-12.04-to-14.04
https://github.com/Security-Onion-Solutions/security-onion/wiki/Installation

* Dustin Webber tarafından geliştirilmiştir.
https://github.com/snorby/snorby

* Web 2.0, Ajax, Ruby-on-Rails

* Sizin için özel Snorby setupıyle e-mail adresiniz ve şifrenizle oturum açabilirsiniz.

* Snorby'nin kendisine ait MYSQL veritabanı vardır.(Sguil ve ELSA veritabanlarından ayrı)

* Snorby veritabanı yalnızca Snor/Suricata ile gelen NIDS alertlerini (uyarıları) toplar.

* CapME 'nin tam paket yakalaması (full-packet capture) Snorby 'nin NIDS alertlerini (uyarılarını) çalışmasıdır.
https://github.com/Security-Onion-Solutions/security-onion/wiki/CapMeAuthentication

* Snorby IP adresi ELSA'nın ilgili günlükleri (logs) çalıştırılır.

* Eğer Snorby veritabanını silmek gerekiyorsa:
https://github.com/Security-Onion-Solutions/security-onion/wiki/WipingSnorby

* Nasıl Snorby veritabanını silebilirim?
https://github.com/Security-Onion-Solutions/security-onion/wiki/WipingSnorby

* Snorby highcards kullandığınıza emin olun. Eğer ticari bir lisans satın almanız gerekiyorsa <a href="https://shop.highsoft.com/">highcards</a> <a href="https://shop.highsoft.com/#what-is-non-commercial"> lisansını</a> okuyun.

* Snorby içinde saat dilimini nasıl değiştirebilirim?
 * Sağ üstteki Ayarlar'a tıklayın
 * Bir sonraki saat dilimi için açılan kutuyu tıklayın
 * Listeden zaman dilimini seçin
 *  Ayarları Güncelle butonuna tıklayın
 * CapMe'nin timezone.php ile aynı saat dilimini ayarlayın
 http://blog.securityonion.net/2014/01/new-capme-package-allows-you-to.html

* Snorby configürasyon yapılandırması için nasıl mail gönderebilirim?
Mail sunucusu için gereken, ana sunucu üzerinde Snorby en mail_config.rb dosyasını değiştirin :

<pre><code>/opt/snorby/config/initializers/mail_config.rb</pre></code>
Ardından Snorby delayed_job  işlemini restart edin.

<pre><code>sudo pkill -f delayed_job
sudo su www-data -c "cd /opt/snorby; bundle exec rake snorby:update RAILS_ENV=production"</pre></code>
Ayrıca /opt/snorby/config/snorby_config.yml dizininde yeralan modife edebilir ve Snorby sunucunun  FQDN veya IP adresini domain ayarlarında Production bölümünde değiştirebilirsiniz.Ancak, http yerine https olacağından e-posta ile sonuçlanan bağlantı hala yanlış olacaktır ve doğru port eksik olacaktır.

<h3 id='datakaynaklari'>Data Kaynakları</h3>
---

<h4 id='nids'>**NIDS**</h5>

* [Snort](http://snort.org)
  Snort, açık kaynak bir ağ temelli saldırı/sızma tespit sistemidir. Gerçek zamanlı trafik analizi ve paket loglama işlemlerini IP protokolleri üzerinde gerçekleştirir. Protokol analizi, içerik içerisinde arama ve eşleştirme yapar. Bu servisler, uygulama bazında servis kalitesini arttırmak veya gecikme konusunda hassas bazı uygulamalar için gereksiz yoğunluktaki ağ trafiğini ayıklamak için kullanılabilir. Snort aynı zamanda sızma/atak, işletim sistemi tespit etme, port-scan gibi işlemleri denetlemede de kullanılabilir.
  Snort, 3 farklı şekilde konfigüre edilebilir: sniffer, packet logger ve NID. Sniffer modunda, ağ paketlerini okur ve konsolda gösterir. Packet logger modunda, paketleri diske loglar. NID modunda ise, ağ trafiğini izler ve kullanıcı tarafından ayarlanmış kurallara göre bu trafiği analiz eder. 

* [Suricata](https://suricata-ids.org)
   Suricata, açık kaynak bir ağ temelli saldırı/sızma tespit sistemidir. Suricata'yı kullanmak için 3 temel sebep vardır:
   
   **Yüksek Ölçeklenebilirlik**
	Suricata, mutli threaded bir uygulamadır. Yani bir instance çalıştırıldığı anda, Suricata'nın kullanılması için konfigüre edilmiş tüm sensorler üzerindeki işlem yükünü dengeleyebilir. 
	
	**Protokol Tanıma**
	En yaygın protokoller, akış başladığı anda Suricata tarafından tanınır. Böylece kuralı yazandan port beklenmez, doğrudan protokole kural yazılmasına olanak sağlanmış olur.

   **Dosya Tanıma, MD5 Checksum, Dosya Çıkarma**
	Suricata, ağınızda gezerken gördüğü binlerce dosya çeşidini tanıyabilir. Sadece tanıma değil, isteğe bağlı durumlarda dosyayı meta verisi ile birlikte çıkarma gibi işlemler de yapılabilir, dosyanın MD5 checksum'ı çok kolay bir şekilde hesaplanabilir.
		
<h4 id='bro'>**Bro**</h5>




<h4 id='ossechids'>**OSSEC HIDS**</h5>


<h4 id='syslog'>**Syslog**</h5>
