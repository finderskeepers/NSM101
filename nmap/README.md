

NMAP (NETWORK MAPPING)

Syntax:
	nmap [tarama türü] [opsiyonlar] [hedef tanımlama] 

Nmap, bilgisayar ağları uzmanı Gordon Lyon (Fyodor) tarafından geliştirilmiş bir güvenlik tarayıcısıdır. Taranan ağın haritasını çıkarabilir ve ağ makinalarında çalışan servislerin durumlarını, işletim sistemlerinin, portların durumlarını gözlemleyebilir.
Nmap kullanarak;

  - -ağa bağlı herhangi bir bilgisayarın işletim sistemi,
  - -çalışan fiziksel aygıt tiplerini çalışma süresi,
  -  -yazılımların hangi servisleri kullandığı,
  - -yazılımların sürüm numaralarını,
  - -zaafiyet testleri, 
  - -bilgisayarın güvenlik duvarına sahip olup olmadığını,
  - -ağ kartının üreticisinin adı gibi bilgiler öğrenilebilmektedir.

Nmap tamamen özgür GPL lisanlı yazılımdır ve istendiği takdirde sitesinin ilgili bölümünden kaynak kodu indirilebilmektedir.
Nmap kurulumu için aşağıdaki <a href="https://nmap.org/download.html> linkten</a> faydalanabilirsiniz.
<h3>NMAP ÇALIŞMA PRENSİBİ</h3>
<ol>
<li><p>Tarama işleminin gerçekleştirileceği hedef makinanın ismi girilirse,Nmap ilk önce kendisine ait olmayan DNS lookup işlemi gerçekleştirir.İsim ile tarama gerçekleştirdiğimiz zaman DNS sorgularının network trafiğinde yeraldığını ve loglandığını unutmamalıyız.Ancak taramak istediğimiz makinanın ismi yerine IP adresi girilirse DNS lookup işlemi gerçekleştirilmeyecektir.
DNS Lookup işleminin iptal edilemez.Ancak Nmap tarafından tarama yapılan makinanın host (ya da Imhost) dosyalarının içerisinde IP-DNS eşleşmesi mevcut ise DNS Lookup işlemi atlanır.</p></li>
<li><p>Nmap taramaya başlamadan önce muhakkak hedef makinayı pingler.Ping işlemi ICMP Echo isteği gönderilip ICMP Echo cevabı döndürülerek gerçekleştirilir.Ancak Nmap bilinen ping işlemi yerine kendisine özgü olan ping işlemini gerçekleştirir.Öncelikle ICMP Echo isteği gönderir. Ardından 80. porta TCP ACK bayraklı paketi göndererek ping işlemini gerçekleştirir.Hedef makina cevap vermezse Nmap diğer hedefe geçer. Ancak başka hedef makina yoksa taramayı sonlandırır.
-P0 parametresi verilerek ping işlemi sonlandırılır.</p></li>
<li><p>Hedef makinanın IP adresi belirtildiği takdirde Nmap reverse DNS lookup yaparak IP-Hostname eşlemesi yapar.Bu durum 1. adımın tersidir.İlk adımda DNS Lookup işlemi yapıldığı için bu adım gereksiz gözükebilir. Ancak IP-Hostname sonuçları ile Hostname-IP sonuçları farklı çıkabilir.Bu yüzden dikkat edilmelidir.</p></li>
<li><p>Nmap taramayı bu dört adımla gerçekleştirir.</p></li>
</ol>

Örnek bir nmap taraması:
nmap -A -T4 bilgio.com

- -A, OS ve versiyon bulma, script taraması ve traceroute özelliğini çalıştırır.
- -T4, hızlı tarama yapılmasını sağlar.(T0ve T5 arasında seçim yapılabilir)

