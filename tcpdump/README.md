***TCPDUMP*** 
Tcpdump Unix benzeri işletim sistemlerinde komut satırından çalışan genel bir paket analizcisidir. Bilgisayara gelen veri paketlerini kaydetmeye, incelemeye, filtremeleye yardımcı bir sistemdir. Kullanıcıya bağlı bulunduğu bir ağ üzerinden iletilen veya alınan TCP/IP paketlerini veya diğer paketleri yakalama ve gözlemleme olanağı sunar. BSD linansı altında dağıtılan tcpdump ücretsiz bir yazılımdır. 
Tcpdump paket yakalamak için lilpcap kütüphanesini kullanır.
Tcpdump, paket yakalama işlemini bitirdikten sonra, yakalanan paket sayısı ile ilgili bir rapor sunar. 
Tcpdump kullanılarak yapılacak paket yakala işleminde eğer bir filtreleme uygulanacak ise bu durumda Berkeley Packet Filter (BPF) ifadelerini kullanırız. Tcpdump, Windump, Wireshark ve gibi bir çok sniffer uygulaması BPD ifadelerini kullanarak filtre tanımı yapılmasına izin verir.

***TCPDUMP TARİHİ***
İlk olarak 1987 yılında Van Jacobson, Craig Leres ve Steven McCanne tarafından Lawrence Berkeley Laboratory Research Group da çalışırlarken yazıldı. 1990'ların sonuna doğru çeşitli işletim sistemlerinin bir parçası olarak çok sayıda tcpdump versiyonu ve iyi düzenlenmemiş birçok yaması dağıtıldı. Michael Richardson ve Bill Fenner 1999 yılında tcpdump.org oluşturuldu. 

Tcpdump ilk olarak Unix sistemler için yazılmış olsada daha sonra Windows'a port edilmiş ve *Windump* olarak adlandırılmıştır.

Genel tanımı ve tarihi bilgilerden sonra programlama kısmını da incelemeye başlayalım 

***TCPDUMP YÜKLEME***

Bazı linux dağıtımlarda kurulmuş olan tcpdump programını komut satırına aşağıdaki komut yardımıyla bilgisayarımıza kurabiliriz.
**apt-get install tcpdump**
 Windows için ise http://www.winpcap.org adresindeki winpcap kütüphanesi indirilmeli ve sonrasında http://www.winpcap.org/windump adresinden Windump indirilerek yüklenebilir.

Linux/Unix sistemlerde *tcpdump* komutunu kullanabilmek için ya root haklarına sahip olmak lazım ya da tcpdump programının suid olarak çalışması lazım.
 

***ALDIĞI PARAMETRELER***
***tcpdump -A ==>*** Yakalanan paketlerin içeriğini ASCII formatında ekrana basar.
![enter image description here](http://www.teknikblog.org/wp-content/uploads/2015/08/3.png)

***tcpdump -c ==>*** Belirtilen sayıda paket yakalaması yapar
![enter image description here](http://www.teknikblog.org/wp-content/uploads/2015/08/2.png)
-i parametresi ile arayüz belirledikten sonra -c parametresi ile kaç adet paket yakalanacağını belirliyoruz.

***tcpdump -C ==>*** Paketleri belirli boyutlarda kayıt etmek istenildiğinde kullanılır.
	

    tcpdump i wlan0 -w kayit.pcap -C 100 // Paketleri 100 Mb'lık dosyalar halinde kayıt ederek tarama işlemine devam eder.

***tcpdump -D ==>*** Sistemde kullanılabilir olan ağ arayüzlerinin listesini verir.

    tcpdump -D

***tcpdump -e ==>*** Paketlerde bulunan Mac adreslerini elde etmek için kullanılır.
***tcpdump -E ==>*** Bir şifreleme anahtarı tarafından IPSEC trafiği şifrelenir.
***tcpdump -i ==>*** Belirtilen ağ arayüzü üzerinden dinleme yapar.
***tcpdump -n ==>*** Dns isimleri çözümlemesi yapılmaz.

***tcpdump -nn ==>*** Yakalanan paketlerde dns, port, ve protokol çözümlemesi yapılmaz.
***tcpdump -p ==>*** Promisc moddan kaçmak için kullanılır.
***tcpdump -r ==>*** Pcap olarak kayıt edişmiş dosyayı okur.

    tcpdump -r pcapadi.pcap

***tcpdump -s ==>*** Her bir paketin ilk ne kadarlık kısmının yakalanacağını belirtir. Varsayılan olarak bu değer 68 byte'dır.
***tcpdump -t ==>*** Yakalanan paketlerin zaman çıktısı olmadan görüntüler.
***tcpdump -v ==>*** Paketler hakkında daha fazla bilgi verir
***tcpdump -vv ==>*** Paketin NFS ve SMB içeriğini de gösteren detaylı bir analiz yapar.
***tcpdump -vvv ==>*** Paketlerin Telnet içeriğini de gösterir.
***tcpdump -w ==>*** Yakalanan paketleri dosyaya yaz.
***tcpdump -x ==>*** Çıktıyı HEX olarak yazdırır.
***tcpdump -XX ==>*** Çıktıyı HEX ve ASCII olarak yazdırır.
![enter image description here](http://www.teknikblog.org/wp-content/uploads/2015/08/Terminal-root@daca-home-caner_002.png)
***Örnek komutlar*** 

    tcpdump -i wlan0 udp
Wlan üzerinden UDP paketlerini yakalamada kullanılır.

    tcpdump -i wlan0 src 192.168.1.1 and dst 192.168.1.3
Ağ üzerindeki iki makine arasında TCP protokolü kullanarak yapılan paketleri yakalar.

    tcpdump -i wlan0 port80
  Wlan üzerinde 80 nolu porttan geçen paketleri yakalar.

    tcpdump -i wlan0 dst 192.168.1.2 and port80
Wlan üzerinde hedef ip adresi 192.168.1.2 ve 80 nolu porta gelen paketleri yakalar.

    tcpdump -i wlan0 -v -nn -w yakalanpaketler.pcap
Wlan0' dan geçen paketleri dns ve ip çözümlemesi yapmadan detaylı bir şekilde yakalananpaketler.pcap dosyası olarak kayıt eder.











***Kaynaklar : ***
 https://tr.wikipedia.org/wiki/Tcpdump
http://www.teknikblog.org/?p=410
