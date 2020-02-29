---
title: Linux performans araçları
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service (AKS) kullanırken karşılaşılan yaygın sorunları giderme ve çözme hakkında bilgi edinin
services: container-service
author: alexeldeib
ms.service: container-service
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: aleldeib
ms.openlocfilehash: eb6b126b4d1794adf0380432040190b91a17a675
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77925611"
---
# <a name="linux-performance-troubleshooting"></a>Linux performans sorunlarını giderme

Linux makinelerde kaynak tükenmesi, yaygın bir sorundur ve çok çeşitli belirtilerle bildirimde bulunabilir. Bu belge, bu tür sorunları tanılamaya yardımcı olmak için kullanılabilen araçlara yüksek düzeyde bir genel bakış sağlar.

Bu araçların birçoğu, sıralı çıkış oluşturmak için bir aralığı kabul eder. Bu çıkış biçimi genellikle, biriktirme desenleri çok daha kolay hale gelir. Kabul edildiğinde, örnek çağırma `[interval]`içerir.

Bu araçların çoğunda kapsamlı bir geçmiş ve geniş bir yapılandırma seçenekleri kümesi vardır. Bu sayfa, yaygın sorunları vurgulamak için yalnızca basit bir çağırma alt kümesi sağlar. Kurallı bilgi kaynağı her bir araç için her zaman başvuru belgesidir. Bu belge, burada sağlandıklarınızın çok daha kapsamlı olacaktır.

## <a name="guidance"></a>Rehber

Performans sorunlarını araştırmaya yönelik yaklaşımınıza sistematik bir çözüm olun. Yaygın olarak kullanılan iki yaklaşım (kullanım, doygunluk, hatalar) ve kırmızı (oran, hatalar, süre). RED genellikle, istek tabanlı izleme için hizmetler bağlamında kullanılır. KULLANıM genellikle kaynakları izlemek için kullanılır: bir makinedeki her kaynak için, kullanımı, doygunluğu ve hataları izleyin. Herhangi bir makinedeki dört ana kaynak türü CPU, bellek, disk ve ağ ' dir. Bu kaynakların herhangi birine yönelik yüksek kullanım, doygunluk veya hata ücretleri sistemle ilgili olası bir sorunu gösterir. Bir sorun olduğunda, kök nedenini araştırın: neden disk GÇ gecikme süresi yüksek? Diskler veya sanal makine SKU 'SU kısıtlandı mı? Cihazlara hangi süreçler yazıyor ve dosyalar?

Yaygın sorunlara ve bunları tanılamaya yönelik göstergelerine örnek olarak şunlar verilebilir:
- IOPS daraltma: cihaz başına ıOPS 'yi ölçmek için Iostat kullanın. Tek bir diskin sınırının üzerinde olmadığından emin olun ve tüm disklerin toplamı, sanal makine limitinden daha düşük.
- Bant genişliği azaltma: ıOPS için Iostat kullanın, ancak okuma/yazma verimini ölçmeye devam edin. Hem cihaz başına hem de toplam verimlilik bant genişliği sınırlarının altında olduğundan emin olun.
- SNAT tükenmesi: Bu, çın 'de yüksek etkin (giden) bağlantılar olarak bildirimde bulunabilir. 
- Paket kaybı: Bu, gönderilen/alınan sayımla ilişkili TCP yeniden aktarım sayısı aracılığıyla proxy ile ölçülebilir. `sar` ve `netstat`, bu bilgileri gösterebilir.

## <a name="general"></a>Genel

Bu araçlar genel amaçlı ve temel sistem bilgilerini kapsar. Bunlar, daha fazla araştırma için iyi bir başlangıç noktasıdır.

### <a name="uptime"></a>hizmet

```
$ uptime
 19:32:33 up 17 days, 12:36,  0 users,  load average: 0.21, 0.77, 0.69
```

çalışma zamanı, sistem çalışma süresi ve 1, 5 ve 15 dakikalık yükün ortalamasını sağlar. Bu yükün yaklaşık olarak iş yapan iş parçacıklarında karşılık gelir veya kesintisiz çalışmanın tamamlanmasını bekler. Mutlak olarak bu sayıların yorumlanması zor olabilir, ancak yararlı bilgileri bize bildirebilecekleri zaman içinde ölçülür:

