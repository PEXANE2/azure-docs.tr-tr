---
title: Linux performans araçları
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmetini (AKS) kullanırken sık karşılaşılan sorunları nasıl gidereceğinizi ve nasıl çözeceğinizi öğrenin
services: container-service
author: alexeldeib
ms.service: container-service
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: aleldeib
ms.openlocfilehash: eb6b126b4d1794adf0380432040190b91a17a675
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77925611"
---
# <a name="linux-performance-troubleshooting"></a>Linux Performans Sorun Giderme

Linux makinelerinde kaynak tükenmesi yaygın bir konudur ve belirtiler geniş bir yelpazede tezahür edebilir. Bu belge, bu tür sorunları tanılamaya yardımcı olmak için kullanılabilir araçların üst düzey bir genel bakış sağlar.

Bu araçların çoğu, yuvarlama çıktısı üretmek için bir aralık kabul eder. Bu çıktı biçimi genellikle lekeleme desenleri çok daha kolay hale getirir. Kabul edildiği durumlarda, örnek çağırma `[interval]`içerir.

Bu araçların çoğu geniş bir geçmişe ve geniş bir yapılandırma seçeneğikümesine sahiptir. Bu sayfa, sık karşılaşılan sorunları vurgulamak için yalnızca basit bir çağrı alt kümesi sağlar. Kanonik bilgi kaynağı her zaman her araç için referans belgeleridir. Bu belgeler burada sağlanandan çok daha kapsamlı olacaktır.

## <a name="guidance"></a>Rehber

Performans sorunlarını araştırma yaklaşımınızda sistematik olun. Use (kullanım, doygunluk, hatalar) ve RED (oran, hatalar, süre) olmak üzere iki yaygın yaklaşım dır. RED genellikle istek tabanlı izleme hizmetleri bağlamında kullanılır. USE genellikle kaynakları izlemek için kullanılır: bir makinedeki her kaynak için, kullanımı, doygunluğu ve hataları izleyin. Herhangi bir makinedeki dört ana kaynak cpu, bellek, disk ve ağdır. Bu kaynaklardan herhangi biri için yüksek kullanım, doygunluk veya hata oranları sistemle ilgili olası bir sorunu gösterir. Bir sorun olduğunda, temel nedeni araştırın: disk IO gecikme sonları neden yüksek? Diskler veya sanal makine SKU daraltılmış mı? Aygıtlara hangi işlemler ve hangi dosyalara yazıyorum?

Tanılamak için sık karşılaşılan sorunlara ve göstergelere bazı örnekler:
- IOPS azaltma: cihaz başına IOPS ölçmek için iostat kullanın. Tek bir diskin sınırının üzerinde olmadığından ve tüm disklerin toplamının sanal makine için sınırdan daha az olduğundan emin olun.
- Bant genişliği azaltma: IOPS için iostat kullanın, ancak okuma/yazma iş bkullanımını ölçme. Hem cihaz başına hem de toplam iş artışının bant genişliği sınırlarının altında olduğundan emin olun.
- SNAT tükenmesi: Bu SAR yüksek aktif (giden) bağlantılar olarak tezahür edebilir. 
- Paket kaybı: Bu, gönderilen/alınan sayıma göre TCP retransmit sayısı ile proxy ile ölçülebilir. Her `sar` `netstat` ikisi de ve bu bilgileri gösterebilir.

## <a name="general"></a>Genel

Bu araçlar genel amaçlıdır ve temel sistem bilgilerini kapsar. Onlar daha fazla araştırma için iyi bir başlangıç noktasıdır.

### <a name="uptime"></a>Uptime

```
$ uptime
 19:32:33 up 17 days, 12:36,  0 users,  load average: 0.21, 0.77, 0.69
```

