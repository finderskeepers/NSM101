	===========================================
	===========================================
	=====TSHARK İLE NETWORK ANALİZİNE GİRİŞ====
	===========================================
	=======DÜZENLEYEN: Efekan TUZCUOĞLU========
	===========================================
	===========================================

	Ön Not: Kayıtlı dosyalar üzerinde çalışacaksanız uygulamalar sorunsuz çalışır, ancak trafiği dinleyecekseniz uygulamalara root izni
	sağlamalısınız. (örn: sudo tshark -pni agArayuzu -w dosya.pcap)

KISACA TSHARK PARAMETRELERİ:
       tshark [ -2 ] [ -a <capture autostop condition> ] ...  [ -b <capture ring buffer option>] ...  [ -B <capture buffer size> ]
       [ -c <capture packet count> ] [ -C <configuration profile> ] [ -d <layer type>==<selector>,<decode-as protocol> ] [ -D ]
       [ -e <field> ] [ -E <field print option> ] [ -f <capture filter> ] [ -F <file format> ] [ -g ] [ -h ] [ -H <input hosts file> ]
       [ -i <capture interface>|- ] [ -I ] [ -K <keytab> ] [ -l ] [ -L ] [ -n ] [ -N <name resolving flags> ]
       [ -o <preference setting> ] ...  [ -O <protocols> ] [ -p ] [ -P ] [ -q ] [ -Q ] [ -r <infile> ] [ -R <Read filter> ]
       [ -s <capture snaplen> ] [ -S <separator> ] [ -t a|ad|adoy|d|dd|e|r|u|ud|udoy ] [ -T fields|pdml|ps|psml|text ] [ -u <seconds type>]
       [ -v ] [ -V ] [ -w <outfile>|- ] [ -W <file format option>] [ -x ] [ -X <eXtension option>] [ -y <capture link type> ]
       [ -Y <displaY filter> ] [ -z <statistics> ] [ --capture-comment <comment> ] [ <capture filter> ]

       tshark -G [ <report type> ]

AÇIKLAMA
	TShark ağ analiz yazılımıdır. Çalışan bir ağ üzerindeki paketleri görüntüleyebilir ve kaydedebilirsiniz, veya önceden kaydedilmiş
	paketler üzerinde çalışabilirsiniz.

	TShark topladığınız paketleri default olarak pcap formatında kaydeder. Pcapten bahsedecek olursak:

	==================================
	===PCAP (Packet Capture) NEDİR?===
	==================================
	  Network yöneticilerinin hayatlarını kolaylaştırmak amacıyla oluşturulmuş bir dosya formatıdır diyebiliriz, ağ trafiğini dinleyerek
	yakaladığımız paketleri daha incelenebilir bir yapıda tutmamıza olanak sağlar.
	  Pcap dosya formatını kullanabilmemiz için Unix/Linux sistemlerde "libpcap" kütüphanesi mevcuttur. Windows sistemlerde ise libpcap
	kütüphanesinin Windows'a uyarlanılmış hali olan "WinPcap" kütüphanesi mevcut.
	(Farklı programlama dilleri için libpcap kütüphanesinin uyarlamaları var.)
	Bu kütüphanelerde çeşitli fonksiyonlar bulunmakta, ancak kütüphanelerin kullanımı daha çok development'a girdiği için bu yazıda
	anlatmayacağım, kullanacağımız araçların bu kütüphanelerden faydalanarak yazıldığını bilmemiz yeterli, merak eden ya da daha fazla
	öğrenmek isteyenler terminale "man pcap" yazarak daha çok bilgiye erişebilirler.
	==================================

	Eğer herhangi bir parametre vermezsek tshark ilk uygun gördüğü ağ arayüzü üzerinden paketleri yakalamaya ve görüntülemeye başlar.
	Ancak yakalanan paketler üzerinde çalışacağımız için kullanacağımız komut: sudo tshark -pni ağArayüzü -w dosyaAdı.pcap
	(Parametrelerin kullanım amaçları yazının devamında)
	***NOT: Okunacak/Yazılacak dosyanın uzantısının bir önemi yok, tshark otomatik olarak pcap formatını tespit eder. Eğer paketler
	kaydedilirken gzip sıkıştırma yapıldıysa tshark bunu da tespit eder ve otomatik olarak arşiv içerisindeki pcap dosyasını okur.
	(*zlib kütüphanesi gereklidir)

       When writing packets to a file, TShark, by default, writes the file in pcap format, and writes all of the packets it sees to the
       output file.  The -F option can be used to specify the format in which to write the file.  This list of available file formats is
       displayed by the -F flag without a value.  However, you can't specify a file format for a live capture.

       Read filters in TShark, which allow you to select which packets are to be decoded or written to a file, are very powerful; more
       fields are filterable in TShark than in other protocol analyzers, and the syntax you can use to create your filters is richer.  As
       TShark progresses, expect more and more protocol fields to be allowed in read filters.

       Packet capturing is performed with the pcap library.  The capture filter syntax follows the rules of the pcap library.  This syntax
       is different from the read filter syntax.  A read filter can also be specified when capturing, and only packets that pass the read
       filter will be displayed or saved to the output file; note, however, that capture filters are much more efficient than read filters,
       and it may be more difficult for TShark to keep up with a busy network if a read filter is specified for a live capture.

       A capture or read filter can either be specified with the -f or -R option, respectively, in which case the entire filter expression
       must be specified as a single argument (which means that if it contains spaces, it must be quoted), or can be specified with
       command-line arguments after the option arguments, in which case all the arguments after the filter arguments are treated as a
       filter expression.  Capture filters are supported only when doing a live capture; read filters are supported when doing a live
       capture and when reading a capture file, but require TShark to do more work when filtering, so you might be more likely to lose
       packets under heavy load if you're using a read filter.  If the filter is specified with command-line arguments after the option
       arguments, it's a capture filter if a capture is being done (i.e., if no -r option was specified) and a read filter if a capture
       file is being read (i.e., if a -r option was specified).


