---
title: Azure VM'ler için TCP/IP performans alamı | Microsoft Dokümanlar
description: Çeşitli ortak TCP/IP performans atonlama tekniklerini ve bunların Azure VM'leriyle olan ilişkilerini öğrenin.
services: virtual-network
documentationcenter: na
author: rimayber
manager: paragk
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: rimayber
ms.reviewer: dgoddard, stegag, steveesp, minale, btalb, prachank
ms.openlocfilehash: bb23484903ac3ce129c6e7a7a27e0765c227fb1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68297778"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>Azure sanal makineleri için TCP/IP performans ayarlaması

Bu makalede, ortak TCP/IP performans aetme teknikleri ve bunları Azure'da çalışan sanal makineler için kullandığınızda göz önünde bulundurulması gereken bazı şeyler anlatılmaktadır. Bu teknikler temel bir bakış sağlayacak ve nasıl ayarlanabilir keşfetmek.

## <a name="common-tcpip-tuning-techniques"></a>Yaygın TCP/IP atoklama teknikleri

### <a name="mtu-fragmentation-and-large-send-offload"></a>MTU, parçalanma ve büyük gönderme boşaltma

#### <a name="mtu"></a>Mtu

Maksimum iletim birimi (MTU), bir ağ arabirimi üzerinden gönderilebilen baytcinsinden belirtilen en büyük boyut çerçevesi (paket) dir. MTU yapılandırılabilir bir ayardır. Azure VM'lerde kullanılan varsayılan MTU ve genel olarak çoğu ağ aygıtında varsayılan ayar 1.500 bayttır.

#### <a name="fragmentation"></a>Parçalanma

Parçalanma, ağ arabiriminin MTU'yu aşan bir paket gönderildiğinde oluşur. TCP/IP yığını paketi arabirimin MTU'suna uygun daha küçük parçalara (parçalara) ayırır. Parçalanma IP katmanında gerçekleşir ve temel protokolden (TCP gibi) bağımsızdır. 2.000 baytlık bir paket 1.500 MTU'luk bir ağ arabirimi üzerinden gönderildiğinde, paket 1.500 baytlık bir pakete ve bir 500 bayte pakete bölünür.

Kaynak ve hedef arasındaki yoldaki ağ aygıtları, MTU'yu aşan paketleri bırakabilir veya paketi daha küçük parçalara ayırabilir.

#### <a name="the-dont-fragment-bit-in-an-ip-packet"></a>IP paketinde Parçala

Don't Fragment (DF) biti IP protokolü başlığındaki bir bayraktır. DF biti, gönderen ve alıcı arasındaki yolüzerindeki ağ aygıtların paketi parçalamaması gerektiğini gösterir. Bu bit birçok nedenden dolayı ayarlanabilir. (Bir örnek için bu makalenin "Yol MTU Bulma" bölümüne bakın.) Bir ağ aygıtı Don't Fragment bit kümesine sahip bir paket aldığında ve bu paket aygıtın arabirimi MTU'yu aştığında, standart davranış aygıtın paketi bırakmasıdır. Aygıt, paketin özgün kaynağına gerekli bir ICMP Parçalanma iletisi gönderir.

#### <a name="performance-implications-of-fragmentation"></a>Parçalanmanın performans etkileri

Parçalanma olumsuz performans etkileri olabilir. Performans üzerindeki etkisinin başlıca nedenlerinden biri, paketlerin parçalanması ve yeniden biraraya getirilmesinin CPU/bellek etkisidir. Bir ağ aygıtının bir paketi parçalaması gerektiğinde, parçalanma gerçekleştirmek için CPU/bellek kaynaklarını ayırması gerekir.

Aynı şey paket yeniden biraraya getirildiğinde olur. Ağ aygıtı, tüm parçaları alınana kadar depolamalı, böylece bunları orijinal pakete yeniden monte edebilir. Bu parçalanma ve yeniden birleştirme işlemi de gecikmeye neden olabilir.

Parçalanmanın diğer olası negatif performans iması, parçalanmış paketlerin sıra dışı gelebileceğidir. Paketler sıradışı alındığı zaman, bazı ağ aygıtları bunları bırakabilir. Bu durumda, tüm paket yeniden iletilmelidir.

Parçalar genellikle ağ güvenlik duvarları gibi güvenlik aygıtları tarafından veya bir ağ aygıtının alma arabellekleri tükendiğinde bırakılır. Bir ağ aygıtının alma arabellekleri tükendiğinde, bir ağ aygıtı parçalanmış bir paketi yeniden biraraya getirmeye çalışır, ancak paketi depolamak ve yeniden varsamak için kaynaklara sahip değildir.

Parçalanma negatif bir işlem olarak görülebilir, ancak internet üzerinden farklı ağları bağlarken parçalanma desteği gereklidir.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>MTU'nun değiştirilmesinin yararları ve sonuçları

Genel olarak konuşursak, MTU'yu artırarak daha verimli bir ağ oluşturabilirsiniz. Aktarılan her paket, özgün pakete eklenen üstbilgi bilgilerine sahiptir. Parçalanma daha fazla paket oluşturduğunda, daha fazla üstbilgi yükü vardır ve bu da ağı daha az verimli hale getirir.