çalışma süresi sistem çalışma süresi ve 1, 5 ve 15 dakikalık yük ortalamaları sağlar. Bu yük ortalamaları kabaca iş yapan veya tamamlanması için kesintisiz çalışma bekleyen iş parçacıkları karşılık gelir. Mutlak olarak bu sayıları yorumlamak zor olabilir, ancak zaman içinde ölçülen bize yararlı bilgiler söyleyebilir:

- 1 dakikalık ortalama > 5 dakikalık ortalama yük artıyor demektir.
- 1 dakikalık ortalama < 5 dakikalık ortalama, yükün azaldığı anlamına gelir.

çalışma süresi, bilgilerin neden kullanılmadığını da aydınlatabilir: kullanıcı makineye erişemeden önce sorun kendi başına veya yeniden başlatılarak çözülmüş olabilir.

Kullanılabilir CPU iş parçacığı sayısından daha yüksek yük ortalamaları, belirli bir iş yüküyle ilgili bir performans sorununu gösterebilir.

### <a name="dmesg"></a>Dmesg

```
$ dmesg | tail 
$ dmesg --level=err | tail
```

dmesg çekirdek tampon döker. OOMKill gibi olaylar çekirdek arabelleği bir giriş ekleyin. Dmesg günlüklerinde BIR OOMKill veya diğer kaynak tükenme sebunu bulma, bir sorunun güçlü bir göstergesidir.

### <a name="top"></a>üst

```
$ top
Tasks: 249 total,   1 running, 158 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.2 us,  1.3 sy,  0.0 ni, 95.4 id,  1.0 wa,  0.0 hi,  0.2 si,  0.0 st
KiB Mem : 65949064 total, 43415136 free,  2349328 used, 20184600 buff/cache
KiB Swap:        0 total,        0 free,        0 used. 62739060 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
116004 root      20   0  144400  41124  27028 S  11.8  0.1 248:45.45 coredns
  4503 root      20   0 1677980 167964  89464 S   5.9  0.3   1326:25 kubelet
     1 root      20   0  120212   6404   4044 S   0.0  0.0  48:20.38 systemd
     ...
```

`top`geçerli sistem durumuna geniş bir genel bakış sağlar. Üstbilgiler bazı yararlı toplu bilgiler sağlar:

- görev durumu: koşma, uyuma, durma.
- CPU kullanımı, bu durumda çoğunlukla boşta zaman gösteren.
- toplam, ücretsiz ve kullanılan sistem belleği.

`top`kısa ömürlü süreçleri kaçırabilir; gibi `htop` alternatifler `atop` ve bu eksikliklerin bazılarını giderirken benzer arayüzler sağlar.

## <a name="cpu"></a>CPU

Bu araçlar CPU kullanım bilgilerini sağlar. Bu, özellikle desenlerin tespit edilebildiği haddeleme çıktısı ile kullanışlıdır.

### <a name="mpstat"></a>mpstat

```
$ mpstat -P ALL [interval]
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

19:49:03     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
19:49:04     all    1.01    0.00    0.63    2.14    0.00    0.13    0.00    0.00    0.00   96.11
19:49:04       0    1.01    0.00    1.01   17.17    0.00    0.00    0.00    0.00    0.00   80.81
19:49:04       1    1.98    0.00    0.99    0.00    0.00    0.00    0.00    0.00    0.00   97.03
19:49:04       2    1.01    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00   97.98
19:49:04       3    0.00    0.00    0.99    0.00    0.00    0.99    0.00    0.00    0.00   98.02
19:49:04       4    1.98    0.00    1.98    0.00    0.00    0.00    0.00    0.00    0.00   96.04
19:49:04       5    1.00    0.00    1.00    0.00    0.00    0.00    0.00    0.00    0.00   98.00
19:49:04       6    1.00    0.00    1.00    0.00    0.00    0.00    0.00    0.00    0.00   98.00
19:49:04       7    1.98    0.00    0.99    0.00    0.00    0.00    0.00    0.00    0.00   97.03
```