PARAMETRELER VE AÇIKLAMALARI
       -2  Eğer ki "-R" parametresi ile çok koşullu bir filtre uygulayacaksanız bu parametreyi de kullanmak zorundasınız. Bu parametre ile
	   filtreleme işlemleri için bir buffer alanı oluşturulur.
	   Örnek: tshark -2 -r dosya.pcap -R "not dns && ip.addr eq 1.2.3.4"
	   (DNS paketleri haricinde, içerisinde 1.2.3.4 ip adresini bulunduran paketleri listeler)

       -a  <capture autostop condition>
           Specify a criterion that specifies when TShark is to stop writing to a capture file.  The criterion is of the form test:value,
           where test is one of:

           duration:value Stop writing to a capture file after value seconds have elapsed.

           filesize:value Stop writing to a capture file after it reaches a size of value kB.  If this option is used together with the -b
           option, TShark will stop writing to the current capture file and switch to the next one if filesize is reached.  When reading a
           capture file, TShark will stop reading the file after the number of bytes read exceeds this number (the complete packet  will be
           read, so more bytes than this number may be read).  Note that the filesize is limited to a maximum value of 2 GiB.

           files:value Stop writing to capture files after value number of files were written.

       -b  <capture ring buffer option>
           Cause TShark to run in "multiple files" mode.  In "multiple files" mode, TShark will write to several capture files.  When the
           first capture file fills up, TShark will switch writing to the next file and so on.

           The created filenames are based on the filename given with the -w option, the number of the file and on the creation date and
           time, e.g. outfile_00001_20050604120117.pcap, outfile_00002_20050604120523.pcap, ...

           With the files option it's also possible to form a "ring buffer".  This will fill up new files until the number of files
           specified, at which point TShark will discard the data in the first file and start writing to that file and so on.  If the files
           option is not set, new files filled up until one of the capture stop conditions match (or until the disk is full).

           The criterion is of the form key:value, where key is one of:

           duration:value switch to the next file after value seconds have elapsed, even if the current file is not completely filled up.

           filesize:value switch to the next file after it reaches a size of value kB.  Note that the filesize is limited to a maximum
           value of 2 GiB.

           files:value begin again with the first file after value number of files were written (form a ring buffer).  This value must be
           less than 100000.  Caution should be used when using large numbers of files: some filesystems do not handle many files in a
           single directory well.  The files criterion requires either duration or filesize to be specified to control when to go to the
           next file.  It should be noted that each -b parameter takes exactly one criterion; to specify two criterion, each must be
           preceded by the -b option.

           Example: -b filesize:1000 -b files:5 results in a ring buffer of five files of size one megabyte each.

       -B  <capture buffer size>
           Set capture buffer size (in MiB, default is 2 MiB).  This is used by the capture driver to buffer packet data until that data
           can be written to disk.  If you encounter packet drops while capturing, try to increase this size.  Note that, while Tshark
           attempts to set the buffer size to 2 MiB by default, and can be told to set it to a larger value, the system or interface on
           which you're capturing might silently limit the capture buffer size to a lower value or raise it to a higher value.

           This is available on UNIX systems with libpcap 1.0.0 or later and on Windows.  It is not available on UNIX systems with earlier
           versions of libpcap.

           This option can occur multiple times.  If used before the first occurrence of the -i option, it sets the default capture buffer
           size.  If used after an -i option, it sets the capture buffer size for the interface specified by the last -i option occurring
           before this option.  If the capture buffer size is not set specifically, the default capture buffer size is used instead.

       -c  <capture packet count>
           Görüntülemek/Yakalamak istediğiniz azami paket sayısı. Belirtilen sayıya ulaşıldığında tshark durur.
	   Örnek: tshark -i 1 -c 5  (Bulunan ilk ağ arayüzünden 5 adet paket toplanılır ve durur)
	   Örnek: tshark -r dosya -c 5  (Dosya içerisindeki ilk 5 paket görüntülenir ve durur)

       -C  <configuration profile>
           TShark belirtilen konfigürasyon profili ile çalıştırılır.

       -d  <layer type>==<selector>,<decode-as protocol>
           Like Wireshark's Decode As... feature, this lets you specify how a layer type should be dissected.  If the layer type in
           question (for example, tcp.port or udp.port for a TCP or UDP port number) has the specified selector value, packets should be
           dissected as the specified protocol.

           Example: -d tcp.port==8888,http will decode any traffic running over TCP port 8888 as HTTP.

           Example: -d tcp.port==8888:3,http will decode any traffic running over TCP ports 8888, 8889 or 8890 as HTTP.

           Example: -d tcp.port==8888-8890,http will decode any traffic running over TCP ports 8888, 8889 or 8890 as HTTP.

           Using an invalid selector or protocol will print out a list of valid selectors and protocol names, respectively.

           Example: -d . is a quick way to get a list of valid selectors.

           Example: -d ethertype==0x0800. is a quick way to get a list of protocols that can be selected with an ethertype.

       -D  TShark'ın capture edebileceği ağ arayüzlerinin listesini yazdırır ve durur. "-i" parametresine bu listedeki arayüzleri isim ya da rakam
	   karşılıklarıyla değer olarak verebilirsiniz.
           NOT: Eğer herhangi bir ağ arayüzü göremiyorsanız root olarak çalıştırmayı deneyin.

       -e  <field>
           Çıktı formatını "-T fields" olarak belirttikten sonra "-e" parametresi hangi alanların gösterileceğini belirtmek için kullanılır.
	   Örneğin "-e ip.dst" şeklinde tüm paketler içerisindeki, sadece destination ip adresleri listelenebilir.
	   Birden çok alanı yazdırmak istiyorsak "-e ip.src -e ip.dst" şeklinde istediğimiz alanları belirtebiliriz.
	   Veya "-e ip" şeklinde, sadece protokol ismi belirterek o protokole ait tüm sütunları çıktı olarak alabiliriz.
	   Örnek: "tshark -r dosya.pcap -T fields -e ip.src -e ip.dst"

       -E  <field print option>
           Set an option controlling the printing of fields when -T fields is selected.

           Options are:

           header=y|n If y, print a list of the field names given using -e as the first line of the output; the field name will be
           separated using the same character as the field values.  Defaults to n.

           separator=/t|/s|<character> Set the separator character to use for fields.  If /t tab will be used (this is the default), if /s,
           a single space will be used.  Otherwise any character that can be accepted by the command line as part of the option may be
           used.

           occurrence=f|l|a Select which occurrence to use for fields that have multiple occurrences.  If f the first occurrence will be
           used, if l the last occurrence will be used and if a all occurrences will be used (this is the default).

           aggregator=,|/s|<character> Set the aggregator character to use for fields that have multiple occurrences.  If , a comma will be
           used (this is the default), if /s, a single space will be used.  Otherwise any character that can be accepted by the command
           line as part of the option may be used.

           quote=d|s|n Set the quote character to use to surround fields.  d uses double-quotes, s single-quotes, n no quotes (the
           default).

       -f  <capture filter>
           Set the capture filter expression.

           This option can occur multiple times.  If used before the first occurrence of the -i option, it sets the default capture filter
           expression.  If used after an -i option, it sets the capture filter expression for the interface specified by the last -i option
           occurring before this option.  If the capture filter expression is not set specifically, the default capture filter expression
           is used if provided.

       -F  <file format>
	   "-w" parametresi ile toplanılan paketler default olarak pcap formatı ile kaydedilir. "-F" parametresine herhangi bir değer vermeden
	   kullanarak seçebileceğiniz dosya formatlarını görüntüleyebilir ve size gerekli olan formatı bu listeden seçebilirsiniz.

       -g  This option causes the output file(s) to be created with group-read permission (meaning that the output file(s) can be read by
           other members of the calling user's group).

       -G  [ <report type> ]
	   Yardım almak için kullanabileceğimiz bir parametredir.
	   "tshark -G ?" şeklinde bu parametrenin alacağı değerleri görüntüleyebilir, neler bulabileceğinize bakabilirsiniz.

       -h ya da --help
	   TShark sürüm bilgisini, seçeneklerini görüntüler ve durur.

       -H  <input hosts file>
           Read a list of entries from a "hosts" file, which will then be written to a capture file.  Implies -W n. Can be called multiple
           times.

           The "hosts" file format is documented at <http://en.wikipedia.org/wiki/Hosts_(file)>.

       -i  <capture interface> | -
           Ağ arayüzünün adını ya da "tshark -D" ile görüntülediğiniz listedeki numarasını bu parametreye değer olarak verebilirsiniz.
	   Eğer herhangi bir ağ arayüzü belirtmezseniz tshark otomatik olarak arayüzleri kontrol eder ve ilk non-loopback arayüzü, non-loopback
           bir arayüz yok ise ilk loopback arayüzü seçer ve dinlemeye başlar. Eğer herhangi bir ağ arayüzü bulamazsa hata verir ve durur.

	   Bu parametreyi birden fazla sayıda kullanarak aynı anda birden fazla ağ arayüzünü dinleyebilirsiniz. Paketler pcap-ng
	   formatında kaydedilir.

           Not: TShark'ın Win32 sürümü pipelardan capturing yapılmasına izin vermemektedir.

       -I  Put the interface in "monitor mode"; this is supported only on IEEE 802.11 Wi-Fi interfaces, and supported only on some
           operating systems.

           Note that in monitor mode the adapter might disassociate from the network with which it's associated, so that you will not be
           able to use any wireless networks with that adapter.  This could prevent accessing files on a network server, or resolving host
           names or network addresses, if you are capturing in monitor mode and are not connected to another network with another adapter.

           This option can occur multiple times.  If used before the first occurrence of the -i option, it enables the monitor mode for all
           interfaces.  If used after an -i option, it enables the monitor mode for the interface specified by the last -i option occurring
           before this option.

       -K  <keytab>
           Verilen keytab dosyasından kerberos crypto anahtarlarını alır. Birden fazla keytab dosyasını "-K k1.keytab -K k2.keytab" şeklinde
	   okutabilirsiniz.

       -l  Flush the standard output after the information for each packet is printed.  (This is not, strictly speaking, line-buffered if
           -V was specified; however, it is the same as line-buffered if -V wasn't specified, as only one line is printed for each packet,
           and, as -l is normally used when piping a live capture to a program or script, so that output for a packet shows up as soon as
           the packet is seen and dissected, it should work just as well as true line-buffering.  We do this as a workaround for a
           deficiency in the Microsoft Visual C++ C library.)

           This may be useful when piping the output of TShark to another program, as it means that the program to which the output is
           piped will see the dissected data for a packet as soon as TShark sees the packet and generates that output, rather than seeing
           it only when the standard output buffer containing that data fills up.

       -L  Ağ arayüzünün desteklediği veri bağlantı türlerini listeler ve durur. (Listelenen türler "-y" parametresine değer olarak verilir)
	   Örnek: tshark -i 1 -L  (1 numaralı ağ arayüzünün desteklediği türleri listeler)

       -n  Network object name çözümlemesini devre dışı bırakır. (hostname, TCP ve UDP port adları gibi.).

       -N  <name resolving flags>
           Turn on name resolving only for particular types of addresses and port numbers, with name resolving for other types of addresses
           and port numbers turned off.  This flag overrides -n if both -N and -n are present.  If both -N and -n flags are not present,
           all name resolutions are turned on.

           The argument is a string that may contain the letters:

           C to enable concurrent (asynchronous) DNS lookups

           d to enable resolution from captured DNS packets

           m to enable MAC address resolution

           n to enable network address resolution

           N to enable using external resolvers (e.g., DNS) for network address resolution

           t to enable transport-layer port number resolution

       -o  <preference>:<value>
           Set a preference value, overriding the default value and any value read from a preference file.  The argument to the option is a
           string of the form prefname:value, where prefname is the name of the preference (which is the same name that would appear in the
           preference file), and value is the value to which it should be set.

       -O  <protocols>
           "-V" parametresinden farklı olarak, sadece istenilen protokol ya da protokollere ait paketleri detaylı olarak gösterir.
	   "tshark -G protocols" komutu ile kullanabileceğiniz protokol isimlerini görüntüleyebilirsiniz.
	   Örnek: tshark -i 1 -O ARP  (Sadece arp paketlerini detaylı olarak görüntüler)

       -p  Ağ cihazının promiscuous moda geçmesini önler. Promiscuous moda geçmesini istemediğimiz arayüzden önce yazılır. (Örn: -p -i 1)
	   Not: Promiscuous mod ağ trafiğindeki tüm paketlerin toplanmasını sağlar, eğer sadece bir cihazın trafiği incelenecek ise
	   bu parametre kullanılmalıdır.

       -P  Decode and display the packet summary, even if writing raw packet data using the -w option.

       -q  When capturing packets, don't display the continuous count of packets captured that is normally shown when saving a capture to a
           file; instead, just display, at the end of the capture, a count of packets captured.  On systems that support the SIGINFO
           signal, such as various BSDs, you can cause the current count to be displayed by typing your "status" character (typically
           control-T, although it might be set to "disabled" by default on at least some BSDs, so you'd have to explicitly set it to use
           it).

           When reading a capture file, or when capturing and not saving to a file, don't print packet information; this is useful if
           you're using a -z option to calculate statistics and don't want the packet information printed, just the statistics.

       -Q  When capturing packets, only display true errors.  This outputs less than the -q option, so the interface name and total packet
           count and the end of a capture are not sent to stderr.

       -r  <infile>
           Read packet data from infile, can be any supported capture file format (including gzipped files).  It is possible to use named
           pipes or stdin (-) here but only with certain (not compressed) capture file formats (in particular: those that can be read
           without seeking backwards).

       -R  <Read filter>
           Cause the specified filter (which uses the syntax of read/display filters, rather than that of capture filters) to be applied
           during the first pass of analysis. Packets not matching the filter are not considered for future passes. Only makes sense with
           multiple passes, see -2. For regular filtering on single-pass dissect see -Y instead.

           Note that forward-looking fields such as 'response in frame #' cannot be used with this filter, since they will not have been
           calculate when this filter is applied.

       -s  <capture snaplen>
           Set the default snapshot length to use when capturing live data.  No more than snaplen bytes of each network packet will be read
           into memory, or saved to disk.  A value of 0 specifies a snapshot length of 65535, so that the full packet is captured; this is
           the default.

           This option can occur multiple times.  If used before the first occurrence of the -i option, it sets the default snapshot
           length.  If used after an -i option, it sets the snapshot length for the interface specified by the last -i option occurring
           before this option.  If the snapshot length is not set specifically, the default snapshot length is used if provided.

       -S  <separator>
           Set the line separator to be printed between packets.

       -t  a|ad|adoy|d|dd|e|r|u|ud|udoy
           Set the format of the packet timestamp printed in summary lines.  The format can be one of:

           a absolute: The absolute time, as local time in your time zone, is the actual time the packet was captured, with no date
           displayed

           ad absolute with date: The absolute date, displayed as YYYY-MM-DD, and time, as local time in your time zone, is the actual time
           and date the packet was captured

           adoy absolute with date using day of year: The absolute date, displayed as YYYY/DOY, and time, as local time in your time zone,
           is the actual time and date the packet was captured

           d delta: The delta time is the time since the previous packet was captured

           dd delta_displayed: The delta_displayed time is the time since the previous displayed packet was captured

           e epoch: The time in seconds since epoch (Jan 1, 1970 00:00:00)

           r relative: The relative time is the time elapsed between the first packet and the current packet

           u UTC: The absolute time, as UTC, is the actual time the packet was captured, with no date displayed

           ud UTC with date: The absolute date, displayed as YYYY-MM-DD, and time, as UTC, is the actual time and date the packet was
           captured

           udoy UTC with date using day of year: The absolute date, displayed as YYYY/DOY, and time, as UTC, is the actual time and date
           the packet was captured

           The default format is relative.

       -T  fields|pdml|ps|psml|text
           Decode haldeki paket datasının görüntülenme formatını belirler.  The options are one of:

           fields The values of fields specified with the -e option, in a form specified by the -E option.  For example,

             -T fields -E separator=, -E quote=d

           would generate comma-separated values (CSV) output suitable for importing into your favorite spreadsheet program.

           pdml Packet Details Markup Language, an XML-based format for the details of a decoded packet.  This information is equivalent to
           the packet details printed with the -V flag.

           ps PostScript for a human-readable one-line summary of each of the packets, or a multi-line view of the details of each of the
           packets, depending on whether the -V flag was specified.

           psml Packet Summary Markup Language, an XML-based format for the summary information of a decoded packet.  This information is
           equivalent to the information shown in the one-line summary printed by default.

           text Text of a human-readable one-line summary of each of the packets, or a multi-line view of the details of each of the
           packets, depending on whether the -V flag was specified.  This is the default.

       -u <seconds type>
           Specifies the seconds type.  Valid choices are:

           s for seconds

           hms for hours, minutes and seconds

       -v  TShark sürüm bilgisini görüntüler ve durur.

       -V  Paketlerin içerisindeki tüm alanları "İsim: Değer" şeklinde detaylı bir biçimde görüntüler.

       -w  <outfile> | -
           Belirtilen isimde bir dosya oluşturur ve raw paket datasını bu dosyaya yazar. Eğer "-w -" şeklinde kullanılırsa bu datayı sadece
	   ekranda gösterir, herhangi bir dosyaya yazmaz.

           NOT: -w parametresi text değil paket datası verir, eğer text formatında bir dataya ihtiyacınız varsa
	   "tshark [parametreler] > dosyaAdi.txt" şeklinde bir komut kullanın.

       -W  <file format option>
           Save extra information in the file if the format supports it.  For example,

             -F pcapng -W n

           will save host name resolution records along with captured packets.

           Future versions of Wireshark may automatically change the capture format to pcapng as needed.

           The argument is a string that may contain the following letter:

           n write network address resolution information (pcapng only)

       -x  Paketlerin hex ve ASCII değerlerini görüntüler. Dosyadan okuma ya da capturing işlemlerinde kullanılabiliyor.

       -X <eXtension options>
           Specify an option to be passed to a TShark module.  The eXtension option is in the form extension_key:value, where extension_key
           can be:

           lua_script:lua_script_filename tells TShark to load the given script in addition to the default Lua scripts.

           lua_scriptnum:argument tells TShark to pass the given argument to the lua script identified by 'num', which is the number
           indexed order of the 'lua_script' command.  For example, if only one script was loaded with '-X lua_script:my.lua', then '-X
           lua_script1:foo' will pass the string 'foo' to the 'my.lua' script.  If two scripts were loaded, such as '-X lua_script:my.lua'
           and '-X lua_script:other.lua' in that order, then a '-X lua_script2:bar' would pass the string 'bar' to the second lua script,
           namely 'other.lua'.

           read_format:file_format tells TShark to use the given file format to read in the file (the file given in the -r command option).
           Providing no file_format argument, or an invalid one, will produce a file of available file formats to use.

       -y  <capture link type>
           Set the data link type to use while capturing packets.  The values reported by -L are the values that can be used.

           This option can occur multiple times.  If used before the first occurrence of the -i option, it sets the default capture link
           type.  If used after an -i option, it sets the capture link type for the interface specified by the last -i option occurring
           before this option.  If the capture link type is not set specifically, the default capture link type is used if provided.

       -Y  <displaY filter>
           Eğer paketleri tek bir koşula göre filtreleyeceksek bu parametreyi kullanmalıyız.
	   Örnek: tshark -pni 1 -Y "dns"   (Bulunan ilk ağ arayüzünü dinler, sadece dns paketlerini görüntüler)
	   Örnek: tshark -r dosya.pcap -Y "dns" (Dosya içerisindeki tüm paketleri filtreler, sadece dns paketlerini görüntüler)

	   NOTLAR:
	   * Eğer ki paketleri filtrelenmiş olarak kaydetmek istiyorsanız "-f" parametresini kullanmalısınız.
	   * Tek koşullu bir filtre uygulayacaksanız, performans açısından "-R" yerine bu parametreyi kullanın.

       -z  <statistics>
	   Bu parametre vereceğiniz değerlere göre, paketlere ait istatistiksel bilgiler görüntülemektedir.
	   *** İşlemin daha çabuk tamamlanmasını sağlamak ve sadece istatistik bilgilerini almak için "-q" parametresi ile birlikte kullanarak 		paketlerin listelenmesini iptal etmelisiniz.

           -z help
               -z parametresine verebileceğiniz tüm değerleri görüntüler.

CAPTURİNG FİLTRELERİ
	Paket yakalama aşamasında kullanabileceğimiz filtreler.
		man pcap-filter
		man tcpdump
		<https://wiki.wireshark.org/CaptureFilters>.

GÖRÜNTÜLEME FİLTRELERİ
	Kaydedilmiş paketleri incelerken kullanabileceğiniz filtreler.
		man wireshark-filter
		<https://wiki.wireshark.org/DisplayFilters>
		<https://www.wireshark.org/docs/dfref/>
************************************
FILES
       These files contains various Wireshark configuration values.

       Preferences
           The preferences files contain global (system-wide) and personal preference settings.  If the system-wide preference file exists,
           it is read first, overriding the default settings.  If the personal preferences file exists, it is read next, overriding any
           previous values.  Note: If the command line option -o is used (possibly more than once), it will in turn override values from
           the preferences files.

           The preferences settings are in the form prefname:value, one per line, where prefname is the name of the preference and value is
           the value to which it should be set; white space is allowed between : and value.  A preference setting can be continued on
           subsequent lines by indenting the continuation lines with white space.  A # character starts a comment that runs to the end of
           the line:

             # Capture in promiscuous mode?
             # TRUE or FALSE (case-insensitive).
             capture.prom_mode: TRUE

           The global preferences file is looked for in the wireshark directory under the share subdirectory of the main installation
           directory (for example, /usr/local/share/wireshark/preferences) on UNIX-compatible systems, and in the main installation
           directory (for example, C:\Program Files\Wireshark\preferences) on Windows systems.

           The personal preferences file is looked for in $HOME/.wireshark/preferences on UNIX-compatible systems and
           %APPDATA%\Wireshark\preferences (or, if %APPDATA% isn't defined, %USERPROFILE%\Application Data\Wireshark\preferences) on
           Windows systems.

       Disabled (Enabled) Protocols
           The disabled_protos files contain system-wide and personal lists of protocols that have been disabled, so that their dissectors
           are never called.  The files contain protocol names, one per line, where the protocol name is the same name that would be used
           in a display filter for the protocol:

             http
             tcp     # a comment

           The global disabled_protos file uses the same directory as the global preferences file.

           The personal disabled_protos file uses the same directory as the personal preferences file.

       Name Resolution (hosts)
           If the personal hosts file exists, it is used to resolve IPv4 and IPv6 addresses before any other attempts are made to resolve
           them.  The file has the standard hosts file syntax; each line contains one IP address and name, separated by whitespace.  The
           same directory as for the personal preferences file is used.

           Capture filter name resolution is handled by libpcap on UNIX-compatible systems and WinPcap on Windows.  As such the Wireshark
           personal hosts file will not be consulted for capture filter name resolution.

       Name Resolution (subnets)
           If the an IPv4 address cannot be translated via name resolution (no exact match is found) then a partial match is attempted via
           the subnets file.

           Each line of this file consists of an IPv4 address, a subnet mask length separated only by a / and a name separated by
           whitespace. While the address must be a full IPv4 address, any values beyond the mask length are subsequently ignored.

           An example is:

           # Comments must be prepended by the # sign!  192.168.0.0/24 ws_test_network

           A partially matched name will be printed as "subnet-name.remaining-address".  For example, "192.168.0.1" under the subnet above
           would be printed as "ws_test_network.1"; if the mask length above had been 16 rather than 24, the printed address would be
           ``ws_test_network.0.1".

       Name Resolution (ethers)
           The ethers files are consulted to correlate 6-byte hardware addresses to names.  First the personal ethers file is tried and if
           an address is not found there the global ethers file is tried next.

           Each line contains one hardware address and name, separated by whitespace.  The digits of the hardware address are separated by
           colons (:), dashes (-) or periods (.).  The same separator character must be used consistently in an address.  The following
           three lines are valid lines of an ethers file:

             ff:ff:ff:ff:ff:ff          Broadcast
             c0-00-ff-ff-ff-ff          TR_broadcast
             00.00.00.00.00.00          Zero_broadcast

           The global ethers file is looked for in the /etc directory on UNIX-compatible systems, and in the main installation directory
           (for example, C:\Program Files\Wireshark) on Windows systems.

           The personal ethers file is looked for in the same directory as the personal preferences file.

           Capture filter name resolution is handled by libpcap on UNIX-compatible systems and WinPcap on Windows.  As such the Wireshark
           personal ethers file will not be consulted for capture filter name resolution.

       Name Resolution (manuf)
           The manuf file is used to match the 3-byte vendor portion of a 6-byte hardware address with the manufacturer's name; it can also
           contain well-known MAC addresses and address ranges specified with a netmask.  The format of the file is the same as the ethers
           files, except that entries of the form:

             00:00:0C      Cisco

           can be provided, with the 3-byte OUI and the name for a vendor, and entries such as:

             00-00-0C-07-AC/40     All-HSRP-routers

           can be specified, with a MAC address and a mask indicating how many bits of the address must match.  The above entry, for
           example, has 40 significant bits, or 5 bytes, and would match addresses from 00-00-0C-07-AC-00 through 00-00-0C-07-AC-FF.  The
           mask need not be a multiple of 8.

           The manuf file is looked for in the same directory as the global preferences file.

       Name Resolution (services)
           The services file is used to translate port numbers into names.

           The file has the standard services file syntax; each line contains one (service) name and one transport identifier separated by
           white space.  The transport identifier includes one port number and one transport protocol name (typically tcp, udp, or sctp)
           separated by a /.

           An example is:

           mydns       5045/udp     # My own Domain Name Server mydns       5045/tcp     # My own Domain Name Server

       Name Resolution (ipxnets)
           The ipxnets files are used to correlate 4-byte IPX network numbers to names.  First the global ipxnets file is tried and if that
           address is not found there the personal one is tried next.

           The format is the same as the ethers file, except that each address is four bytes instead of six.  Additionally, the address can
           be represented as a single hexadecimal number, as is more common in the IPX world, rather than four hex octets.  For example,
           these four lines are valid lines of an ipxnets file:

             C0.A8.2C.00              HR
             c0-a8-1c-00              CEO
             00:00:BE:EF              IT_Server1
             110f                     FileServer3

           The global ipxnets file is looked for in the /etc directory on UNIX-compatible systems, and in the main installation directory
           (for example, C:\Program Files\Wireshark) on Windows systems.

           The personal ipxnets file is looked for in the same directory as the personal preferences file.

ENVIRONMENT VARIABLES
       WIRESHARK_APPDATA
           On Windows, Wireshark normally stores all application data in %APPDATA% or %USERPROFILE%.  You can override the default location
           by exporting this environment variable to specify an alternate location.

       WIRESHARK_DEBUG_WMEM_OVERRIDE
           Setting this environment variable forces the wmem framework to use the specified allocator backend for *all* allocations,
           regardless of which backend is normally specified by the code. This is mainly useful to developers when testing or debugging.
           See README.wmem in the source distribution for details.

       WIRESHARK_RUN_FROM_BUILD_DIRECTORY
           This environment variable causes the plugins and other data files to be loaded from the build directory (where the program was
           compiled) rather than from the standard locations.  It has no effect when the program in question is running with root (or
           setuid) permissions on *NIX.

       WIRESHARK_DATA_DIR
           This environment variable causes the various data files to be loaded from a directory other than the standard locations.  It has
           no effect when the program in question is running with root (or setuid) permissions on *NIX.

       ERF_RECORDS_TO_CHECK
           This environment variable controls the number of ERF records checked when deciding if a file really is in the ERF format.
           Setting this environment variable a number higher than the default (20) would make false positives less likely.

       IPFIX_RECORDS_TO_CHECK
           This environment variable controls the number of IPFIX records checked when deciding if a file really is in the IPFIX format.
           Setting this environment variable a number higher than the default (20) would make false positives less likely.

       WIRESHARK_ABORT_ON_DISSECTOR_BUG
           If this environment variable is set, TShark will call abort(3) when a dissector bug is encountered.  abort(3) will cause the
           program to exit abnormally; if you are running TShark in a debugger, it should halt in the debugger and allow inspection of the
           process, and, if you are not running it in a debugger, it will, on some OSes, assuming your environment is configured correctly,
           generate a core dump file.  This can be useful to developers attempting to troubleshoot a problem with a protocol dissector.

       WIRESHARK_ABORT_ON_TOO_MANY_ITEMS
           If this environment variable is set, TShark will call abort(3) if a dissector tries to add too many items to a tree (generally
           this is an indication of the dissector not breaking out of a loop soon enough).  abort(3) will cause the program to exit
           abnormally; if you are running TShark in a debugger, it should halt in the debugger and allow inspection of the process, and, if
           you are not running it in a debugger, it will, on some OSes, assuming your environment is configured correctly, generate a core
           dump file.  This can be useful to developers attempting to troubleshoot a problem with a protocol dissector.

Daha fazla bilgi için bakmanızı önerebileceğim manualler:
	wireshark-filter, wireshark, editcap, pcap, dumpcap, text2pcap, mergecap, pcap-filter, tcpdump
	(örn: man wireshark-filter)
NOTLAR:
	TShark, Wireshark yazılımının bir parçasıdır. Wireshark'ın son sürümüne ve daha fazla bilgiye <https://www.wireshark.org> adresinden
	ulaşabilirsiniz.