Bir örneği aşağıda verilmiştir. Ethernet üstbilgi boyutu 14 bayt artı çerçeve tutarlılığı sağlamak için 4 bayt kare kontrol sırasıdır. Bir 2.000 baytlık paket gönderilirse, ağa 18 bayt Ethernet yükü eklenir. Paket 1.500 baytlık bir paket ve 500 baytlık bir pakete bölünüyorsa, her pakette toplam 36 bayt olmak üzere 18 bayt Ethernet başlığı olacaktır.

MTU'yu artırmanın mutlaka daha verimli bir ağ oluşturmayacağını unutmayın. Bir uygulama yalnızca 500 baytlık paket gönderirse, MTU 1.500 bayt veya 9.000 bayt olsun aynı üstbilgi yükü bulunur. Ağ, yalnızca MTU'dan etkilenen daha büyük paket boyutları kullanırsa daha verimli hale gelir.

#### <a name="azure-and-vm-mtu"></a>Azure ve VM MTU

Azure VM'ler için varsayılan MTU 1.500 bayttır. Azure Sanal Ağ yığını, bir paketi 1.400 bayt olarak parçalamaya çalışır.

Sanal Ağ yığınının, VM'lerin MTU'su 1.500 olmasına rağmen paketleri 1.400 bayt olarak parçaladığı için doğal olarak verimsiz olmadığını unutmayın. Ağ paketlerinin büyük bir yüzdesi 1.400 veya 1.500 bayttan çok daha küçüktür.

#### <a name="azure-and-fragmentation"></a>Azure ve parçalanma

Sanal Ağ yığını, orijinal parçalanmış sıralarına gelmeyen parçalanmış paketleri "sıra dışı parçalar"ı bırakacak şekilde ayarlanır. Bu paketler esas olarak Kasım 2018'de açıklanan FragmentSmack adlı ağ güvenliği açığı nedeniyle bırakılır.

FragmentSmack, Linux çekirdeğinin parçalanmış IPv4 ve IPv6 paketlerinin yeniden biraraya gelip montajını ele alış şeklindeki bir kusurdur. Uzak bir saldırgan, bu kusuru, cpu'nun artmasına ve hedef sistemde hizmet reddine yol açabilecek pahalı parça yeniden montaj işlemlerini tetiklemek için kullanabilir.

#### <a name="tune-the-mtu"></a>MTU'yu ayarlayın

Azure VM MTU'su, diğer işletim sisteminde olduğu gibi yapılandırabilirsiniz. Ancak, bir MTU yapılandırırken azure'da meydana gelen parçalanmayı göz önünde bulundurmalısınız.

Müşterilerimizi VM MTUs'u artırmaya teşvik etmiyoruz. Bu tartışma, Azure'un MTU'yu nasıl uyguladığı ve parçalanma yı nasıl gerçekleştirdiğinin ayrıntılarını açıklamak içindir.

> [!IMPORTANT]
>MTU'nun artırılmasının performansı artırabileceği bilinmemektedir ve uygulama performansı üzerinde olumsuz bir etkisi olabilir.
>
>

#### <a name="large-send-offload"></a>Büyük gönderme yükü

Büyük gönderme boşaltma (LSO), paketlerin segmentasyonunu Ethernet bağdaştırıcısına boşaltarak ağ performansını artırabilir. LSO etkinleştirildiğinde, TCP/IP yığını büyük bir TCP paketi oluşturur ve iletmeden önce segmentasyon için Ethernet bağdaştırıcısına gönderir. LSO'nun yararı, CPU'yu paketleri MTU'ya uygun boyutlarda bölmekten kurtarabilmesi ve bu işlemi donanımda gerçekleştirilen Ethernet arabirimine boşaltmasıdır. LSO'nun yararları hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso)

LSO etkinleştirildiğinde, Azure müşterileri paket yakalama ları gerçekleştirirken büyük çerçeve boyutları görebilir. Bu büyük çerçeve boyutları, bazı müşterilerin parçalanma oluştuğunu veya büyük bir MTU'nun kullanılmadığını düşünmelerine neden olabilir. Ethernet bağdaştırıcısı LSO ile daha büyük bir TCP paketi oluşturmak için TCP/IP yığınına daha büyük bir maksimum segment boyutunun (MSS) reklamını yapabilir. Tüm bu segmentsiz çerçeve daha sonra Ethernet bağdaştırıcısına iletilir ve VM üzerinde gerçekleştirilen bir paket yakalama görünür olacaktır. Ancak Ethernet adaptörü MTU'ya göre paket Ethernet adaptörü tarafından çok daha küçük karelere bölünecek.

### <a name="tcp-mss-window-scaling-and-pmtud"></a>TCP MSS pencere ölçekleme ve PMTUD

#### <a name="tcp-maximum-segment-size"></a>TCP maksimum segment boyutu

TCP maksimum segment boyutu (MSS), TCP paketlerinin parçalanmasını önleyen TCP segmentlerinin boyutunu sınırlayan bir ayardır. İşletim sistemleri genellikle MSS ayarlamak için bu formülü kullanır:

`MSS = MTU - (IP header size + TCP header size)`