`mpstat`benzer CPU bilgilerini üste yazdırır, ancak CPU iş parçacığına göre ayrılmıştır. Tüm çekirdekleri aynı anda görmek, örneğin tek bir dişli uygulama %100 kullanımda tek bir çekirdek kullandığında, son derece dengesiz CPU kullanımını algılamak için yararlı olabilir. Bu sorunu, sistemdeki tüm CPU'lar üzerinde toplandığında fark etmek daha zor olabilir.

### <a name="vmstat"></a>vmstat

```
$ vmstat [interval]
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 43300372 545716 19691456    0    0     3    50    3    3  2  1 95  1  0
```

`vmstat`benzer bilgiler `mpstat` `top`sağlar ve, CPU (r sütunu), bellek istatistikleri ve her iş durumunda harcanan CPU zamanının yüzdesi bekleyen işlemlerin sayısını numaralandırma.

## <a name="memory"></a>Bellek

Bellek çok önemli ve neyse ki kolay, kaynak izlemektir. Bazı araçlar, cpu ve bellek `vmstat`gibi hem de rapor edebilirsiniz. Ama gibi `free` araçlar hala hızlı hata ayıklama için yararlı olabilir.

### <a name="free"></a>serbest

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:          64403        2338       42485           1       19579       61223
Swap:             0           0           0
```

`free`kullanılan ve boş bellek hakkında temel bilgileri sunar. `vmstat`haddeleme çıktısı sağlama yeteneği nden dolayı temel bellek analizi için bile daha yararlı olabilir.

## <a name="disk"></a>Disk

Bu araçlar disk IOPS'yi, bekleme sıralarını ve toplam iş artışlarını ölçer. 

### <a name="iostat"></a>iostat

```
$ iostat -xy [interval] [count]
$ iostat -xy 1 1
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           3.42    0.00    2.92    1.90    0.00   91.76

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
loop0             0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
sdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
sda               0.00    56.00    0.00   65.00     0.00   504.00    15.51     0.01    3.02    0.00    3.02   0.12   0.80
scd0              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
```

`iostat`disk kullanımına ilişkin derin bilgiler sağlar. Bu çağrı, `-x` uzun istatistikler `-y` için geçer, önyükleme beri ilk çıktı `1 1` yazdırma sistemi ortalamaları atlamak için, ve biz 1 saniyelik aralık istiyorum belirtmek için, çıkış bir blok sonra biten. 

`iostat`birçok yararlı istatistik ortaya çıkarır:

- `r/s`ve `w/s` saniyede okur ve saniyede yazar. Bu değerlerin toplamı IOPS'dir.
- `rkB/s`ve `wkB/s` kilobaytlar saniyede okunur/yazılır. Bu değerlerin toplamı iş bölümüdür.
- `await`sıraya alınan istekler için milisaniye cinsinden ortalama iowait süresidir.
- `avgqu-sz`sağlanan aralık üzerindeki ortalama sıra boyutudur.

Azure VM'de:

- ve tek `r/s` `w/s` bir blok aygıtı için toplamı, bu diskin SKU sınırlarını aşamaz.
- ve tek `rkB/s` `wkB/s` bir blok aygıtı için toplamı, bu diskin SKU sınırlarını aşamaz
- tüm blok `r/s` `w/s` aygıtlarının toplamı VM SKU sınırlarını aşamaz.
- tüm blok `rkB/s` aygıtları için 'wkB/s toplamı VM SKU sınırlarını aşamaz.

İşletim sistemi diskinin kapasitesine karşılık gelen en küçük SKU'nun yönetilen diski olarak sayıldığını unutmayın. Örneğin, 1024GB işletim sistemi diski Bir P30 diske karşılık gelir. Kısa ömürlü işletim sistemi diskleri ve geçici diskler tek tek disk sınırları yoktur; bunlar sadece tam VM limitleri ile sınırlıdır.

Bekleyen veya avgqu-sz sıfır olmayan değerleri de IO çekişme iyi göstergelervardır.

## <a name="network"></a>Ağ

Bu araçlar, iş aktarımı, iletim hataları ve kullanım gibi ağ istatistiklerini ölçer. Daha derin analizler, sıkışıklık ve bırakılan paketler hakkında ince taneli TCP istatistiklerini ortaya çıkarabilir.

### <a name="sar"></a>Sar

```
$ sar -n DEV [interval]
22:36:57        IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
22:36:58      docker0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
22:36:58    azv604be75d832      1.00      9.00      0.06      1.04      0.00      0.00      0.00      0.00
22:36:58       azure0     68.00     79.00     27.79     52.79      0.00      0.00      0.00      0.00
22:36:58    azv4a8e7704a5b    202.00    207.00     37.51     21.86      0.00      0.00      0.00      0.00
22:36:58    azve83c28f6d1c     21.00     30.00     24.12      4.11      0.00      0.00      0.00      0.00
22:36:58         eth0    314.00    321.00     70.87    163.28      0.00      0.00      0.00      0.00
22:36:58    azva3128390bff     12.00     20.00      1.14      2.29      0.00      0.00      0.00      0.00
22:36:58    azvf46c95ddea3     10.00     18.00     31.47      1.36      0.00      0.00      0.00      0.00
22:36:58       enP1s1     74.00    374.00     29.36    166.94      0.00      0.00      0.00      0.00
22:36:58           lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
22:36:58    azvdbf16b0b2fc      9.00     19.00      3.36      1.18      0.00      0.00      0.00      0.00
```

`sar`analiz geniş bir yelpazede için güçlü bir araçtır. Bu örnek, ağ istatistiklerini ölçme yeteneğini kullansa da, CPU ve bellek tüketimini ölçmek için eşit derecede güçlüdür. Bu örnek, `sar` `-n` ağ çıktısını `DEV` aygıt tarafından görüntüleyen (ağ aygıtı) anahtar sözcük lerini belirtmek için bayrakla çağrır.

- Belirli bir `rxKb/s` `txKb/s` aygıtın toplam iş bölümü ve toplamıdır. Bu değer, sağlanan Azure NIC için sınırı aştığında, makinedeki iş yükleri artan ağ gecikmesi ile karşılaşır.
- `%ifutil`belirli bir cihaz için kullanımını ölçer. Bu değer %100'e yaklaştıkça, iş yükleri ağ gecikmesi artırılır.

```
$ sar -n TCP,ETCP [interval]
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