<h3>NMAP HEDEF TANIMLAMA</h3>
<ul>
<li>-iL <input_file_name>:Networklerin veya hostların belirtildiği dosyadan bilgi  tarama yapar.</li>
nmap -sV -iL ip.txt yazarak belirtilen txt'deki adreslere test gerçekleştirir.
<li>-iR<num_host>:Belirtilen host sayısı ile kaç hedefin taranılacağı belirtilir.</li>
nmap -p 80 -iR 10 yazarak HTTP servisini kullanan rastgele 10 hostu bulabiliriz.
<li>-exclude:Taranılmasını istemediğimiz network ya da hostların belirtilmesinde kullanılır.</li>
nmap -sP -exclude bilgio.com,nmap.org 192.168.1.0/24 yazarak 192.168.1.0-192.168.1.255 subnetinde belirtilen adresler dışındaki herşeyi tarayabiliriz.
<li>-excludefile <file>:Taranılmasını istemediğimiz network veya hostların bir dosya içerisinden alınarak hedefler belirtilir.</li>  
<p>nmap --excludefile istenmeyen.txt 192.168.0.0/16 yazarak 192.168.0.0-192.168.255.255 subnetinde belirtilen dosyadaki adresler dışında  herşey  taranır.
192.168.1-2*=192.168.1,2.0-255:192.168.1.0-192.168.2.255 arasındaki bütün adresleri tarar.</p>

<h3>SUNUCULARI/İSTEMCİLERİ KEŞFETME</h3>

Keşfetme işlemini basit olarak ping scan ile gerçekleştirebiliriz.
nmap -sP 192.168.1.0/24

Ping scan belirtilen hedefin 80. portuna ICMP Echo request ve TCP ACK(root veya administrator değilse SYN) paketleri gönderir.Hedeften dönen tepkilere göre bilgiler çıkartılır.Nmap ile hedef aynı yerel ağ içerisindeyse, Nmap hedef MAC adreslerini ve ilgili üreticiye ait bilgileri (OUI) sunar.Bunun sebebi, Nmap default olarak ARP taraması, -PR yapar.Bu özelliği iptal etmek için –-send-ip seçeneği kullanılabilir.Ping scan portları taramaz.Network envanteri vb. işlemleri için idealdir.

Nmap taramalarında hedef belirtilirken DNS ismi,LP,Subnet gibi seçenekler dışında neler kullanabileceğimize bakalım:

- -sP:Ping scan gerçekleştirmek için kullanılır.
- -sL:List Scan-Hedefleri ve DNS isimlerinin listesini oluşturur.
- -sn:Ping Scan-Port scan işlemini iptal eder.
- -Pn:Host discovery yapılamaz, bütün hostlar ayakta gözükür.
- -n/-R:Asla DNS çözümlemesi yapılmaz / Herzaman DNS çözümlemesi yapılır.
- -dns-serves : Özel DNS serverları belirtmek için kullanılr.
- --system-dns:OS'e ait DNS çözümleyici kullanır.
- --traceroute:Traceroute aktifleştirilir.TCP Connect ve Idle Scan dışındaki tarama türleri yapılamaz.
- -p:Port veya port aralıklarını belirtmek için kuallanılır.-p45;-p1-65536; gibi
- -F: Fast mode, varsayılan taramalarda belirlenen portlardan daha azı tarama için kulanılır.
- -r:Portları sırayla tarar.
- -top-ports sayi:sayi ile belirtilen ortak portlar taranır.
- p—port-ratio<oran>:Belirtilen <oran> üzerinden ortak portlar taranır.
- --randomize_hosts, -rH:Listede belirtilen taranılacak hostları rastgele seçer.
- --source_port, -g: Taramayı yapacak olan makinanın kaynak portunu belirlemek amacıyla kullanılır.
- -S ip:Kaynak IP yi belirlemek amacıyla kullanılır.
- -e:Network arayüzünü belirlemek amacıyla kullanılır.

<h3>TCP BAYRAKLARI İLE PING</h3>

<h5>ICMP Echo Request ve TCP ACK PING</h5> <p>Aynı anda ICMP  Echo isteği ve TCP ACK bayraklı paket gönderilip TCP RST ve ICMP Echo Reply'in dönmesi beklenir.
<br>nmap -PB destination_IP</br>
<br>nmap -PB 192.168.1.2</br>

<h5>ICMP Echo Request PING</h5> Hedef makinaya ICMP Echo isteği gönderilir ve cevap dönmesi beklenir.Cevap gelmediği takdirde Makine kapalıdır ve filtered olarak kabul edilir.
<br>nmap -PE destination_IP</br>
<br>nmap _PE 192.168.1.2</br>


