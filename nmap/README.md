![NMAP](https://nmap.org/images/eyelogo.jpg)
#NMAP (NETWORK MAPPING)

> * <a href='#nmap'>NMAP</a> 
>  * <a href='#calisma'>NMAP ÇALIŞMA PRENSİBİ</a> 
>  * <a href='#sunucu/istemci'>SUNUCULARI/İSTEMCİLERİ KEŞFETME</a> 
>  * <a href='#hedef'>NMAP HEDEF TANIMLAMA</a> 
> * <a href='#ping'>TCP BAYRAKLARI İLE PINGG</a> 
>   * <a href='#icmp'>ICMP Echo Request ve TCP ACK PING</a> 
>   * <a href='#icmp echo'>ICMP Echo Request PING</a> 
>   * <a href='#tcpsyn'>TCP SYN PING</a> 
>   * <a href='#icmpaddress'>ICMP Address Mask PING</a> 
>   * <a href='#tcpack'>TCP ACK PING</a> 
>   * <a href='#dontping'>Don't PING BEFORE SCANNING</a> 
> * <a href='#porttarama'>PORT TARAMA</a> 
>   * <a href='#synscan'>TCP Syn Scan</a> 
>   * <a href='#connectscan'>TCP Connect Scan</a> 
>   * <a href='#fin'>FIN Scan</a> 
>   * <a href='#xmas'>XMAS Tree Scan</a>
>   * <a href='#ip'>IP Protocol Scan</a>
>   * <a href='#udp'>UDP Scan</a>
>   * <a href='#ip'>IP Protocol Scan</a>
>   * <a href='#ack'>ACK Scan</a>
>   * <a href='#window'>Window Scan</a>
>   * <a href='#idle'>Idle Scan</a>
>   * <a href='#version'>Version Detection</a>
>   * <a href='#rpc'>RPC Scan</a>
>   * <a href='#list'>List Scan</a>
>   * <a href='#firewall'>Firewall</a>
>   * <a href='#ftp'>FTP Bounce Scan</a>
> * <a href='#os'>OS İZİ BELİRLEME</a>
>   * <a href='#osizi'>Os İzi Belirleme Seçenekleri</a>
> * <a href='#gorunmez'>Görünmez Tarama Seçenekleri</a>
> * <a href='#nse'>NSE</a>
>   * <a href='#nseornek'>NSE ÖRNEKLERİ</a>
>   * <a href='#nsesecenek'>NSE Seçeneklerİ</a>
> * <a href='#guvenlik'>Güvenlik Ürünleri ve Nmap</a>
> * <a href='#fragmentation'>Fragmentation</a>
> * <a href='#spoofing'>Spoofing</a>
> * <a href='#packet'>Packet Manipulating</a>
> * <a href='#zenmap'>Zenmap</a>


<h1 id='nmap'>**NMAP Nedir?**</h2>

Syntax:
	nmap [tarama türü] [opsiyonlar] [hedef tanımlama] 

Nmap, bilgisayar ağları uzmanı Gordon Lyon (Fyodor) tarafından geliştirilmiş bir ağ tarayıcısıdır. Taranan ağın haritasını çıkarabilir ve ağ makinalarında çalışan servislerin durumlarını, işletim sistemlerinin, portların durumlarını gözlemleyebilir.
Nmap kullanarak;

  - ağa bağlı herhangi bir bilgisayarın işletim sistemi,
  - çalışan fiziksel aygıt tiplerini çalışma süresi,
  - yazılımların hangi servisleri kullandığı,
  - yazılımların sürüm numaralarını,
  - zaafiyet testleri, 
  - bilgisayarın güvenlik duvarına sahip olup olmadığını,
  - ağ kartının üreticisinin adı gibi bilgiler öğrenilebilmektedir.

Nmap tamamen özgür GPL lisanlı yazılımdır ve istendiği takdirde sitesinin ilgili bölümünden kaynak kodu indirilebilmektedir.
Nmap'i indirmek için <a href="https://nmap.org/download.html" > tıklayın </a>

<h2 id='calisma'>**NMAP ÇALIŞMA PRENSİBİ**</h3>
 1. Tarama işleminin gerçekleştirileceği hedef makinanın ismi girilirse,Nmap ilk önce kendisine ait olmayan DNS lookup işlemi gerçekleştirir. İsim ile tarama gerçekleştirdiğimiz zaman DNS sorgularının network trafiğinde yeraldığını ve loglandığını unutmamalıyız. Ancak taramak istediğimiz makinanın ismi yerine IP adresi girilirse DNS lookup işlemi gerçekleştirilmeyecektir.
 2. DNS Lookup işleminin iptal edilemez. Ancak Nmap tarafından tarama yapılan makinanın host (ya da Imhost) dosyalarının içerisinde Nmap taramaya başlamadan önce muhakkak hedef makinayı pingler.
 3. Ping işlemi ICMP Echo isteği gönderilip ICMP Echo cevabı döndürülerek gerçekleştirilir. Ancak Nmap bilinen ping işlemi yerine kendisine özgü olan ping işlemini gerçekleştirir. Öncelikle ICMP Echo isteği gönderir. Ardından 80. porta TCP ACK bayraklı paketi göndererek ping işlemini gerçekleştirir. Hedef makina cevap vermezse Nmap diğer hedefe geçer. Ancak başka hedef makina yoksa taramayı sonlandırır.(-P0 parametresi verilerek ping işlemi sonlandırılır.)
 4. Hedef makinanın IP adresi belirtildiği takdirde Nmap reverse DNS lookup yaparak IP-Hostname eşlemesi yapar. Bu durum 1. adımın tersidir. İlk adımda DNS Lookup işlemi yapıldığı için bu adım gereksiz gözükebilir. Ancak IP-Hostname sonuçları ile Hostname-IP sonuçları farklı çıkabilir.Bu yüzden dikkat edilmelidir.

Nmap taramayı bu dört adımla gerçekleştirir.

 Örnek bir nmap taraması:
> nmap -A -T4 bilgio.com

-A, OS ve versiyon bulma, script taraması ve traceroute özelliğini çalıştırır.

-T4, hızlı tarama yapılmasını sağlar.(T0 ve T5 arasında seçim yapılabilir)

<h1 id='hedef'>**NMAP HEDEF TANIMLAMA**</h3>


- -iL (input-file-name): Networklerin veya hostların belirtildiği dosyadan bilgi taraması yapar.

> nmap -sV -iL ip.txt yazarak belirtilen txt'deki adreslere test gerçekleştirir.

- -iR (num_host): Belirtilen host sayısı ile kaç hedefin taranılacağı belirtilir.

> nmap -p 80 -iR 10 yazarak HTTP servisini kullanan rastgele 10 hostu bulabiliriz.

- -exclude: Taranılmasını istemediğimiz network ya da hostların belirtilmesinde kullanılır.

> nmap -sP -exclude bilgio.com,nmap.org 192.168.1.0/24 yazarak 192.168.1.0-192.168.1.255 subnetinde belirtilen adresler dışındaki herşeyi tarayabiliriz.

- -excludefile (file):Taranılmasını istemediğimiz network veya hostların bir dosya içerisinden alınarak hedefler belirtilir. 

> nmap --excludefile istenmeyen.txt 192.168.0.0/16 yazarak 192.168.0.0-192.168.255.255 subnetinde belirtilen dosyadaki adresler dışında  herşey  taranır.
192.168.1-2*=192.168.1,2.0-255:192.168.1.0-192.168.2.255 arasındaki bütün adresleri tarar.

<h2 id='sunucu/istemci'>**SUNUCULARI/İSTEMCİLERİ KEŞFETME**</h3>

Keşfetme işlemini basit olarak ping scan ile gerçekleştirebiliriz.
> nmap -sP 192.168.1.0/24

Ping scan belirtilen hedefin 80. portuna ICMP Echo request ve TCP ACK(root veya administrator değilse SYN) paketleri gönderir. Hedeften dönen tepkilere göre bilgiler çıkartılır.Nmap ile hedef aynı yerel ağ içerisindeyse, Nmap hedef MAC adreslerini ve ilgili üreticiye ait bilgileri (OUI) sunar. Bunun sebebi, Nmap default olarak ARP taraması, -PR yapar. Bu özelliği iptal etmek için –-send-ip seçeneği kullanılabilir. Ping scan portları taramaz.Network envanteri vb. işlemleri için idealdir.

Nmap taramalarında hedef belirtilirken DNS ismi, LP, Subnet gibi seçenekler dışında neler kullanabileceğimize bakalım:

- -sP: Ping scan gerçekleştirmek için kullanılır.
- -sL: (list Scan) Hedefleri ve DNS isimlerinin listesini oluşturur.
- -sn: (Ping Scan) Port scan işlemini iptal eder.
- -Pn: Host discovery yapılamaz, bütün hostlar ayakta gözükür.
- -n/-R: Asla DNS çözümlemesi yapılmaz / Herzaman DNS çözümlemesi yapılır.
- -dns-serves : Özel DNS serverları belirtmek için kullanılr.
- --system-dns: OS'e ait DNS çözümleyici kullanır.
- --traceroute: Traceroute aktifleştirilir.TCP Connect ve Idle Scan dışındaki tarama türleri yapılamaz.
- -p: Port veya port aralıklarını belirtmek için kuallanılır.-p45;-p1-65536; gibi
- -F: (Fast mode) Varsayılan taramalarda belirlenen portlardan daha azı tarama için kulanılır.
- -r: Portları sırayla tarar.
- -top-ports sayi: sayi ile belirtilen ortak portlar taranır.
- -p (oran): Belirtilen <oran> üzerinden ortak portlar taranır.
- -rH: (randomize_hosts) Listede belirtilen taranılacak hostları rastgele seçer.
- -g:(source_port) Taramayı yapacak olan makinanın kaynak portunu belirlemek amacıyla kullanılır.
- -S Ip: Kaynak IP yi belirlemek amacıyla kullanılır.
- -e: Network arayüzünü belirlemek amacıyla kullanılır.

<h2 id='ping'>**TCP BAYRAKLARI İLE PING**</h3>


<h4 id='icmp'>**ICMP Echo Request ve TCP ACK PING<**</h4>
Aynı anda ICMP  Echo isteği ve TCP ACK bayraklı paket gönderilip TCP RST ve ICMP Echo Reply'in dönmesi beklenir.
> nmap -PB destination_IP
nmap -PB 192.168.1.2

<h4 id='icmp echo'>**ICMP Echo Request PING**</h4>
Hedef makinaya ICMP Echo isteği gönderilir ve cevap dönmesi beklenir.Cevap gelmediği takdirde Makine kapalıdır ve filtered olarak kabul edilir.
> nmap -PE destination_IP
nmap _PE 192.168.1.2

<h4 id='tcpack'>**TCP ACK PING**</h4>
Hedef makinaya TCP ACK bayraklı paket gönderilip gelen cevap da RST bayraklı paket olursa hedef makinanın açık olduğu anlaşılır.
> nmap -PA destination_IP
nmap -PA 192.168.1.2

<h4 id='tcpsyn'>TCP SYN PING</h4>
Hedef makinaya TCP SYN bayraklı paket gönderildiğinde Makine açıksa SYN+ACK bayraklı paket, kapalıysa RST bayraklı paket döndürülür.
> nmap -PS destination_IP
nmap -PS 192.168.1.2

<h4 id='icmpaddress'>ICMP Address Mask PING</h4>
Hedef makinaya ICMP Adrees Mask isteği gönderilir.Eğer ICMP Adress Mask cevabı dönerse Makine açık kabul edilir.Eğer Makine kapalıysa ping düşer ve işlem yapılamaz.
> nmap -PO destination_IP 
nmap -PM 192.168.1.2

<h4 id='dontping'>Don't PING BEFORE SCANNING</h4>
Nmap ping işlemi gerçekleştirmeden direkt olarak tramayı gerçekleştirir.Ancak reverse DNS sorgusu aktif olarak bekler.
> nmap -PO destination_IP
nmap -PO 192.168.1.2

<h2 id='porttarama'>PORT TARAMA</h3>
(-v ya da -vv seçeneği sayesinde daha düzenli bir çıktı sağlarız.) 
Gerçekleştirilen taramada portların durumu hakkında bilgi veren 6 durumumuz vardır.

- Open : Portlar açık ve aktif olarak TCP veya UDP bağlantısını kabul eder.
- Closed : Portlar kapalı ancak erişilebilir.Üzerlerinde dinlenilen aktif bir bağlantı yoktur.
- Filtered : Dönen tepkiler bir paket filtreleme mekanizması tarafından engellenir.Nmap portunun açık olduğuna karar veremez.
- Unfiltered : Portlar erişilebilir ancak Nmap portların açık veya kapalı olduğuna karar veremez.(Sadece ACK için)
- Open|filtered : Nmap portların açık veya filtrelenmiş olduğuna karar veremez.(UDP,IP,Proto,FIN,Null,Xmas Scan için)
- Closed|filtered : Nmap portların kapalı ya da filtreli olduğuna karar veremez.(Sadece Idle Scan için)

- -sS : Hedef sistemin portlarına SYN paketi yollanır. Eğer SYN/ACK paketi dönerse port açık demektir.
- -sT : Hedef sistemin portlarına SYN paketi yollanır.SYN/ACK paketi döndüğünde ACK paketi yollar ve porta bağlanır. Bu tarama daha yavaş ama daha güvenlidir.
- -sA : ACK paketleri kullanılarak firewall’ a takılmadan hedef sisteme ulaşabilmeyi sağlar.Rastgele üretilmiş ACK/Sequence paketleri hedef sisteme yollanır. ICMP Port Unreachabler mesajı gelirse yada cevap gelmezse port kapalı demektir.
- -sF : Hedef sisteme sanki açık bir TCP oturumu varmış da onu kapatıyormuşsunuz gibi sonlandır bayrağı olan FIN paketi gönderilir ve RST beklenir.

 
TCP bayraklarını manual olarak eklemek istendiğinde kullanılacak syntax:
> nmap --scanflags <TCP_Flag>[destination_IP]

<img src="http://www.networkuptime.com/nmap/images/scan_summary.gif">
   
<h4 id='synscan'>TCP Syn Scan(-sS)</h4>

Bu tarama türünde kaynak makina hedef makinaya TCP Syn bayraklı paket göndererek başlattığı tarama sırasında portların çoğu kapalı olucaktır. Portların kapalı olduğu durumlarda hedef Makine RST+ACK bayraklı segmenti döndürür.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/rst-ack-nmap.png">

Portların açık olduğu durumlarda ise hedef makinaya SYN+ACK bayraklı segment döndürür. Daha sonra kaynak Makine RST bayraklı segment göndererek bağlantıyı koparır ve böylece TCP three-way handsaking tamamlanmaz. Handsaking gerçekleşmediği için bu tarama türü hedef sistemlerinde hernhangi iz bırakmaz. Oldukça hızlıdır. Portların durumu için dönecek cevaplar:
Open, Closed, Filtered olabilir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/syn-ack-nmap.png">

> Kullanımı: 

> nmap -sS destination_ip

> nmap -sS 192.168.1.180

<h4 id='connectscan'>TCP Connect Scan(-sT)</h4>

Kaynak makinanın gerçekleştireceği TCP Connect Scan, kapalı portlara yapıldığı zaman RST+ACK bayraklı segment dönecektir.
Ancak açık portlara yapıldığında hedef makinanın göndereceği SYN+ACK bayraklı segmenti, kaynak makine ACK bayraklı segment göndererek cevaplar ve TCP three-way handsaking tamamlanır.

> Kullanımı:	

> nmap -sT -v destination_ip

> nmap -sT -v 192.168.1.180


<h4 id='fin'>FIN Scan(-sF)</h4>

FIN Scan ile ilişkili olan “saklı” frameler hedef makinaya ilk TCP handsaking olmadan gönderirler.
Hedef makinaya TCP bağlantı isteği olmadan gönderilen segmentle tarama yapılır. Kaynak makinanın göndereceği FIN bayraklı segment, hedef makinanın kapalı bir portuna gelirse hedef makina RST + ACK bayraklı segment döndürecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/fin-scan.png">

Eğer açık portuna gelirse hedef makinadan herhangi bir tepki dönmeyecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/fin-scan-port-acik.png"> 

> Kullanımı:

> namp -sF -v destination_ip

> nmap -sF -v 192.168.1.18

<h4 id='xmas'>XMAS Tree Scan(-sX)</h4>

Kaynak makinanın TCP frame içine URG, PSH ve FIN bayraklarını set edeceği paket hedef makinaya gönderilir. Hedef makinanın döndüreceği cevaplar FIN Scan ile aynıdır. 
Kaynak makinanın göndereceği URG,PSH ve FIN bayraklı paket, hedef makinanın kapalı bir portuna gelirse hedef makina RST + ACK bayraklı paket döndürecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-xmas.png">

Eğer port açık olursa hedef makinadan herhangi bir tepki dönmeyecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-xmas-urg-psh-fin.png">

> Kullanımı:

> nmap -sX -v destination_ip

> nmap -sX -v 192.168.1.180

<h4 id='null'>NULL Scan(-sN)</h4>

Hiçbir bayrağın bulunmayacağı bu tarama türü, gerçek hayatta karşımıza çıkmayan bir durumdur.Kaynak makinanın göndereceği bayraksız segmentler karşısında hedef makinanın vereceği tepkiler FIN Scan ile aynıdır.Kaynak makinanın göndereceği bayraksız segment, hedef makinanın kapalı bir portuna gelirse hedef makina RST + ACK bayraklı segment döndürecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-null-scan.png">

Eğer port açık olursa hedef makinadan herhangi bir tepki dönmeyecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-null-scan-2.png">

> Kullanımı:

> nmap -sN -v destination_ip

> nmap -sN -v 192.168.1.180

<h4 id='ping'>Ping Scan(-sP)</h4>

Bu tarama türünde kaynak makina hedef makinaya tek bir ICMP Echo istek paketi gönderir.IP adresi erişilebilir ve ICMP filtreleme bulunmadığı sürece, hedef makina ICMP Echo cevabı döndürecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ping-scan.png">

Eğer hedef makina erişilebilir değilse veya paket filtreleyici ICMP paketlerini filtreliyorsa, hedef makinadan herhangi bir cevap dönmeyecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ping-scan-2.png">


> Kullanımı:

> nmap -sP -v destination_ip

> nmap -sP -v 192.168.1.180

<h4 id='udp'>UDP Scan(-sU)</h4>

Kaynak makinanın hedef makinaya göndereceği UDP datagramına, ICMP Port Unreachable cevabı döndürülüyorsa hedef makina kapalı kabul edilecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-udp-scan.png">

Herhangi bir tepki döndürmeyen hedef makina open|filtered kabul edilecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-udp-scan-2.png">

UDP datagramıyla cevap döndüren hedef makinaya ait port ise açık kabul edilecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-udp-scan-3.png">

> Kullanımı:

> nmap -sU -v destination_ip

> nmap -sU -v 192.168.1.180

<h4 id='ip'>IP Protocol Scan(-s0)</h4>
Bu tarama türü standart NMAP tarama türlerinden biraz farklıdır.Bu tarama türünde hedef makinaların üzerlerinde çalışan IP tabanlı protokoller tespit edilmektedir.Bu yüzden bu tarama türüne tam anlamıyla bir port taraması demek mümkün değildir.Hedef makina üzerinde, taramasını yaptığımız IP protokolü aktif haldeyse hedef makinadan bu taramaya herhangi bir cevap gelmeyecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ip-protokol-1.png">					
					
Hedef makina üzerinde, taramasını yaptığımız IP protokolü aktif halde değilse hedef makinadan bu taramaya, tarama yapılan protokolün türüne göre değişebilen RST bayraklı (RST bayrağı "1" yapılmış) bir segment cevap olarak gelecektir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ip-protokol-2.png">
					   
> Kullanımı:

> nmap -sO -v destination_ip
> nmap -sO -v 192.168.1.180

<h4 id='ack'>ACK Scan(-sA)</h4>
Bu tarama türünde kaynak makina hedef makinaya TCP ACK bayraklı segment gönderir.Eğer hedef makina ICMP Destination Unreachable mesajını dönerse ya da hedef makinada bu taramaya karşılık herhangi bir tepki oluşmazsa port “filtered” olarak kabul edilir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ack-scan.png">

Eğer hedef makina RST bayraklı segment döndürürse port “unfiltered” kabul edilir.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ack-scan-2.png">
					    
> Kullanımı:

> nmap -sA -v destination_ip

>nmap -sA -v 192.168.1.180

<h4 id='window'>Window Scan(-sW)</h4>
Window Scan, ACK Scan türüne benzer ancak bir önemli farkı vardır. Window Scan portların açık olma durumlarını yani “open” durumlarını gösterebilir.Bu taramanın ismi TCP Windowing işleminden gelmektedir.Bazı TCP yığınları, RST bayraklı segmentlere cevap döndüreceği zaman, kendilerine özel window boyutları sağlarlar.Hedef makinaya ait kapalı bir porttan dönen RST segmentine ait window boyutu sıfırdır.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-windows-scan.png">

Hedef makinaya ait açık bir porttan dönen RST segmentine ait window boyutu sıfırdan farklı olur.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-window-scan-2.png">   

> Kullanımı:

> nmap -sW -v destination_ip

> nmap -sW -v 192.168.1.180

<h4 id='idle'>Idle Scan(-sl)</h4>

Bu tarama türü, kaynak makinanın hedef makinayı tarama esnasında aktif olarak rol almadığı bir türdür.Kaynak makina “zombi” olarak nitelendirilen makinalar üzerinden hedef makinayı tarayarak bilgi toplar.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-idl-scan.png">

> Kullanımı:

> nmap -sW -v destination_ip

> nmap -sW -v 192.168.1.180

<h4 id='version'>Version Detection(-sV)</h4>
Version Detection, bütün portların bilgilerini bulabilecek herhangi bir tarama türü ile beraber çalışır. Eğer herhangi bir tarama türü belirtilmezse yetkili kullanıcılar ( root, admin ) için TCP SYN, yetkisiz kullanıcılar için TCP Connect Scan çalıştırılır.
Eğer açık port bulunursa, Version Detection Scan hedef Makine üzerinde araştırma sürecini başlatır.Hedef makinanın uygulamalarıyla direkt olarak iletişime geçerek elde edebileceği kadar bilgiyi almaya çalışır.Başlangıçta varsayılan olarak TCP SYN Scan yapıldığı ve cevaplarının döndüğünü kabul edersek, 80. Port üzerinde çalışan HTTP hakkında bilgi toplayacak olan Version Detection Scangerçekleştireceği tarama işlemleri aşağıdaki gibidir:

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/Nmap-tarama-version.png">

> Kullanımı:

> nmap -sV -v destination_ip

> nmap -sV -v 198.168.1.2

<h4 id='rpc'>RPC Scan(-sR)</h4>
RPC Scan, hedef makina üzerinde koşan RPC uygulamalarını keşfeder. Başka bir tarama türü ile açık portlar keşfedildikten sonra,RPC Scan hedef makinanın açık portlarına RPC null göndererek, eğer çalışan bir RPC uygulaması varsa, RPC uygulamasını harekete geçirir.RPC Scan, Version Detection Scan işlemi esnasında otomatik olarak çalıştırılır.

<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-rpc-scan.png">

> Kullanımı:

> nmap -sR -v destination_ip

> nmap -sR -v 192.168.1.2

<h4 id='list'>List Scan(-sL)</h4>

Bu tarama türü gerçek bir tarama değildir.Sadece Nmapin sorun çözme ve test yetenekleriniaktif kılar.Taranacak olan aktif makinaların Iplerini sıralar.Eğer reverse DNS çözümlemesi iptal edilmişse List Scan network üzerinde tamamen sessizdir.Herhangi bir paket almaz veya göndermez.

> Kullanımı:

> nmap -sL -v destination_ip

> nmap -sL -v 192.168.1.2

<h4 id='firewall'>Firewall</h4>
Hedef portu olmayan bir açık bir port olmalıdır. Uygun port bulunamzsa komut port tarama sonuçlarından filtrelenmiş veya kapalı port bulmaya çalışacaktır .

> Kullanımı:

> nmap --script firewall-bypass 

> nmap --script firewall-bypass --script-args firewall-bypass.helper="ftp", firewall-bypass.targetport=22 

<h4 id='ftp'>FTP Bounce Scan</h4>
<h5>FTP Bounce Scan</h5>
FTP Bounce Scan, FTP Serverlarının pasif olarak çalışması ile gerçekleştirilir. Pasif moddaki FTPde, komut bağlantıları ile veriler tamamen ayrıdır.FTP Serverlar dışarıya veri bağlantıları kurduğu için FW ile uyumlu çalışması gerekir. Bunun dışında, herhangi bir kullanıcı bir veriyi tamamen farklı bir hedefe gönderebilir.
Nmapin taramayı gerçekleştirebilmesi için, aradaki adam olacak olan FTP Serverla bağlantı kurması gerekir.Bağlantı kurulduktan sonra Nmap verileri taranacak olan hedef IP ve porta yönlendirir.
Yönlendirme işleminden sonra FTP üzerinde taramayı gerçekleştirebilmek için öncelikle PORT komutu, daha sonra verileri aktarabilmek için LIST komutu çalıştırılır.Kapalı portta bağlantı sağlanamazken, açık portta sağlanır:

> nmap -b -v [user@ftpserver] [hedef_IP]

<h2 id='os'>OS İZİ BELİRLEME</h3>
 1. Ping ve scan işlemi nmap tarafından tarama gerçekleştirilir.
 2. Tarama sonucunda portları open, closed, filtered olarak sınıflandırır. Çünkü OS işleminde açık ve kapalı port bulunması gerekmektedir.
 3. Os izi belirlemeye geçilir ve TCP hand-shaking ile devam eder.
 4. Hand-shaking ile TCP uptime, TCP sequence ve IPID tahminleri gerçekleştirilerek gönderilen herhangi bir bayraklı paketlere verilen cevaplar, ttl değerleri ve bahsetmiş olduğumuz seçenekler sonucunda Nmap OS izi hakkında tahminde bulunucaktır.Bu durumu gerçekleştirmek için şu komutları kullanırız:

> nmap -O destination _IP

> nmap -O 192.168.1.45

> nmap -O -A 192.168.1.45 (verisyon keşif ve OS Analizi)

<h4 id='osizi'>Os İzi Belirleme Seçenekleri</h4>
- osscan-limit: Hedef makinanın OS izini belirlerken en az bir tane kapalı ve açık portu bulunması şartıyla kullanırlır.
- osscan-guess: Nmap'in tarama sonucunda algılayamadığı durumlar için agresif bir şekilde belirleme yapmak için kullanılır.
- max-retries: Hedef makinaya belirtilen sayı kadar OS izi belirleme denemesi yapılır.

<h2 id='gorunmez'>Görünmez Tarama Seçenekleri</h3>
NMAP ile ağ taraması gerçekleştirirken dikkat edilmesi gereken önemli bir nokta da taramanın gizliliğinin sağlanmak ve IPS/IDS gibi sistemler tarafından tespitini zorlaştırmaktır.Bunu nedenle görünmez olarak tarama gerçekleştirmek için aşağıdaki seçeneklerden faydalanılabilir:
- T(0-5):Tarama hızın belirtir.T5 en hızlı taramayı sağlarken, ağ üzerinde oldukça fazla ses çıkartır ve ağ cihazları tarafından yakalanma olasılığını arttırır. Bunun yanında, T0 seçeneği ile tarama yapılması durumunda işlem oldukça fazla zaman alabilmekte ve ağdaki cihazların bu taramayı tespiti uzun sürmektedir.Bunun yanında, T0, T1 ve T2 hızlarında paralel tarama gerçekleştirilemez. 
- max-hostgroup (BilgisayarAdedi):Bu seçenek ile eş zamanlı taranacak grup boyutu belirtilir. Aynı anda kaç bilgisayarın taranacağını belirtir. 
- max-parallelism (sayı):Aynı anda en fazla kaç işlemin beraber yapılabileceğini belirler.  
- host-timeout <Saniye>:Hedeften vazgeçme süresi, tarama işleminin belirtilen sistem için iptal edilme süresini belirtir.
- scan-delay <Saniye>:Sondalar arası gecikme (saniye) süresini belirtir. Gönderilen her bir paket arasında geçen süredir. 
- max-rate <PaketAdedi>:Saniyede gönderilecek en fazla paket sayısını belirtir.
- D <SahteDegerler> / (Decoy Scanning):Kaynak IP olarak belirtilen sahte değerler gösterilir.
- f:Paketlerin parçalanarak (fragmante edilerek) gönderileceği belirtilir.
- mtu <PaketBoyutu>: Parçalanan paketlerin boyutu (8’in katı olacak şekilde) ayarlanabilir.
- data-length=<VeriBoyutu>:Verinin içeriğine padding eklenerek, verinin uzunluğu ayarlanabilir.Böylece, güvenlik cihazlarının NMAP aracının oluşturduğu standart boyuttaki paketleri düşürmesinin önüne geçilmeye çalışılır.
- iR :Hedef ip adreslerinden rastgele olarak seçilmesini sağlayan parametredir.Özellikle IDS/IPS gibi sistemlerin tarama gerçekleştirildiğini tespit edememesi için kullanılabilen bir parametredir.Bu şekilde hedef ip adreslerine sıralı bir biçimde paket gönderilmeyerek taramaların tespit edilmesi zorlaştırılmaya çalışılmaktadır.

> nmap -sS -p 80, 21-25, 443 -T2 -n -Pn –open 192.168.2.0/24  gizlenmek üzere düzenlendiğinde

> nmap -sS -p 80 –open -n -Pn –max-hostgroup 1 –max-retries 0 –max-parallelism 10 –max-rate 2 –scan-delay 2 -D microsoft.com,google.com –data-length=1333 -f –mtu=24 10.0.0.0/24 şeklinde bir kullanım gerçekleştirilebilir.

<h2 id='nse'>NSE (Nmap scripting engine)</h3>
NSE (Nmap scripting engine), Nmap'in en güçlü ve kullanışlı özelliklerinden birisidir.NSE'yi; normal Nmap komutlarıyla yapılamayan ya da yapılması çok zor olan işlemlerin daha kolay bir şekilde yapılmasının sağlandığı bir betikler bütünü olarak tarif edebiliriz.Nmap ile birlikte birçok betik kütüphanesi hazır olarak gelmektedir.Fakat NSE aynı zamanda kullanıcıların ihtiyaç duydukları betikleri kendilerinin de yazabilmelerini ve bunları paylaşabilmelerini de sağlar.NSE'de yer alan betikler aynı anda paralel olarak da çalıştırılabilirler.

NSE kullanarak temel olarak yapılabilecekler aşağıda listelenmiştir:
1. Ağ keşifleri:Hedef etki alanlarının (domain) whois veri tabanı sorguları yapılabilir.Hedef sistemlerin SNMP sorguları yapılabilir ve mevcut NFS/SMB/RPC paylaşım ve servisleri listelenebilir. 
2. Karmaşık versiyon tespiti:Normal Nmap komutlarıyla hedef sistemlerin versiyonları belirlenebilmektedir.NSE ile hedef sistemlerin versiyonları çok daha ayrıntılı bir şekilde tespit edilebilmektedir.
3. Zafiyet (vulnerability) tespiti:Normal Nmap komutlarıyla hedef sistemlerin zafiyetleri tam anlamıyla tespit edilememektedir.NSE ile bu zafiyetler daha kolayca belirlenebilmektedir.Nmap ile hazır gelen birçok zafiyet tespit betiği bulunmaktadır.Fakat Nmap'in temel işlevinin bir zafiyet tarayıcısı olmadığının bilinmesi gerekir. 
4. Arka kapı (backdoor) tespiti:NSE bazı arka kapı programlarını da tespit edebilmektedir. 
5. Zafiyet sömürmesi (Vulnerability exploitation):NSE ile sadece hedef sistemlerin zafiyetleri tespit edilmekle kalmayıp bu zafiyetlerin bazıları kullanılarak hedef sistemlere sızılması da mümkün olmaktadır.Fakat Nmap'in temel amacı Metasploit gibi bir zafiyet sömürü programına dönüşmek değildir. Buna rağmen NSE ile hedef sistemlerdeki bazı zafiyetlerin sömürülmesi de mümkündür.         
NSE ile temel olarak bu işlemlerin yapılması hedeflenmekle beraber NSE'nin geliştirilmesine devam edilmektedir.

<h4 id='nseornek'>NSE ÖRNEKLERİ</h4>
NSE kullanmak için nmap komutunun sonuna "--script=example.nse" parametresi eklenir.                                           Aşağıdaki nmap komutuyla hedef sistemin başlık (banner) bilgisi alınabilmektedir: 

> nmap -sS 192.168.61.61 –script=banner.nse        

Aşağıdaki nmap komutuyla hedef sistemin http başlık (http header) bilgi alınabilmektedir: 
> nmap -sS 192.168.61.61 –script=http-header.nse            

- "nmap -sC" komutu kullanılarak NSE kütüphanesinde bulunan temel bazı betiklerin çalıştırılması sağlanır. Bu komut nmap—script=default" komutuyla aynı görevi yapmaktadır.Varsayılan olarak, Version Scanning ( -sV ) versiyon kategorisinde bulunan bütün NSE scriptlerini çalıştırır. 
- A özelliği ise, -sC ( güvenli veizinsiz giriş kategorileri ) seçeneğini çalıştırır.NSE scriptleri Lua script dilinde yazılır ve .nse uzantısına sahiptir ve Nmap ana dizinin altında “scripts” dizininde saklanırlar. Bununla birlikte “script.db” Nmap ana dizinin altında bulunur ve bütün scriptleri kategorileriyle ( Güvenli, Zorla Giriş, Zararlı Yazılım,Arka Kapı, Versiyon, Keşif, Zafiyet ) saklar.		
- NSE, scripti çalıştırmadan önce hedefteki makinanın, Nmap çıktılarına dayanarak, gerekli kriterleri karşılayıp karşılamadığını araştırır. Bu araştırmadan sonra scriptin çalışmasına kararverir.                                              NSE kullanmanın en çabuk yolu aşağıdaki gibidir :              

> nmap -sC 192.168.1.0/24    

Yukarıdaki seçenek vasıtasıyla NSE bütün Güvenli ve Zorla Giriş scriptlerinin çalıştıracaktır. Eğer daha özel bir scriptin çalıştırılması istenirse --script seçeneği kullanılarak istenilen bir kategoriyeait scriptler çalıştırılabilir :  

> nmap --script=vulnerability 192.168.1.34                      

Sadece tek bir script çalıştırılmak istenirse aşağıdaki seçenekkullanılmalıdır.
> nmap --script=promiscuous.nse 192.168.1.0/24           

Belirli bir dizinin altındaki scriptleri çalıştırmak istenirse aşağıdaki seçenek kullanılmalıdır :
> nmap --script=/my-scripts 192.168.1.0/24                    

Bütün scriptlerin çalışması istenirse aşağıdaki seçenek kullanılmalıdır 
> nmap --script=all 192.168.1.55

<h4 id='nsesecenek'>NSE Seçeneklerİ</h4>

- script-args:Varolan script değerlerinin yerine belirlenen yeni değerler atanır.
- script-trace : Scripte ait bütün iç ve dış iletişimin çıktısını gösterir.
- script-updatedb : Scriptlerin bulunduğu veritabanını günceller.           

<h2 id='guvenlik'>Güvenlik Ürünleri ve Nmap</h3>

Nmap, taranılacak olan hedeflerin önünde bulunan güvenlik ürünlerinin kısıtlaması nedeniyle, istenilen şekilde tam olarak çalışamayabilir. Günümüzdeki güvenlik ürünleri Nmap ve taramalarını rahatlıkla yakalayabiliyor. Ancak Nmap kendi bünyesinde bulunan bazı seçenekler vasıtasıyla bu güvenlik ürünlerini atlatabilir. Fragmantasyon, spoofing ve packet manipulating seçenekleri vasıtasıyla Nmap güvenlik ürünlerini atlatıp, taramalarını daha rahat bir şekilde gerçekleştirebilir.

<h2 id='fragmentation'>Fragmentation</h3>

Nmap, taranılacak olan hedeflerin önünde bulunan güvenlik ürünlerinin kısıtlaması nedeniyle, istenilen şekilde tam olarak çalışamayabilir. Günümüzdeki güvenlik ürünleri Nmap ve taramalarını rahatlıkla yakalayabiliyor. Ancak Nmap kendi bünyesinde bulunan bazı seçenekler vasıtasıyla bu güvenlik ürünlerini atlatabilir.Fragmantasyon, spoofing ve packet manipulating seçenekleri vasıtasıylaNmap güvenlik ürünlerini atlatıp, taramalarını daha rahat bir şekilde gerçekleştirebilir. FragmentationNmap ile fragmantasyon yapılmak istenirse,-f, -f –f veya --mtu seçenekleri kullanılmalıdır.
Eğer parçalanmak istenilen paketin maksimum boyutu, IP başlık bilgisinden sonra, 8 byte olması isteniyorsa aşağıdaki komut kullanılmalıdır :
> nmap –f [Hedef_IP]:Eğer parçalanmak istenilen paketin maksimum boyutu, IP başlık bilgisinden sonra, 16 byte olması 

isteniyorsa aşağıdaki komut kullanılmalıdır : 						
> nmap –f –f [Hedef_IP]								             			       

Eğer parçalanmak istenilen paketin maksimum boyutu, IP başlık bilgisinden sonra, el ile girilerek belirlenmek isteniyorsa aşağıdaki komut kullanılmalıdır 												
> nmap --mtu <Sayı> [Hedef_IP]							        

<h2 id='spoofing'>Spoofing</h3>
Fragmantasyon seçeneğinin güvenlik ürünleri tarafından yüksek oranla yakalanması yüzünden diğer bir atlatma türü olan spoofing tercih edilebilir. Nmap Decoy Scan ( -D ), tercih edilen Nmap taramasının bir makinadan değil, belirtilecek olan makinalardan da yapılıyormuş gibi göstererek yakalanma riskini düşürür. Belirtilecek olan makinaların IP leri taramanın yapılacağı ortamla uyumlu olması çok önemlidir. Private IP kullanılan LAN ortamın Reel IP ile tarama yapılması pek akıllıca olmayacaktır. Eğer IP ler belirtilmezse Nmap rastgele olarak IP ler seçecektir. Ancak bu IP lerin Reel IP olma olasılığı var ve yukarıda bahsedilen durumun aynısı oluşabilir. Spoofing işleminin yapılması için kullanılması gereken komut aşağıdaki gibidir :  

> nmap -D < [Spooflanan_IP] > [Hedef_IP]          

Eğer geleneksel spoof yöntemi kullanılmak istenirse aşağıdaki komut kullanılmalıdır. Ancak geleneksel yöntemle gönderilen paketlerin cevapları taramanın yapıldığı makinaya geri dönmeyecektir. Aynı zamanda bu yöntemi Nmapin ethernet kart arayüzünün IP adresini bulamadığı durumlarda –e parametresi ile beraber kullanarak IP adresi atanabilir. Buradaki –e parametresi interface ismini belirtir. 

> nmap –S <[Spooflanan_IP]> [Hedef_IP] 	

> nmap –S <[Spooflanan_IP]>e [interface] [Hedef_IP]   		

Diğer bir spoofing yöntemi ise MAC adresleri. Nmap paketlerinin içerisinde farklı MAC adresleri bulunması sağlanabilir. Bütün bir MAC adresi girilebileceği gibi bir vendor ismi veya vendor prefixi de girilebilir. Eğer () şeklinde yazılırsa Nmap MAC adresini kendisi belirler. MAC spoofing için aşağıdaki komutlar kullanılmalıdır :

> nmap --spoof-mac 11:22:33:44:55:66 192.168.1.0/24 

> nmap --spoof-mac 000D93 192.168.1.0/24              

> nmap --spoof-mac D-Link 192.168.1.0/24  

Son olarak kaynak port için spoofing kullanılabilir. Bu işlem için aşağıdaki komut kullanılmalıdır : 

> nmap -g 53192.168.1.0/2	                                     

> nmap --source-port 53 192.168.1.0/24

<h2 id='packet'>Packet Manipulating</h3>
Nmap çok fazla sayıda packet manipulating özelliği barındırır.Bunları güvenlik ürünlerini atlatmak için kullanır.
- data-length <sayı> : Paket boyutunun olacağı uzunluğu <sayı> belirtir.
- ip-options yada --ip-options: Paketler içerisindeki IP özelliklerini belirtir.
- ttl <değer> : --randomize-hosts : Listede belirtilen taranılacak hostları rastgele bir şekilde seçer.
- badsum : Yanlış checksuma sahip TCP veya UDP paketleri gönderir

<h2 id='zenmap'>Zenmap</h3>
Nmap'in grafiksel arayüzüne taşınmış halidir.
Windows işletim sisteminde;

<img src="https://nmap.org/zenmap/images/zenmap-no-648x700.png"  width="400" height="400" >

Linux işletim sisteminde consol üzerinden Nmap taramaları gerçekleştirilir.

<img src="https://nmap.org/images/nmap-401-demoscan-798x774.gif" width="400" height="400" >