22:50:08     active/s passive/s    iseg/s    oseg/s
22:50:09         2.00      0.00     19.00     24.00

22:50:08     atmptf/s  estres/s retrans/s isegerr/s   orsts/s
22:50:09         0.00      0.00      0.00      0.00      0.00

Average:     active/s passive/s    iseg/s    oseg/s
Average:         2.00      0.00     19.00     24.00

Average:     atmptf/s  estres/s retrans/s isegerr/s   orsts/s
Average:         0.00      0.00      0.00      0.00      0.00
```

Bu çağrı, `sar` `TCP,ETCP` TCP bağlantılarını incelemek için anahtar kelimeleri kullanır. Son satırın üçüncü sütunu olan "retrans", tcp saniyede yeniden iletim sayısıdır. Bu alan için yüksek değerler güvenilir olmayan bir ağ bağlantısını gösterir. Birinci ve üçüncü satırlarda "etkin" yerel aygıttan kaynaklanan bir bağlantı anlamına gelirken, "uzak" gelen bir bağlantıyı gösterir.  Azure'da sık karşılaşılan bir sorun, algılamaya yardımcı olabilecek `sar` SNAT bağlantı noktası tükenmesidir. Sorun, yerel olarak başlatılan TCP bağlantılarının yüksek oranda olması nedeniyle SNAT bağlantı noktası tükenmesi yüksek "etkin" değerler olarak tezahür eder.

Bir `sar` aralık aldığından, yuvarlanma çıktısını yazdırır ve ardından çağırmadan elde edilen ortalama sonuçları içeren son çıktı satırlarını yazdırır.

### <a name="netstat"></a>Netstat

```
$ netstat -s
Ip:
    71046295 total packets received
    78 forwarded
    0 incoming packets discarded
    71046066 incoming packets delivered
    83774622 requests sent out
    40 outgoing packets dropped