<h5>TCP ACK PING</h5>Hedef makinaya TCP ACK bayraklı paket gönderilip gelen cevap da RST bayraklı paket olursa hedef makinanın açık olduğu anlaşılır.
<br>nmap -PA destination_IP</br>
<br>nmap -PA 192.168.1.2</br>

<h5>TCP SYN PING</h5>Hedef makinaya TCP SYN bayraklı paket gönderildiğinde Makine açıksa SYN+ACK bayraklı paket, kapalıysa RST bayraklı paket döndürülür.
<br>nmap -PS destination_IP</br>
<br>nmap -PS 192.168.1.2</br>

<h5>ICMP Address Mask PING</h5> Hedef makinaya ICMP Adrees Mask isteği gönderilir.Eğer ICMP Adress Mask cevabı dönerse Makine açık kabul edilir.Eğer Makine kapalıysa ping düşer ve işlem yapılamaz.
<br>nmap -PM 192.168.1.2</br>

<h5>Don't PING BEFORE SCANNING</h5> Nmap ping işlemi gerçekleştirmeden direkt olarak tramayı gerçekleştirir.Ancak reverse DNS sorgusu aktif olarak bekler.
<br>nmap -PO destination_IP</br>
<br>nmap -PO 192.168.1.2</br>

<h3>PORT TARAMA</h3>
<p>(-v ya da -vv seçeneği sayesinde daha düzenli bir çıktı sağlarız.) 
Gerçekleştirilen taramada portların durumu hakkında bilgi veren 6 durumumuz vardır.
<ol>
<li>
  Open:Portlar açık ve aktif olarak TCP veya UDP bağlantısını kabul
        eder. </li>
  <li>Closed:Portlar kapalı ancak erişilebilir.Üzerlerinde dinlenilen aktif bir bağlantı yoktur.</li>
 <li>Filtered:Dönen tepkiler bir paket filtreleme mekanizması tarafından
    engellenir.Nmap portunun açık olduğuna karar veremez.</li>
<li>Unfiltered:Portlar erişilebilir ancak Nmap portların açık veya
     kapalı olduğuna karar veremez.(Sadece ACK için)</li>
<li> Open|filtered:Nmap portların açık veya filtrelenmiş olduğuna karar
        veremez.(UDP,IP,Proto,FIN,Null,Xmas Scan için)</li>
<li>Closed|filtered:Nmap portların kapalı ya da filtreli olduğuna karar
        veremez.(Sadece Idle Scan için)</li>
</p>
- -sS :Hedef sistemin portlarına SYN paketi yollanır. Eğer SYN/ACK paketi dönerse port açık demektir.
- -sT :Hedef sistemin portlarına SYN paketi yollanır.SYN/ACK paketi döndüğünde ACK paketi yollar ve porta bağlanır. Bu tarama daha yavaş ama daha güvenlidir.
- -sA : ACK paketleri kullanılarak firewall’ a takılmadan hedef sisteme ulaşabilmeyi sağlar.Rastgele üretilmiş ACK/Sequence paketleri hedef sisteme yollanır. ICMP Port Unreachabler mesajı gelirse yada cevap gelmezse port kapalı demektir.
- -sF : Hedef sisteme sanki açık bir TCP oturumu varmış da onu kapatıyormuşsunuz gibi sonlandır bayrağı olan FIN paketi gönderilir ve RST beklenir.

Var olan tarama türleri Şekil 1.1 de belirtilmiştir. TCP bayraklarını manual olarak eklemek istendiğinde kullanılacak syntax:
nmap - -scanflags <TCP_Flaga>[destination_IP]

   <img src="http://www.networkuptime.com/nmap/images/scan_summary.gif">
<h5>TCP Syn Scan(-sS)</h5>
<p>Bu tarama türünde kaynak makina hedef makinaya TCP Syn bayraklı paket göndererek başlattığı tarama sırasında portların çoğu kapalı olucaktır. Portların kapalı olduğu durumlarda hedef Makine RST+ACK bayraklı segmenti döndürür.(Şekil 1.2)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/rst-ack-nmap.png">
<p>Portların açık olduğu durumlarda ise hedef makinaya SYN+ACK bayraklı segment döndürür. Daha sonra kaynak Makine RST bayraklı segment göndererek bağlantıyı koparır ve böylece TCP three-way handsaking tamamlanmaz. Handsaking gerçekleşmediği için bu tarama türü hedef sistemlerinde hernhangi iz bırakmaz.(Şekil 1.3) Oldukça hızlıdır. Portların durumu için dönecek cevaplar:
Open, Closed, Filtered olabilir.</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/syn-ack-nmap.png">
>Kullanımı:
nmap -sS destination_ip
nmap -sS 192.168.1.180