IP üstbilgisi ve TCP üstbilgisi her biri 20 bayt veya toplam 40 bayttır. Yani 1.500 MTU'luk bir arayüz 1.460 MSS'ye sahip olacak. Ama MSS yapılandırılabilir.

Bu ayar, bir kaynak ve hedef arasında bir TCP oturumu ayarlandığında TCP üç yönlü el sıkışmasında kabul edilir. Her iki taraf da bir MSS değeri gönderir ve ikisinin alt kısmı TCP bağlantısı için kullanılır.

KAYNAK ve hedefin MTÜ'lerinin MSS değerini belirleyen tek etken olmadığını unutmayın. Azure VPN Ağ Geçidi de dahil olmak üzere VPN ağ geçitleri gibi ara ağ aygıtları, en iyi ağ performansını sağlamak için MTU'yu kaynaktan ve hedeften bağımsız olarak ayarlayabilir.

#### <a name="path-mtu-discovery"></a>Yol MTU Discovery

MSS görüşüldü, ancak kullanılabilecek gerçek MSS'yi göstermeyebilir. Bunun nedeni, kaynak ve hedef arasındaki yoldaki diğer ağ aygıtları kaynak ve hedeften daha düşük bir MTU değerine sahip olabilir. Bu durumda, MTU'su paketten daha küçük olan aygıt paketi düşürür. Cihaz, MTU'yu içeren bir ICMP Parçalanma Gerekli (Tip 3, Kod 4) iletisini geri gönderir. Bu ICMP iletisi, kaynak ana bilgisayar yolunu NTU'yu uygun şekilde azaltmasına olanak tanır. İşleme Path MTU Discovery (PMTUD) adı verilir.

PMTUD işlemi verimsizdir ve ağ performansını etkiler. Bir ağ yolunun MTU'sunu aşan paketler gönderildiğinde, paketlerin daha düşük bir MSS ile yeniden gönderilmesi gerekir. Gönderen ICMP Parçalanma Gerekli iletialmaz, belki de yolda bir ağ güvenlik duvarı nedeniyle (genellikle *PMTUD karadelik*olarak anılacaktır), gönderen MSS düşürmek için gerekenleri bilmiyor ve sürekli paketi yeniden iletir. Bu nedenle Azure VM MTU'yu artırmanızı önermiyoruz.

#### <a name="vpn-and-mtu"></a>VPN ve MTU

Kapsülleme gerçekleştiren VM'ler kullanıyorsanız (IPsec VPN'leri gibi), paket boyutu ve MTU ile ilgili bazı ek hususlar vardır. VPN'ler paketlere daha fazla üstbilgi ekler, bu da paket boyutunu artırır ve daha küçük bir MSS gerektirir.

Azure için TCP MSS bağlamayı 1.350 bayt ve tünel arabirimi MTU'su 1.400 olarak ayarlamanızı öneririz. Daha fazla bilgi için [VPN aygıtları ve IPSec/IKE parametreleri sayfasına](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)bakın.

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Gecikme süresi, gidiş-dönüş süresi ve TCP pencere ölçekleme

#### <a name="latency-and-round-trip-time"></a>Gecikme süresi ve gidiş-dönüş süresi

Ağ gecikmesi, fiber optik ağ üzerinden ışık hızına bağlıdır. TCP'nin ağ çıktısı da iki ağ aygıtı arasındaki gidiş-dönüş süresi (RTT) tarafından etkin bir şekilde yönetilir.

| | | | |
|-|-|-|-|
|**Rota**|**Uzaklık**|**Tek yönlü zaman**|**RTT**|
|New York'dan San Francisco'ya|4.148 km|21 ms|42 ms|
|New York'dan Londra'ya|5.585 km|28 ms|56 ms|
|New York'dan Sidney'e|15.993 km|80 ms|160 ms|

Bu tablo, iki konum arasındaki düz çizgi mesafesini gösterir. Ağlarda mesafe genellikle düz çizgi uzaklığından daha uzundur. Işık hızına göre yönetilen minimum RTT'yi hesaplamak için basit bir formül aşağıda verilmiştir:

`minimum RTT = 2 * (Distance in kilometers / Speed of propagation)`

Yayılma hızı için 200 kullanabilirsiniz. Bu mesafe, metre cinsinden, ışık 1 milisaniye içinde hareket ediyor.

New York'u San Francisco'ya örnek olarak verelim. Düz hat mesafesi 4.148 km'dir. Bu değeri denkleme takarak, aşağıdakileri elde ediyoruz:

`Minimum RTT = 2 * (4,148 / 200)`

Denklemin çıktısı milisaniye cinsindendir.

En iyi ağ performansını elde etmek istiyorsanız, mantıksal seçenek aralarında en kısa mesafeye sahip hedefleri seçmektir. Ayrıca, sanal ağınızı trafik yolunu en iyi duruma getirmek ve gecikme süresini azaltmak için tasarlamanız gerekir. Daha fazla bilgi için bu makalenin "Ağ tasarımı konuları" bölümüne bakın.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>TCP üzerinde gecikme süresi ve gidiş-dönüş zaman etkileri