Icmp:
    103 ICMP messages received
    0 input ICMP message failed.
    ICMP input histogram:
        destination unreachable: 103
    412802 ICMP messages sent
    0 ICMP messages failed
    ICMP output histogram:
        destination unreachable: 412802
IcmpMsg:
        InType3: 103
        OutType3: 412802
Tcp:
    11487089 active connections openings
    592 passive connection openings
    1137 failed connection attempts
    404 connection resets received
    17 connections established
    70880911 segments received
    95242567 segments send out
    176658 segments retransmited
    3 bad segments received.
    163295 resets sent
Udp:
    164968 packets received
    84 packets to unknown port received.
    0 packet receive errors
    165082 packets sent
UdpLite:
TcpExt:
    5 resets received for embryonic SYN_RECV sockets
    1670559 TCP sockets finished time wait in fast timer
    95 packets rejects in established connections because of timestamp
    756870 delayed acks sent
    2236 delayed acks further delayed because of locked socket
    Quick ack mode was activated 479 times
    11983969 packet headers predicted
    25061447 acknowledgments not containing data payload received
    5596263 predicted acknowledgments
    19 times recovered from packet loss by selective acknowledgements
    Detected reordering 114 times using SACK
    Detected reordering 4 times using time stamp
    5 congestion windows fully recovered without slow start
    1 congestion windows partially recovered using Hoe heuristic
    5 congestion windows recovered without slow start by DSACK
    111 congestion windows recovered without slow start after partial ack
    73 fast retransmits
    26 retransmits in slow start
    311 other TCP timeouts
    TCPLossProbes: 198845
    TCPLossProbeRecovery: 147
    480 DSACKs sent for old packets
    175310 DSACKs received
    316 connections reset due to unexpected data
    272 connections reset due to early user close
    5 connections aborted due to timeout
    TCPDSACKIgnoredNoUndo: 8498
    TCPSpuriousRTOs: 1
    TCPSackShifted: 3
    TCPSackMerged: 9
    TCPSackShiftFallback: 177
    IPReversePathFilter: 4
    TCPRcvCoalesce: 1501457
    TCPOFOQueue: 9898
    TCPChallengeACK: 342
    TCPSYNChallenge: 3
    TCPSpuriousRtxHostQueues: 17
    TCPAutoCorking: 2315642
    TCPFromZeroWindowAdv: 483
    TCPToZeroWindowAdv: 483
    TCPWantZeroWindowAdv: 115
    TCPSynRetrans: 885
    TCPOrigDataSent: 51140171
    TCPHystartTrainDetect: 349
    TCPHystartTrainCwnd: 7045
    TCPHystartDelayDetect: 26
    TCPHystartDelayCwnd: 862
    TCPACKSkippedPAWS: 3
    TCPACKSkippedSeq: 4
    TCPKeepAlive: 62517
IpExt:
    InOctets: 36416951539
    OutOctets: 41520580596
    InNoECTPkts: 86631440
    InECT0Pkts: 14
```

`netstat`burada özet çıktı ile çağrılan, ağ istatistikleri geniş bir yelpazede içe dönük olabilir. Konuya bağlı olarak burada birçok yararlı alanlar vardır. TCP bölümündeki yararlı alanlardan biri "başarısız bağlantı denemeleri"dir. Bu, SNAT bağlantı noktası tükenmesinin veya giden bağlantıları oluşturan diğer sorunların bir göstergesi olabilir. Yüksek oranda yeniden aktarılan segmentler (ayrıca TCP bölümü altında) paket teslimiyle ilgili sorunları gösterebilir. 