<h5>TCP Connect Scan(-sT)</h5>
<p>Kaynak makinanın gerçekleştireceği TCP Connect Scan, kapalı portlara yapıldığı zaman RST+ACK bayraklı segment dönecektir.(şekil 1.4)</p>	
Ancak açık portlara yapıldığında hedef makinanın göndereceği SYN+ACK bayraklı segmenti, kaynak Makine ACK bayraklı segment göndererek cevaplar ve TCP three-way handsaking tamamlanır(şekil 1.5).
>Kullanımı:				
map -sT -v destination_ip
nmap -sT -v 192.168.1.180
<h5>FIN Scan(-sF)</h5>
<p>FIN Scan ile ilişkili olan “saklı” frameler hedef makinaya ilk TCP handsaking olmadan gönderirler.
Hedef makinaya TCP bağlantı isteği olmadan gönderilen segmentle tarama yapılır. Kaynak makinanın göndereceği FIN bayraklı segment, hedef makinanın kapalı bir portuna gelirse hedef makina RST + ACK bayraklı segment döndürecektir.(Şekil 1.6)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/fin-scan.png">
<p>Eğer açık portuna gelirse hedef makinadan herhangi bir tepki dönmeyecektir.(Şekil 1.7)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/fin-scan-port-acik.png"> 
>Kullanımı:
namp -sF -v destination_ip
nmap -sF -v 192.168.1.18
<h5>XMAS Tree Scan(-sX)</h5>
<p>Kaynak makinanın TCP frame içine URG, PSH ve FIN bayraklarını set edeceği paket hedef makinaya gönderilir. Hedef makinanın döndüreceği cevaplar FIN Scan ile aynıdır. 
Kaynak makinanın göndereceği URG,PSH ve FIN bayraklı paket, hedef makinanın kapalı bir portuna gelirse hedef makina RST + ACK bayraklı paket döndürecektir.(Şekil 1.8)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-xmas.png">
Eğer port açık olursa hedef makinadan herhangi bir tepki dönmeyecektir.(Şekil 1.9)
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-xmas-urg-psh-fin.png">
>Kullanımı:
nmap -sX -v destination_ip
nmap -sX -v 192.168.1.180
<h5>NULL Scan(-sN)</h5>
<p>Hiçbir bayrağın bulunmayacağı bu tarama türü, gerçek hayatta karşımıza çıkmayan bir durumdur.Kaynak makinanın göndereceği bayraksız segmentler karşısında hedef makinanın vereceği tepkiler FIN Scan ile aynıdır.Kaynak makinanın göndereceği bayraksız segment, hedef makinanın kapalı bir portuna gelirse hedef makina RST + ACK bayraklı segment döndürecektir.(Şekil 1.10) </p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-null-scan.png">
Eğer port açık olursa hedef makinadan herhangi bir tepki dönmeyecektir.(Şekil 1.11)
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-null-scan-2.png">
>Kullanımı:
nmap -sN -v destination_ip
nmap -sN -v 192.168.1.180
<h5>Ping Scan(-sP)</h5>
<p>Bu tarama türünde kaynak makina hedef makinaya tek bir ICMP Echo istek paketi gönderir.IP adresi erişilebilir ve ICMP filtreleme bulunmadığı sürece, hedef makina ICMP Echo cevabı döndürecektir.(Şekil 1.12)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ping-scan.png">
<p>Eğer hedef makina erişilebilir değilse veya paket filtreleyici ICMP paketlerini filtreliyorsa, hedef makinadan herhangi bir cevap dönmeyecektir.(Şekil 1.13)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ping-scan-2.png">
>Kullanımı:
nmap -sP -v destination_ip
nmap -sP -v 192.168.1.180
<h5>UDP Scan(-sU)</h5>
<p>Kaynak makinanın hedef makinaya göndereceği UDP datagramına, ICMP Port Unreachable cevabı döndürülüyorsa hedef makina kapalı kabul edilecektir.(Şekil 1.14)</p> 
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-udp-scan.png">
<p>Herhangi bir tepki döndürmeyen hedef makina open|filtered kabul edilecektir.(Şekil 1.15)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-udp-scan-2.png">
<p>UDP datagramıyla cevap döndüren hedef makinaya ait port ise açık kabul edilecektir.(image 1.16)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-udp-scan-3.png">
				