- 1 dakikalık ortalama > 5 dakikalık ortalama yükün artması anlamına gelir.
- 1 dakikalık ortalama < 5 dakikalık ortalama yükün azaltılması anlamına gelir.

çalışma süresi Ayrıca bilgilerin neden kullanılabilir durumda olduğunu da gösterebilir: sorun, kullanıcının makineye erişebilmesi için kendi başına veya bir yeniden başlatma ile çözümlenebilir.

Yük ortalamaları, kullanılabilir CPU iş parçacığı sayısından daha yüksek bir performans sorunu olduğunu gösterebilir.

### <a name="dmesg"></a>dmesg

```
$ dmesg | tail 
$ dmesg --level=err | tail
```

dmesg, çekirdek arabelleğini döker. Oomkıll gibi olaylar çekirdek arabelleğine bir giriş ekleyin. Dmesg günlüklerinde Oomkıll veya diğer kaynak tükenme iletilerinin bulunması bir sorunun güçlü göstergesidir.

### <a name="top"></a>Sayfanın Üstü

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

`top` geçerli sistem durumuna ilişkin kapsamlı bir genel bakış sağlar. Üst bilgiler bazı yararlı toplu bilgiler sağlar:

- görevlerin durumu: çalışıyor, Uyuyan, durduruldu.
- Bu durumda çoğunlukla boşta kalma süresini gösteren CPU kullanımı.
- Toplam, ücretsiz ve kullanılan sistem belleği.

`top` kısa süreli süreçler kaçırmayabilir; `htop` ve `atop` gibi alternatifler, bu eksiklerin bazılarını düzeltirken benzer arabirimler sağlar.

## <a name="cpu"></a>CPU

Bu araçlar CPU kullanım bilgilerini sağlar. Bu özellikle, desenlerin kolayca spot bir şekilde olacağı, sıralı çıktı ile kullanışlıdır.

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

`mpstat`, benzer CPU bilgilerini üste yazdırır, ancak CPU iş parçacığı tarafından bölünür. Tüm çekirdekleri aynı anda görmek, yüksek oranda kullanılabilir CPU kullanımını algılamak için faydalı olabilir. Örneğin, tek bir iş parçacıklı uygulama bir çekirdekli %100 kullanım aşamasında bir çekirdek kullanır. Bu sorun, sistemdeki tüm CPU 'Larda Toplandığınızda daha zor olabilir.

### <a name="vmstat"></a>vmstat

```
$ vmstat [interval]
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 43300372 545716 19691456    0    0     3    50    3    3  2  1 95  1  0
```

`vmstat` benzer bilgiler `mpstat` ve `top`, CPU (r sütunu) üzerinde bekleyen işlem sayısını, bellek istatistiklerini ve her iş durumunda harcanan CPU süresinin yüzdesini sağlar.

## <a name="memory"></a>Bellek

Bellek çok önemli ve takip etmek için çok daha kolay ve kaynak. Bazı araçlar `vmstat`gibi hem CPU hem de belleği rapor edebilir. Ancak `free` gibi araçlar hızlı hata ayıklama için yine de yararlı olabilir.