Gidiş-dönüş süresi, maksimum TCP verimi üzerinde doğrudan bir etkiye sahiptir. TCP *protokolünde, pencere boyutu,* gönderenin alıcıdan onay alması gerekmeden önce Bir TCP bağlantısı üzerinden gönderilebilen maksimum trafik miktarıdır. TCP MSS 1.460 olarak ayarlanırsa ve TCP pencere boyutu 65.535 olarak ayarlanmışsa, gönderen alıcıdan onay almadan önce 45 paket gönderebilir. Gönderen onay alamazsa, verileri yeniden iletir. Formül şudur:

`TCP window size / TCP MSS = packets sent`

Bu örnekte, 65.535 / 1.460 45'e yuvarlanır.

RtT'nin TCP veri sini etkilemesine neden olan, verilerin güvenilir bir şekilde teslim edilmesini sağlayan bir mekanizma olan bu "onay bekleme" durumudur. Gönderen onay için ne kadar uzun süre beklerse, daha fazla veri göndermeden önce beklemesi o kadar uzun sürer.

Tek bir TCP bağlantısının maksimum iş alım ını hesaplamanın formülü aşağıda verilmiştir:

`Window size / (RTT latency in milliseconds / 1,000) = maximum bytes/second`

Bu tablo, tek bir TCP bağlantısının saniye başına maksimum megabayt///saniye iş kısmını gösterir. (Okunabilirlik için, megabayt ölçü birimi için kullanılır.)

| | | | |
|-|-|-|-|
|**TCP pencere boyutu (bayt)**|**RTT gecikmesi (ms)**|**Maksimum megabayt/saniye verimi**|**Maksimum megabit/saniye verimi**|
|65,535|1|65.54|524.29|
|65,535|30|2.18|17.48|
|65,535|60|1.09|8.74|
|65,535|90|.73|5.83|
|65,535|120|.55|4.37|

Paketler kaybolursa, gönderen zaten gönderdiği verileri yeniden aktarırken TCP bağlantısının maksimum iş verimi azalır.

#### <a name="tcp-window-scaling"></a>TCP pencere ölçekleme

TCP pencere ölçekleme, bir onay gerekmeden önce daha fazla veri gönderilmesine izin vermek için TCP pencere boyutunu dinamik olarak artıran bir tekniktir. Önceki örnekte, bir onay gerekmeden önce 45 paket gönderilir. Bir bildirim gerekli olmadan önce gönderilebilen paket sayısını artırırsanız, gönderenin onay beklediği kaç katını azaltmış olursunuz ve bu da TCP maksimum iş verisini artırır.

Bu tablo bu ilişkileri göstermektedir:

| | | | |
|-|-|-|-|
|**TCP pencere boyutu (bayt)**|**RTT gecikmesi (ms)**|**Maksimum megabayt/saniye verimi**|**Maksimum megabit/saniye verimi**|
|65,535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

Ancak TCP pencere boyutu için TCP üstbilgi değeri yalnızca 2 bayt uzunluğundadır, bu da alma penceresiiçin en büyük değerin 65.535 olduğu anlamına gelir. En büyük pencere boyutunu artırmak için bir TCP pencere ölçeği faktörü tanıtıldı.

Ölçek faktörü aynı zamanda bir işletim sisteminde yapılandırabileceğiniz bir ayardır. Ölçek faktörleri kullanarak TCP pencere boyutunu hesaplamanın formülü aşağıda verilmiştir:

`TCP window size = TCP window size in bytes \* (2^scale factor)`

Burada 3 ve 65.535 bir pencere boyutu bir pencere ölçeği faktörü için hesaplama' s:

`65,535 \* (2^3) = 262,140 bytes`

TCP pencere boyutunda 14 sonuç (izin verilen maksimum ofset) 14 sonuçölçeği. TCP pencere boyutu 1.073.725.440 bayt (8,5 gigabat) olacaktır.

#### <a name="support-for-tcp-window-scaling"></a>TCP pencere ölçekleme desteği