>Kullanımı:
nmap -sU -v destination_ip
nmap -sU -v 192.168.1.180
<h5>IP Protocol Scan(-s0)</h5>
<p>Bu tarama türü standart NMAP tarama türlerinden biraz farklıdır.Bu tarama türünde hedef makinaların üzerlerinde çalışan IP tabanlı protokoller tespit edilmektedir.Bu yüzden bu tarama türüne tam anlamıyla bir port taraması demek mümkün değildir.Hedef makina üzerinde, taramasını yaptığımız IP protokolü aktif haldeyse hedef makinadan bu taramaya herhangi bir cevap gelmeyecektir.(Şekil 1.17)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ip-protokol-1.png">					
					
<p>Hedef makina üzerinde, taramasını yaptığımız IP protokolü aktif halde değilse hedef makinadan bu taramaya, tarama yapılan protokolün türüne göre değişebilen RST bayraklı (RST bayrağı "1" yapılmış) bir segment cevap olarak gelecektir.(Şekil 1.18)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ip-protokol-2.png">
					   
>Kullanımı:
nmap -sO -v destination_ip
nmap -sO -v 192.168.1.180
<h5>ACK Scan(-sA)</h5>
<p>Bu tarama türünde kaynak makina hedef makinaya TCP ACK bayraklı segment gönderir.Eğer hedef makina ICMP Destination Unreachable mesajını dönerse ya da hedef makinada bu taramaya karşılık herhangi bir tepki oluşmazsa port “filtered” olarak kabul edilir.(Şekil 1.19)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ack-scan.png">
<p>Eğer hedef makina RST bayraklı segment döndürürse port “unfiltered” kabul edilir.(Şekil 1.20)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-ack-scan-2.png">
					    
>Kullanımı:
<br>nmap -sA -v destination_ip</br>
nmap -sA -v 192.168.1.180
<h5>Window Scan(-sW)</h5>
<p>Window Scan, ACK Scan türüne benzer ancak bir önemli farkı vardır. Window Scan portların açık olma durumlarını yani “open” durumlarını gösterebilir.Bu taramanın ismi TCP Windowing işleminden gelmektedir.Bazı TCP yığınları, RST bayraklı segmentlere cevap döndüreceği zaman, kendilerine özel window boyutları sağlarlar.Hedef makinaya ait kapalı bir porttan dönen RST segmentine ait window boyutu sıfırdır.(Şekil 1.21)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-windows-scan.png">
<p>Hedef makinaya ait açık bir porttan dönen RST segmentine ait window boyutu sıfırdan farklı olur.(Şekil 1.22)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-window-scan-2.png">   			
>Kullanımı:
nmap -sW -v destination_ip
nmap -sW -v 192.168.1.180
<h5>Idle Scan(-sl)</h5>
<p>Bu tarama türü, kaynak makinanın hedef makinayı tarama esnasında aktif olarak rol almadığı bir türdür.Kaynak makina “zombi” olarak nitelendirilen makinalar üzerinden hedef makinayı tarayarak bilgi toplar.(Şekil 1.23)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-idl-scan.png">	
>Kullanımı:
nmap -sW -v destination_ip
nmap -sW -v 192.168.1.180