### <a name="free"></a>ücretsiz

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:          64403        2338       42485           1       19579       61223
Swap:             0           0           0
```

`free`, toplam bellek ve kullanılan ve boş bellek hakkındaki temel bilgileri gösterir. `vmstat`, temel bellek analizinde, sıralı çıkış sağlama yeteneği nedeniyle bile daha kullanışlı olabilir.

## <a name="disk"></a>Disk

Bu araçların disk ıOPS 'si, bekleme sıraları ve toplam verimlilik ölçümüdür. 

### <a name="iostat"></a>Iostat

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

`iostat` disk kullanımı hakkında ayrıntılı öngörüler sağlar. Bu çağrı `-y`, genişletilmiş istatistikler için `-x` geçirir, önyüklemeden bu yana ilk çıktı yazdırma sistem ortalamaları `1 1` ve tek bir çıkış bloğundan sonra sona ermek üzere, 1 saniyelik bir Aralık istiyoruz 

`iostat` birçok yararlı istatistiği kullanıma sunar:

- `r/s` ve `w/s`, saniye başına okuma ve yazma işlemleri. Bu değerlerin toplamı ıOPS 'dir.
- `rkB/s` ve `wkB/s`, saniye başına okunan/yazılan kilobayt. Bu değerlerin toplamı üretilen iş sayısıdır.
- `await`, sıraya alınan istekler için milisaniye cinsinden ortalama ıowaıt süresi.
- `avgqu-sz`, belirtilen Aralık üzerinde ortalama sıra boyutudur.

Bir Azure VM 'de:

- tek bir blok cihaz için `r/s` ve `w/s` toplamı, diskin SKU sınırlarını aşamaz.
- tek bir blok cihaz için `rkB/s` ve `wkB/s` toplamı, bu diskin SKU sınırları aşmayabilir
- tüm blok cihazların `r/s` ve `w/s` toplamı, VM SKU 'sunun sınırlarını aşamaz.
- tüm blok cihazları için `rkB/s` ve ' wkB/sn toplamı, VM SKU 'sunun sınırlarını aşamaz.

İşletim sistemi diskinin, kapasitesine karşılık gelen en küçük SKU 'nun yönetilen diski olarak sayıldığını unutmayın. Örneğin, bir 1024GB işletim sistemi diski bir P30 diskine karşılık gelir. Kısa ömürlü işletim sistemi disklerinin ve geçici disklerin ayrı disk limitleri yoktur; Bunlar yalnızca tam VM limitleriyle sınırlıdır.

Await veya avgqu-SZ ' y i sıfır olmayan değerleri de GÇ çakışması 'nın iyi göstergeleri.

## <a name="network"></a>Ağ

Bu araçlar aktarım hızı, iletim arızaları ve kullanım gibi ağ istatistiklerini ölçer. Daha derin analiz, sıkışıklık ve bırakılan paketlere ilişkin ayrıntılı TCP istatistikleri sunabilir.

### <a name="sar"></a>b

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

`sar`, çok çeşitli analizler için güçlü bir araçtır. Bu örnekte ağ istatistiklerini ölçme özelliği kullanılırken, CPU ve bellek tüketimini ölçmek için eşit derecede güçlüdür. Bu örnek, cihaz tarafından ağ aktarım hızını görüntüleyen `DEV` (ağ aygıtı) anahtar sözcüğünü belirtmek için `-n` bayrağıyla `sar` çağırır.

- `rxKb/s` ve `txKb/s` toplamı, belirli bir cihaz için toplam aktarım hızına sahiptir. Bu değer, sağlanan Azure NIC 'in sınırını aştığında, makinedeki iş yükleri daha fazla ağ gecikmesi yaşar.
- `%ifutil` belirli bir cihaz için kullanımı ölçer. Bu değer %100 yaklaşırsa, iş yükleri artan ağ gecikmesi ile karşılaşacaktır.

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

Bu `sar` çağrısı, TCP bağlantılarını incelemek için `TCP,ETCP` anahtar sözcüklerini kullanır. Son satırın üçüncü sütunu, "retrans", saniye başına TCP yeniden iletim sayısıdır. Bu alan için yüksek değerler, güvenilir olmayan bir ağ bağlantısını gösterir. İlk ve üçüncü satırlarda, "etkin" yerel cihazdan kaynaklanan bir bağlantı anlamına gelir, ancak "uzak" gelen bir bağlantıyı gösterir.  Azure 'daki yaygın bir sorun, `sar` algılamaya yardımcı olabilecek SNAT bağlantı noktası tükenmesi 'dir. Bu sorun yüksek oranda giden, yerel olarak başlatılmış bir TCP bağlantısı nedeniyle, SNAT bağlantı noktası tükenmesi yüksek "etkin" değerler olarak bildirimde bulunur.

`sar` bir Aralık aldığında, kayan çıktıyı yazdırır ve sonra, çağrının ortalama sonucunu içeren son çıktı satırlarını yazdırır.

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

`netstat`, burada Özet çıktı ile çağrılan çok çeşitli ağ istatistiklerini introspect olabilir. Soruna bağlı olarak burada çok sayıda yararlı alan vardır. TCP bölümündeki bir faydalı alan "başarısız bağlantı denemeleri" olur. Bu, SNAT bağlantı noktası tükenmesi veya giden bağlantılar oluşturma sorunları hakkında bir gösterge olabilir. Yüksek oranda yeniden aktarılan parçaların (TCP bölümünde de) paket tesliminde sorunlar olduğunu gösterebilir. 