Windows, farklı bağlantı türleri için farklı ölçekleme faktörleri ayarlayabilir. (Bağlantı sınıfları veri merkezi, internet vb.'yi içerir.) Pencere ölçekleme bağlantı türünü görüntülemek için `Get-NetTCPConnection` PowerShell komutunu kullanırsınız:

```powershell
Get-NetTCPConnection
```

`Get-NetTCPSetting` Her sınıfın değerlerini görüntülemek için PowerShell komutunu kullanabilirsiniz:

```powershell
Get-NetTCPSetting
```

PowerShell komutunu kullanarak Windows'daki ilk TCP pencere boyutunu `Set-NetTCPSetting` ve TCP ölçekleme faktörlerini ayarlayabilirsiniz. Daha fazla bilgi için Bkz. [Set-NetTCPSetting.](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

```powershell
Set-NetTCPSetting
```

Bunlar için `AutoTuningLevel`etkili TCP ayarları şunlardır:

| | | | |
|-|-|-|-|
|**AutoTuningLevel**|**Ölçekleme faktörü**|**Ölçekleme çarpanı**|**Maksimum<br/>pencere boyutunu hesaplamak için formül**|
|Devre dışı|None|None|Pencere boyutu|
|Kısıtlı|4|2^4|Pencere boyutu * (2^4)|
|Son derece kısıtlı|2|2^2|Pencere boyutu * (2^2)|
|Normal|8|2^8|Pencere boyutu * (2^8)|
|Deneysel|14|2^14|Pencere boyutu * (2^14)|

Bu ayarlar TCP performansını etkileme olasılığı en yüksek olanlardır, ancak Azure'un kontrolü dışında internet genelinde ki diğer birçok faktörün TCP performansını da etkileyebileceğini unutmayın.

#### <a name="increase-mtu-size"></a>MTU boyutunu artırın

Daha büyük bir MTU daha büyük bir MSS anlamına geldiği için, MTU'yu artırmanın TCP performansını artırıp artıramayacağını merak edebilirsiniz. Muhtemelen hayır. Sadece TCP trafiğinin ötesinde paket boyutu artıları ve eksileri vardır. Daha önce de belirtildiği gibi, TCP iş verme performansını etkileyen en önemli faktörler TCP pencere boyutu, paket kaybı ve RTT'dir.

> [!IMPORTANT]
> Azure müşterilerinin sanal makinelerde varsayılan MTU değerini değiştirmelerini önermiyoruz.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Hızlandırılmış ağ ve yan ölçekleme alma

#### <a name="accelerated-networking"></a>Hızlandırılmış ağ iletişimi

Sanal makine ağı işlevleri tarihsel olarak hem konuk VM hem de hipervizör/ana bilgisayarda CPU yoğun olmuştur. Ana bilgisayardan geçen her paket, tüm sanal ağ kapsülleme ve decapsulation dahil olmak üzere ana bilgisayar CPU tarafından yazılım olarak işlenir. Yani ana bilgisayardan ne kadar çok trafik geçerse, CPU yükü de o kadar yüksek olur. Ana bilgisayar CPU'su diğer işlemlerle meşgulse, bu ağ iş buzunu ve gecikmeyi de etkiler. Azure, hızlandırılmış ağ ile bu sorunu gideriyor.

Hızlandırılmış ağ, Azure'un şirket içi programlanabilir donanımı ve SR-IOV gibi teknolojiler aracılığıyla tutarlı ultralow ağ gecikmesi sağlar. Hızlandırılmış ağ, Azure yazılım tanımlı ağ yığınının çoğunu CPU'lardan çıkarıp FPGA tabanlı SmartNIC'lere taşır. Bu değişiklik, son kullanıcı uygulamalarının bilgi işlem döngülerini geri almasını sağlar, bu da VM'ye daha az yük bindirerek gecikme deki gerginliği ve tutarsızlığı azaltır. Başka bir deyişle, performans daha belirleyici olabilir.

Hızlandırılmış ağ, konuk VM'nin ana bilgisayarı atlayabilmesine ve doğrudan bir ana bilgisayar SmartNIC ile bir veri yolu oluşturmasına izin vererek performansı artırır. Hızlandırılmış ağ la ilgili bazı avantajlar şunlardır:

- **Saniyebaşına daha düşük gecikme süresi / daha yüksek paketler (pps)**: Sanal anahtarı veri yolundan kaldırmak, paketlerin ilke işleme için ana bilgisayarda harcadığı zamanı ortadan kaldırır ve VM'de işlenebilen paket sayısını artırır.

- **Azaltılmış jitter**: Sanal anahtar işleme, uygulanması gereken ilke miktarına ve işlemeyi yapan CPU'nun iş yüküne bağlıdır. İlke zorlamanın donanıma indirilmesi, paketleri doğrudan VM'ye teslim ederek bu değişkenliği ortadan kaldırır, ana bilgisayara-VM iletişimini ve tüm yazılım kesintilerini ve bağlam anahtarlarını ortadan kaldırır.

- **Azalmış CPU kullanımı**: Ana bilgisayardaki sanal anahtarı atlayarak ağ trafiğini işlemek için daha az CPU kullanımına yol açar.

Hızlandırılmış ağ kullanımını kullanmak için, bunu geçerli her VM'de açıkça etkinleştirmeniz gerekir. Bkz. Talimatlar için [Hızlandırılmış Ağ ile bir Linux sanal makine oluşturun.](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

#### <a name="receive-side-scaling"></a>Yan ölçekleme alma

Receive yan ölçekleme (RSS), çok işlemcili bir sistemde alma işlemini birden çok CPU'ya dağıtarak ağ trafiğinin alınmasını daha verimli bir şekilde dağıtan bir ağ sürücüsü teknolojisidir. Basit bir ifadeyle, RSS bir sistem yerine tüm kullanılabilir CPU'lar kullandığı ndan daha fazla alınan trafiği işlemek için izin verir. RSS hakkında daha teknik bir tartışma için yan [ölçekleme almak için Giriş bölümüne](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling)bakın.

VM'de hızlandırılmış ağ etkinleştirildiğinde en iyi performansı elde etmek için RSS'yi etkinleştirmeniz gerekir. RSS, hızlandırılmış ağ kullanmayan VM'ler üzerinde de avantajlar sağlayabilir. RSS'nin etkin olup olmadığını niçin belirleyeceğine ve nasıl etkinleştirilene genel bir bakış için Azure [sanal makineleri için ağ bilgisini optimize etme bölümüne](https://aka.ms/FastVM)bakın.

### <a name="tcp-time_wait-and-time_wait-assassination"></a>TCP TIME_WAIT ve TIME_WAIT suikast

TCP TIME_WAIT ağ ve uygulama performansını etkileyen başka bir ortak ayardır. TCP'nin normal çalışması sırasında, istemci olarak veya sunucu olarak (Kaynak IP:Kaynak Bağlantı Noktası + Hedef IP:Hedef Bağlantı Noktası) birçok soketi açıp kapatan meşgul VM'lerde, belirli bir soket uzun süre TIME_WAIT durumda son bulabilir. TIME_WAIT durumu, herhangi bir ek verinin kapatmadan önce bir sokete teslim edilmesine izin vermek içindir. Bu nedenle TCP/IP yığınları genellikle istemcinin TCP SYN paketini sessizce bırakarak bir soketin yeniden kullanılmasını engeller.

Bir soketin TIME_WAIT olan süre yapılandırılabilir. 30 saniye ile 240 saniye arasında değişebilir. Soketler sınırlı bir kaynaktır ve herhangi bir zamanda kullanılabilecek soket sayısı yapılandırılabilir. (Kullanılabilir soket lerin sayısı genellikle yaklaşık 30.000'dir.) Kullanılabilir soketler tüketilirse veya istemciler ve sunucular TIME_WAIT ayarlarıyla eşleşmediğinde ve VM TIME_WAIT durumunda bir soketi yeniden kullanmaya çalışırsa, TCP SYN paketleri sessizce bırakıldığında yeni bağlantılar başarısız olur.

Giden soketler için bağlantı noktası aralığı nın değeri genellikle bir işletim sisteminin TCP/IP yığını içinde yapılandırılabilir. Aynı şey TCP TIME_WAIT ayarları ve soket yeniden için de geçerlidir. Bu sayıların değiştirilmesi ölçeklenebilirliği artırabilir. Ancak, duruma bağlı olarak, bu değişiklikler birlikte çalışabilirlik sorunlarına neden olabilir. Bu değerleri değiştirirseniz dikkatli olmalısınız.

Bu ölçekleme sınırlamasını gidermek için suikast TIME_WAIT kullanabilirsiniz. TIME_WAIT suikast, yeni bağlantının IP paketindeki sıra numarasının önceki bağlantıdaki son paketin sıra numarasını aşması gibi belirli durumlarda bir soketin yeniden kullanılmasını sağlar. Bu durumda, işletim sistemi yeni bağlantının kurulmasına izin verir (yeni SYN/ACK'yı kabul eder) ve TIME_WAIT durumundaki önceki bağlantıyı kapatmaya zorlar. Bu özellik Azure'daki Windows VM'lerde desteklenir. Diğer VM'lerde destek hakkında bilgi edinmek için işletim sistemi satıcısına danışın.

TCP TIME_WAIT ayarlarını ve kaynak bağlantı noktası aralığını yapılandırma hakkında bilgi edinmek [için ağ performansını artırmak için değiştirilebilen Ayarlar'a](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance)bakın.

## <a name="virtual-network-factors-that-can-affect-performance"></a>Performansı etkileyebilecek sanal ağ faktörleri

### <a name="vm-maximum-outbound-throughput"></a>VM maksimum giden verim

Azure, her biri farklı performans özellikleri karışımına sahip çeşitli VM boyutları ve türleri sağlar. Bu özelliklerden biri, saniyede megabit (Mbps) cinsinden ölçülen ağ iş birliği (veya bant genişliği) dir. Sanal makineler paylaşılan donanımda barındırıldığı için, ağ kapasitesinin aynı donanımı kullanan sanal makineler arasında adil bir şekilde paylaşılması gerekir. Daha büyük sanal makineler, daha küçük sanal makinelere göre daha fazla bant genişliği ayrılır.

Her sanal makineye ayrılan ağ bant genişliği, sanal makineden çıkış (giden) trafik üzerinde ölçülür. Sanal makineden ayrılan tüm ağ trafiği, hedeften bağımsız olarak ayrılan sınıra doğru sayılır. Örneğin, sanal bir makinenin 1.000 Mbps sınırı varsa, bu sınır giden trafiğin aynı sanal ağdaki başka bir sanal makineye mi yoksa Azure dışında mı olduğunu da zedler.

Giriş doğrudan ölçülmez veya sınırlı değildir. Ancak, sanal bir makinenin gelen verileri işleme yeteneğini etkileyebilecek CPU ve depolama sınırları gibi başka etkenler de vardır.

Hızlandırılmış ağ, gecikme, iş sonu ve CPU kullanımı da dahil olmak üzere ağ performansını artırmak için tasarlanmıştır. Hızlandırılmış ağ, sanal makinenin iş bilgililiğini artırabilir, ancak bunu yalnızca sanal makinenin ayrılan bant genişliğine kadar yapabilir.

Azure sanal makinelerinin bağlı en az bir ağ arabirimi vardır. Birkaç tane olabilir. Sanal bir makineye ayrılan bant genişliği, makineye bağlı tüm ağ arabirimlerindeki tüm giden trafiğin toplamıdır. Başka bir deyişle, bant genişliği, makineye kaç ağ arabirimi bağlı olursa olsun, sanal makine başına olarak ayrılır.

Beklenen giden iş bölümü ve her VM boyutu tarafından desteklenen ağ arabirimlerinin sayısı [Azure'daki Windows sanal makineleri için Boyutlar'da](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json)ayrıntılı olarak açıklanmıştır. Maksimum iş elde etme **için, Genel amaç**gibi bir tür seçin ve ardından ortaya çıkan sayfadaki boyut serisiyle ilgili bölümü bulun (örneğin, "Dv2 serisi"). Her seri için, son sütunda ağ belirtimleri sağlayan bir tablo vardır, bu da "Max NIC' ler / Beklenen ağ bant genişliği (Mbps)" olarak adlandırılır.

İş verme sınırı sanal makine için geçerlidir. İş mesuliyonu aşağıdaki faktörlerden etkilenmez:

- **Ağ arabirimi sayısı**: Bant genişliği sınırı sanal makineden giden tüm trafiğin toplamı için geçerlidir.

- **Hızlandırılmış ağ :** Bu özellik yayımlanmış sınıra ulaşmada yararlı olsa da, sınırı değiştirmez.

- **Trafik hedefi**: Tüm varış noktaları giden sınıra doğru sayılır.

- **Protokol**: Tüm protokoller üzerindeki tüm giden trafik sınıra doğru sayılır.

Daha fazla bilgi için [Virtual machine network bant genişliğine](https://aka.ms/AzureBandwidth)bakın.

### <a name="internet-performance-considerations"></a>İnternet performansı yla ilgili hususlar

Bu makalede ele alındığı gibi, Internet'teki ve Azure denetimi dışındaki etkenler ağ performansını etkileyebilir. Bu faktörlerden bazıları şunlardır:

- **Gecikme süresi**: İki destinasyon arasındaki gidiş-dönüş süresi ara ağlardaki sorunlardan, "en kısa" mesafe yolunu olmayan trafikten ve en uygun olmayan akran yollarından etkilenebilir.

- **Paket kaybı**: Paket kaybı, ağ tıkanıklığı, fiziksel yol sorunları ve düşük performanslı ağ aygıtları nedeniyle olabilir.

- **MTU boyutu/Parçalanma**: Yol boyunca parçalanma, veri varışında veya sipariş dışında gelen paketlerde gecikmelere yol açabilir ve bu da paketlerin teslimini etkileyebilir.

Traceroute, kaynak aygıt ve hedef aygıt arasındaki her ağ yolu boyunca ağ performansı özelliklerini (paket kaybı ve gecikme sonu gibi) ölçmek için iyi bir araçtır.

### <a name="network-design-considerations"></a>Ağ tasarımı ile ilgili hususlar

Bu makalede daha önce tartışılan hususların yanı sıra, sanal bir ağın topolojisi ağın performansını etkileyebilir. Örneğin, trafiği tek hub'lı bir sanal ağa geri çeken hub ve kollu bir tasarım, genel ağ performansını etkileyecek ağ gecikmesini başlatacaktır.

Ağ trafiğinin geçtiği ağ aygıtlarının sayısı da genel gecikmeyi etkileyebilir. Örneğin, hub ve kollu bir tasarımda, trafik internete geçmeden önce bir ağ sanal cihaz dan ve hub sanal cihazdan geçiyorsa, ağ sanal cihazları gecikme ye neden olabilir.

### <a name="azure-regions-virtual-networks-and-latency"></a>Azure bölgeleri, sanal ağlar ve gecikme sonu

Azure bölgeleri, genel bir coğrafi bölgede bulunan birden çok veri merkezinden oluşur. Bu veri merkezleri fiziksel olarak yan yana olmayabilir. Bazı durumlarda 10 kilometre ye kadar ayrılırlar. Sanal ağ, Azure fiziksel veri merkezi ağının üstündeki mantıksal bir kaplamadır. Sanal ağ, veri merkezi içinde belirli bir ağ topolojisi anlamına gelmez.

Örneğin, aynı sanal ağ ve alt ağda bulunan iki VM farklı raflarda, satırlarda ve hatta veri merkezlerinde olabilir. Onlar fiber optik kablo ayakları veya fiber optik kablo kilometre ile ayrılabilir. Bu varyasyon, farklı VM'ler arasında değişken gecikme (birkaç milisaniye fark) getirebilir.

VM'lerin coğrafi yerleşimi ve iki VM arasında ortaya çıkan olası gecikme, kullanılabilirlik kümelerinin ve Kullanılabilirlik Bölgelerinin yapılandırmasından etkilenebilir. Ancak bir bölgedeki veri merkezleri arasındaki mesafe bölgeye özgüdür ve öncelikle bölgedeki veri merkezi topolojisi tarafından etkilenir.

### <a name="source-nat-port-exhaustion"></a>Kaynak NAT bağlantı noktası tükenmesi

Azure'daki bir dağıtım, ortak internette ve/veya genel IP alanında Azure dışındaki uç noktalarla iletişim kurabilir. Bir örnek giden bağlantı başlattığında, Azure özel IP adresini genel bir IP adresiyle dinamik olarak eşler. Azure bu eşlemi oluşturduktan sonra, giden kaynak akışının iade trafiği, akışın kaynaklandığı özel IP adresine de ulaşabilir.

Her giden bağlantı için Azure Yük Dengeleyicisinin bu eşlemayı belirli bir süre boyunca sürdürmesi gerekir. Azure'un çok kiracılı yapısıyla, her VM için her giden akış için bu eşlemi sürdürmek kaynak yoğun olabilir. Bu nedenle, Azure Sanal Ağı'nın yapılandırmasını temel alan ve ayarlanan sınırlar vardır. Veya daha doğrusu, bir Azure VM'nin belirli bir zamanda yalnızca belirli sayıda giden bağlantı kurabileceğini söylemek gerekirse. Bu sınırlara ulaşıldığında, VM daha fazla giden bağlantı yapamaz.

Ama bu davranış yapılandırılabilir. SNAT ve SNAT bağlantı noktası tükenmesi hakkında daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections)bakın.

## <a name="measure-network-performance-on-azure"></a>Azure'da ağ performansını ölçme

Bu makaledeki performans maksimumlarının bir kısmı, iki VM arasındaki ağ gecikmesi / gidiş-dönüş süresi (RTT) ile ilgilidir. Bu bölümde gecikme/RTT nasıl test edilebilmek ve TCP performansı ile VM ağ performansı nasıl test edilebiz gibi öneriler verilmektedir. Bu bölümde açıklanan teknikleri kullanarak daha önce tartışılan TCP/IP ve ağ değerlerini ayarlayabilir ve performans testi yapabilirsiniz. Gecikme süresi, MTU, MSS ve pencere boyutu değerlerini daha önce sağlanan hesaplamalara takabilir ve teorik maksimumları test sırasında gözlemlediğiniz gerçek değerlerle karşılaştırabilirsiniz.

### <a name="measure-round-trip-time-and-packet-loss"></a>Gidiş-dönüş süresini ve paket kaybını ölçme

TCP performansı büyük ölçüde RTT ve paket Kaybı'na dayanır. Windows ve Linux'ta bulunan PING programı RTT ve paket kaybını ölçmenin en kolay yolunu sağlar. PING çıktısı, kaynak ve hedef arasındaki minimum/maksimum/ortalama gecikmeyi gösterir. Ayrıca paket kaybı nı da gösterir. PING varsayılan olarak ICMP protokolünü kullanır. TCP RTT'yi test etmek için PsPing'i kullanabilirsiniz. Daha fazla bilgi için [PsPing'e](https://docs.microsoft.com/sysinternals/downloads/psping)bakın.

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>TCP bağlantısının gerçek iş kısmını ölçme

NTttcp, bir Linux veya Windows VM'nin TCP performansını test etmek için bir araçtır. Çeşitli TCP ayarlarını değiştirebilir ve nttcp kullanarak avantajları sınayabilirsiniz. Daha fazla bilgi için şu kaynaklara bakın:

- [Bant genişliği/İş-İş-İş testi (NTttcp)](https://aka.ms/TestNetworkThroughput)

- [NTttcp Yardımcı Programı](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Sanal bir makinenin gerçek bant genişliğini ölçme

IPerf adlı bir araç kullanarak farklı VM türlerinin, hızlandırılmış ağ kullanımının ve benzeri performanslarının test edilmesi. iPerf Linux ve Windows'da da kullanılabilir. iPerf, genel ağ iş parçacığı nın test ini test etmek için TCP veya UDP kullanabilir. iPerf TCP iş sonu testleri bu makalede tartışılan faktörlerden (gecikme ve RTT gibi) etkilenir. Yani udp sadece maksimum verim test etmek istiyorsanız daha iyi sonuçlar verebilir.

Daha fazla bilgi için şu makalelere bakın:

- [Sorun giderme Expressroute ağ performansı](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [Sanal ağa yönelik VPN aktarım hızını doğrulama](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Verimsiz TCP davranışlarını algılama

Paket yakalamalarda, Azure müşterileri ağ performansı sorunlarını gösterebilecek TCP paketleri (SACK, DUP ACK, RETRANSMIT ve FAST RETRANSMIT) ile tcp paketleri görebilir. Bu paketler özellikle paket kaybından kaynaklanan ağ verimsizliklerini gösterir. Ancak paket kaybına Azure performans sorunları neden olmak zorunda değildir. Performans sorunları, uygulama sorunları, işletim sistemi sorunları veya Azure platformuyla doğrudan ilişkili olmayan diğer sorunlardan kaynaklanabilir.

Ayrıca, bazı yeniden iletim ve yinelenen AK'ların ağda normal olduğunu unutmayın. TCP protokolleri güvenilir olacak şekilde oluşturulmuş. Bu TCP paketlerinin paket yakalamadaki kanıtları, aşırı olmadığı sürece sistemik bir ağ sorununu göstermez.

Yine de, bu paket türleri, bu makalenin diğer bölümlerinde tartışılan nedenlerle TCP iş veriminin maksimum performansı elde edilemediğinin göstergeleridir.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'ler için TCP/IP performans atoması hakkında bilgi edindiğinize göre, [sanal ağlar planlamak](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) veya [sanal ağları bağlama ve yapılandırma hakkında daha fazla bilgi edinmek](https://docs.microsoft.com/azure/virtual-network/)için diğer hususlar hakkında bilgi almak isteyebilirsiniz.