<h5>Version Detection(-sV)</h5>
<p>Version Detection, bütün portların bilgilerini bulabilecek herhangi bir tarama türü ile beraber çalışır. Eğer herhangi bir tarama türü belirtilmezse yetkili kullanıcılar ( root, admin ) için TCP SYN, yetkisiz kullanıcılar için TCP Connect Scan çalıştırılır.
Eğer açık port bulunursa, Version Detection Scan hedef Makine üzerinde araştırma sürecini başlatır.Hedef makinanın uygulamalarıyla direkt olarak iletişime geçerek elde edebileceği kadar bilgiyi almaya çalışır.Başlangıçta varsayılan olarak TCP SYN Scan yapıldığı ve cevaplarının döndüğünü kabul edersek, 80. Port üzerinde çalışan HTTP hakkında bilgi toplayacak olan Version Detection Scangerçekleştireceği tarama işlemleri aşağıdaki gibidir :(Şekil1.24)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/Nmap-tarama-version.png">
>Kullanımı:
nmap -sV -v destination_ip
nmap -sV -v 198.168.1.2
<h5>RPC Scan(-sR)</h5>
<p>RPC Scan, hedef makina üzerinde koşan RPC uygulamalarını keşfeder. Başka bir tarama türü ile açık portlar keşfedildikten sonra,RPC Scan hedef makinanın açık portlarına RPC null göndererek, eğer çalışan bir RPC uygulaması varsa, RPC uygulamasını harekete geçirir.RPC Scan, Version Detection Scan işlemi esnasında otomatik olarak çalıştırılır.(Şekil 1.26)</p>
<img src="http://www.teakolik.com/wp-content/uploads/2010/11/nmap-rpc-scan.png">
>Kullanımı:
nmap -sR -v destination_ip
nmap -sR -v 192.168.1.2
<h5>List Scan(-sL)</h5>
<p>Bu tarama türü gerçek bir tarama değildir.Sadece Nmapin sorun çözme ve test yetenekleriniaktif kılar.Taranacak olan aktif makinaların Iplerini sıralar.Eğer reverse DNS çözümlemesi iptal edilmişse List Scan network üzerinde tamamen sessizdir.Herhangi bir paket almaz veya göndermez.</p>
>Kullanımı:
nmap -sL -v destination_ip
nmap -sL -v 192.168.1.2
<h5>FTP Bounce Scan</h5>
<p>FTP Bounce Scan, FTP Serverlarının pasif olarak çalışması ile gerçekleştirilir. Pasif moddaki FTPde, komut bağlantıları ile veriler tamamen ayrıdır.FTP Serverlar dışarıya veri bağlantıları kurduğu için FW ile uyumlu çalışması gerekir. Bunun dışında, herhangi bir kullanıcı bir veriyi tamamen farklı bir hedefe gönderebilir.
Nmapin taramayı gerçekleştirebilmesi için, aradaki adam olacak olan FTP Serverla bağlantı kurması gerekir.Bağlantı kurulduktan sonra Nmap verileri taranacak olan hedef IP ve porta yönlendirir.
Yönlendirme işleminden sonra FTP üzerinde taramayı gerçekleştirebilmek için öncelikle PORT komutu, daha sonra verileri aktarabilmek için LIST komutu çalıştırılır.Kapalı portta bağlantı sağlanamazken, açık portta sağlanır :</p>

>nmap -b -v [user@ftpserver] [hedef_IP]

<h4>OS İZİ BELİRLEME</h4>

 1. Ping ve scan işlemi nmap tarafından tarama gerçekleştirilir.
 2.  Tarama sonucunda portları open, closed, filtered olarak sınıflandırır. Çünkü OS işleminde açık ve kapalı port bulunması gerekmektedir.
 3. Os izi belirlemeye geçilir ve TCP hand-shaking ile devam eder.
 4. Hand-shaking ile TCP uptime, TCP sequence ve IPID tahminleri gerçekleştirilerek gönderilen herhangi bir bayraklı paketlere verilen cevaplar, ttl değerleri ve bahsetmiş olduğumuz seçenekler sonucunda Nmap OS izi hakkında tahminde bulunucaktır.Bu durumu gerçekleştirmek için şu komutları kullanırız:
>nmap -O destination _IP     
nmap -O 192.168.1.45
nmap -O -A 192.168.1.45 (verisyon keşif ve OS Analizi)

<h4>Os İzi Belirleme Seçenekleri</h4>
–osscan-limit: Hedef makinanın OS izini belirlerken en az bir tane kapalı ve açık portu bulunması şartıyla kullanırlır.
<br>–osscan-guess: Nmap'in tarama sonucunda algılayamadığı durumlar için agresif bir şekilde belirleme yapmak için kullanılır.</br>
<br>–max-retries: Hedef makinaya belirtilen sayı kadar OS izi belirleme denemesi yapılır.</br> 


