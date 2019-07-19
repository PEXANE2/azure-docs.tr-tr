---
title: Azure Load Balancer sistem durumu araştırmalarını kullanarak hizmetiniz için yüksek kullanılabilirlik sağlayın
titlesuffix: Azure Load Balancer
description: Yük dengeleyicinin arkasında izlemek için sistem durumu araştırmaları kullanmayı öğrenin
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: allensu
ms.openlocfilehash: 75009530940a0cce7adb8469ead5f55f509a1faa
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275343"
---
# <a name="load-balancer-health-probes"></a>Load Balancer sistem durumu araştırmaları

Azure Load Balancer, Yük Dengeleme kuralları ile kullanım için sistem durumu araştırmaları sağlar.  Durum araştırma yapılandırması ve araştırma yanıtları, hangi arka uç havuzu örneklerinin yeni akışlar alacağını belirlenir. Arka uç örnek bir uygulama hatasını algılamak için sistem durumu araştırmaları kullanabilirsiniz. Ayrıca, bir sistem durumu araştırmasına özel bir yanıt oluşturabilir ve yük veya planlanan kapalı kalma süresini yönetmek için akış denetimi için sistem durumu araştırması ' ni kullanabilirsiniz. Durum araştırması başarısız olduğunda, yük dengeleyici sağlıksız ilgili örneğine yeni akışlar gönderme durdurur.

Durum araştırmaları birden çok protokolü destekler. Belirli bir protokolü desteklemek için belirli bir sistem durumu araştırmasının kullanılabilirliği Load Balancer SKU 'ya göre değişir.  Ayrıca, hizmetin davranışı Load Balancer SKU 'ya göre farklılık gösterir.

| | Standart SKU | Temel SKU |
| --- | --- | --- |
| [Araştırma türleri](#types) | TCP VE HTTP, HTTPS | TCP VE HTTP |
| [Davranışı araştırma](#probedown) | Tüm araştırmaları, tüm TCP akışları devam edin. | Tüm yoklamalar, tüm TCP akışları sona erer. | 

> [!IMPORTANT]
> Load Balancer sistem durumu araştırmaları, 168.63.129.16 IP adresinden kaynaklanacak ve örneğinizi işaretlemek için araştırmaların engellenmemelidir.  Gözden geçirme [araştırma kaynak IP adresi](#probesource) Ayrıntılar için.

## <a name="types"></a>Araştırma türleri

Sistem durumu araştırması, aşağıdaki üç protokol kullanılarak dinleyiciler için yapılandırılabilir:

- [TCP dinleyicisi](#tcpprobe)
- [HTTP uç noktaları](#httpprobe)
- [HTTPS uç noktaları](#httpsprobe)

Sistem durumu araştırmaları türlerini yük dengeleyici SKU bağlı olarak farklılık gösterir:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standart SKU |    &#9989; |   &#9989; |   &#9989; |
| Temel SKU |   &#9989; |   &#9989; | &#10060; |

Seçtiğiniz araştırma türünden bağımsız olarak, sistem durumu araştırmaları, gerçek hizmetin sağlandığı bağlantı noktası da dahil olmak üzere arka uç örneğindeki herhangi bir bağlantı noktasını gözlemleyebilirsiniz.

### <a name="tcpprobe"></a> TCP araştırma

TCP araştırmaları, bir üç yönlü açık TCP el sıkışması tanımlı bir bağlantı ile gerçekleştirerek bir bağlantıyı başlatırsınız.  TCP araştırmaları bir bağlantıyı dört yönlü bir kapalı TCP el sıkışması ile sonlandırır.

En düşük araştırma aralığı 5 saniyedir ve iyi durumda olmayan yanıtlar en az sayıda 2'dir.  Tüm aralıkların toplam süresi 120 saniyeyi aşamaz.

Bir TCP araştırması başarısız olur:
* Örnek noktasındaki TCP dinleyiciyi sırasında zaman aşımı süresi hiç yanıt vermiyor.  Araştırma, araştırmayı işaretlemeden önce yanıtlanmak üzere yapılandırılan başarısız araştırma isteği sayısına göre aşağı işaretlenir.
* Örneğinden sıfırlama bir TCP araştırması alır.

#### <a name="resource-manager-template"></a>Resource Manager şablonu

```json
    {
      "name": "tcp",
      "properties": {
        "protocol": "Tcp",
        "port": 1234,
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="httpprobe"></a> <a name="httpsprobe"></a> HTTP / HTTPS araştırma

> [!NOTE]
> HTTPS araştırması için kullanılabilir, yalnızca [Standard Load Balancer](load-balancer-standard-overview.md).

HTTP ve HTTPS araştırmaları TCP araştırmasına dayanır ve belirtilen yola sahip bir HTTP GET oluşturur. Bu araştırmaların her ikisi de göreli yollar için HTTP GET destekler. HTTPS araştırmaları, aynı Aktarım Katmanı Güvenliği (TLS, SSL adıyla) birlikte HTTP araştırmaları gibi sarmalayıcı. Örnek bir HTTP 200 durum zaman aşımı süresi içinde yanıt verdiğinde durum yoklaması işaretlenir.  Sistem durumu araştırması, varsayılan olarak her 15 saniyede bir yapılandırılan durum araştırma bağlantı noktasını denetlemeye çalışır. En düşük araştırma aralığı 5 saniyedir. Tüm aralıkların toplam süresi 120 saniyeyi aşamaz.

HTTP/HTTPS araştırmaları Ayrıca, sistem durumu araştırması yapmak istiyorsanız yararlı olabilir.  araştırma bağlantı noktası Ayrıca hizmetin kendisi için dinleyicidir, yük dengeleyici dönüşten örnekleri kaldırmak için kendi mantığınızı uygulayın. Örneğin, % 90 CPU ise örneğini kaldırmaya karar ve 200 HTTP durum döndürür. 

Artık Cloud Services'ı kullanın ve w3wp.exe kullanan web rolleri, ayrıca otomatik izleme, Web sitesini ulaşın. Web sitesi kodunuzdaki hataları, yük dengeleyici araştırması için 200 durumu döndürür.

Bir HTTP / HTTPS araştırma başarısız:
* Araştırma uç noktasına bir HTTP yanıt kodu 200 (örneğin, 403, 404 veya 500) dışında döndürür. Bu, sistem durumu araştırmasını hemen işaretleyecek. 
* Araştırma uç noktası 31 saniyelik zaman aşımı süresi boyunca hiç yanıt vermiyor. Araştırma çalışmıyor olarak işaretlenmeden ve tüm zaman aşımı aralıklarının toplamına ulaşılıncaya kadar birden çok araştırma isteği yanıtlanmayabilir.
* Araştırma uç noktası, TCP sıfırlama yoluyla bağlantıyı kapatır.

#### <a name="resource-manager-templates"></a>Resource Manager şablonları

```json
    {
      "name": "http",
      "properties": {
        "protocol": "Http",
        "port": 80,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

```json
    {
      "name": "https",
      "properties": {
        "protocol": "Https",
        "port": 443,
        "requestPath": "/",
        "intervalInSeconds": 5,
        "numberOfProbes": 2
      },
```

### <a name="guestagent"></a>Konuk aracı araştırması (yalnızca klasik)

Bulut hizmeti rolleri (çalışan rolleri ve web rolleri), varsayılan olarak araştırma izlemesi için konuk Aracısı kullanın.  Konuk aracı araştırması, son çare bir yapılandırmadır.  Her zaman bir TCP veya HTTP araştırmasıyla bir sistem durumu araştırması kullanın. Konuk aracı araştırması, çoğu uygulama senaryoları için açıkça tanımlanmış araştırmaları olabildiğince verimli değildir.

Konuk aracı araştırması, Konuk aracısının VM içindeki bir denetimdir. Dinler ve hazır durumda yalnızca örnektir ile bir HTTP 200 OK yanıtı yanıt verdiği. (Geri dönüştürme veya durduruluyor meşgul diğer durumlar vardır.)

Daha fazla bilgi için [Hizmet tanım dosyası (csdef) yapılandırmak için sistem durumu araştırmaları](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut Hizmetleri için genel yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Konuk Aracısı HTTP 200 OK ile yanıt vermezse, yük dengeleyici örnek yanıt vermiyor olarak işaretler. Ardından, bu örneğe akışlar gönderme durdurur. Yük Dengeleyici örneği denetlemek devam eder. 

Yük Dengeleyici yeni akışlar bu örneğe Konuk Aracısı bir HTTP 200 yanıt verirse, yeniden gönderir.

Bir web rolü kullandığınızda, Web sitesi kodu genellikle Azure tarafından izlenen değil w3wp.exe çalışan yapı veya konuk Aracısı. W3wp.exe (örneğin, HTTP 500 yanıt) hataları Konuk Aracısı ile bildirilen değildir. Sonuç olarak, yük dengeleyici rotasyon dışında bu örneğe almaz.

<a name="health"></a>
## <a name="probehealth"></a>Sistem durumu araştırması

TCP, HTTP ve HTTPS sistem durumu araştırmaları sağlıklı olarak kabul edilir ve rol örneği iyi durumda olduğunda olarak işaretle:

* VM önyüklendikten sonra sistem durumu araştırması başarılı olur.
* Rol örneğini sağlıklı olarak işaretlemek için gereken yoklamaların belirtilen sayısı.

> [!NOTE]
> Durum araştırması dalgalandıktan sonra, yük dengeleyici, rol örneğini yeniden sağlıklı duruma getirmeden önce daha uzun süre bekler. Bu ek bekleme süresi, kullanıcı ve altyapı korur ve kasıtlı bir ilkedir.

## <a name="probe-count-and-timeout"></a>Yoklama sayısı ve zaman aşımı

Araştırma davranışı bağlıdır:

* İşaretlenecek örneği izin başarılı bir araştırmalarla sayısı kadar yukarı.
* İşaretlenecek örneği neden başarısız yoklama sayısını kapalı olarak.

Belirtilen zaman aşımı ve Aralık değerleri bir örneğin yukarı veya aşağı olarak işaretlenip işaretlenmediğini belirtir.

## <a name="probedown"></a>Davranışı araştırma

### <a name="tcp-connections"></a>TCP bağlantıları

Yeni TCP bağlantıları kalan sağlıklı arka uç örnekleri için başarılı olur.

Bir arka uç örneği durum araştırması başarısız olursa, yerleşik TCP bağlantıları bu arka uç örneğe devam edin.

Yeni akış, bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, arka uç havuzuna gönderilir. Standart Load Balancer, devam etmek için yerleşik TCP akışları izin verir.  Temel yük dengeleyici arka uç havuzu için tüm mevcut TCP akışları sona erer.
 
Load Balancer bir geçiş hizmetidir (TCP bağlantılarını sonlandıramaz) ve akış her zaman istemci ile sanal makinenin Konuk işletim sistemi ve uygulaması arasındadır. Tüm yoklamalar içeren bir havuz, akışı alacak ve bir SYN-ACK ile yanıt veren sağlam bir arka uç örneği bulunmadığından, ön uç, TCP bağlantı açma girişimlerine (SYN) yanıt vermemesine neden olur.

### <a name="udp-datagrams"></a>UDP veri birimi

UDP veri birimi için sağlıklı arka uç örnekleriyle verilecektir.

UDP bağlantısız ve izlenen UDP için hiçbir akış durumu yoktur. Herhangi bir arka uç örneğinin sistem durumu araştırması başarısız olursa, mevcut UDP akışları arka uç havuzunda iyi durumda başka bir örneğine taşıyabilir.

Bir arka uç havuzundaki tüm örnekleri için tüm araştırmaları başarısız olursa, temel ve standart Load balancer'ları için mevcut UDP akışları sonlanacaktır.

<a name="source"></a>
## <a name="probesource"></a>Kaynak IP adresi araştırma

Yük Dengeleyici, dağıtılmış bir yoklama hizmeti için kendi iç sistem durumu modeli kullanır. Yoklama hizmeti, sanal makinelerin bulunduğu her bir konakta bulunur ve müşterinin yapılandırmasına göre sistem durumu araştırmaları oluşturmak için isteğe bağlı olarak programlanabilir. Durum araştırma trafiği, sistem durumu araştırması ve müşteri VM 'si üreten araştırma hizmeti arasında doğrudan yapılır. Tüm yük dengeleyici sistem durumu araştırmaları, kaynak 168.63.129.16 IP adresinden kaynaklanan.  RFC1918 alanı olmayan bir VNet 'in içindeki IP adres alanını kullanabilirsiniz.  Küresel olarak ayrılmış, Microsoft 'un sahip olduğu IP adresi, VNet içinde kullandığınız IP adresi alanıyla bir IP adresi çakışması olasılığını azaltır.  Bu IP adresi tüm bölgelerde aynıdır ve değişmez ve yalnızca iç Azure platformu bileşeni bu IP adresinden bir paket kaynak sağladığından bir güvenlik riski değildir. 

AzureLoadBalancer Service etiketi, [ağ güvenlik gruplarınızı](../virtual-network/security-overview.md) bu kaynak IP adresini tanımlar ve varsayılan olarak sistem durumu araştırma trafiğine izin verir.

Load Balancer sistem durumu araştırmalarının yanı sıra [aşağıdaki işlemler bu IP adresini kullanır](../virtual-network/what-is-ip-address-168-63-129-16.md):

- VM platformu ile iletişim kurmak için bir "Hazır" durumunda olduğu sinyal aracısı sağlar
- Özel DNS sunucuları tanımlamaz müşterilere filtrelenmiş ad çözümlemesi sağlamak için DNS sanal sunucu ile iletişimi sağlar.  Bu filtreleme müşteriler bunların dağıtım ana bilgisayar adları yalnızca çözümleyebilmesini sağlar.
- VM 'nin Azure 'daki DHCP hizmetinden dinamik bir IP adresi almasını sağlar.

## <a name="design"></a>Tasarım Kılavuzu

Sistem durumu araştırmaları, hizmetinizi dayanıklı hale getirmek ve ölçeklendirilmesine izin vermek için kullanılır. Yanlış yapılandırma veya hatalı tasarım deseninin kullanılabilmesi, hizmetinizin kullanılabilirliğini ve ölçeklenebilirliğini etkileyebilir. Tüm belgeyi gözden geçirin ve bu araştırma yanıtının ne kadar etkilendiğine veya ne zaman işaretleneceğini ve uygulama senaryonuzun kullanılabilirliğini nasıl etkilediğini düşünün.

Uygulamanız için sistem durumu modelini tasarlarken, bu örneğin durumunu __ve__ sağladığınızda uygulama hizmetini yansıtan bir arka uç örneğinde bir bağlantı noktasını araştırmanız gerekir.  Uygulama bağlantı noktası ve yoklama bağlantı noktasının aynı olması gerekmez.  Bazı senaryolarda, araştırma bağlantı noktasının uygulamanızın hizmeti sağladığı bağlantı noktasından farklı olması istenebilir.  

Bazen uygulamanızın sistem durumunu algılamaması için bir sistem durumu araştırma yanıtı oluşturması faydalı olabilir, ancak aynı zamanda örneğinizin yeni akışlar alıp almamasının gerekip gerekmediğini Load Balancer doğrudan işaret edin.  Uygulamanızın, sistem durumu araştırmasını veya uygulamanızın bakımını hazırlamaya ve senaryonuzu boşaltmaya hazırlanarak, yeni akışların bir örneğe basıncını ve daha fazla teslimini oluşturmasına izin vermek için araştırma yanıtını değiştirebilirsiniz.  Standart Load Balancer kullanırken, [araştırma](#probedown) sinyali, boşta kalma zaman aşımı veya bağlantı kapanışına kadar her zaman TCP akışlarının devam etmesine izin verir. 

UDP yük dengelemesi için, arka uç örneğinden özel bir sistem durumu araştırma sinyali oluşturmanız ve UDP uygulamanızın sistem durumunu yansıtmak için karşılık gelen dinleyiciyi hedefleyen bir TCP, HTTP veya HTTPS sistem durumu araştırması kullanmanız gerekir.

[Standart Load Balancer](load-balancer-standard-overview.md)Ile [ha bağlantı noktaları Yük Dengeleme kuralları](load-balancer-ha-ports-overview.md) kullanırken, tüm bağlantı noktaları yük dengedir ve tek bir sistem durumu araştırma yanıtı, tüm örneğin durumunu yansıtmalıdır.

Bu yapılandırma, senaryomunuzdan oluşan basamaklı hatalara yol açacağından, sistem durumu araştırmasını VNet 'teki başka bir örneğe alan örnek aracılığıyla bir sistem durumu araştırmasını veya proxy 'yi çevirmeyin.  Aşağıdaki senaryoyu göz önünde bulundurun: bir dizi üçüncü taraf aracı, gereçlere yönelik ölçek ve artıklık sağlamak üzere bir Load Balancer kaynağın arka uç havuzunda dağıtılır ve sistem durumu araştırması, üçüncü taraf gereç proxy 'lerinin bulunduğu bir bağlantı noktasını yoklamak üzere yapılandırılmıştır veya Gereç arkasındaki diğer sanal makinelere çevirir.  Gerecin arkasındaki diğer sanal makinelere veya ara sunucu isteklerini çevirmek için kullandığınız bağlantı noktasını araştırdıysanız, gerecin arkasındaki tek bir sanal makineden gelen yoklama yanıtları, gerecin kendisini ölü olarak işaretler. Bu yapılandırma, gerecin arkasındaki tek bir arka uç örneğinin sonucu olarak tüm uygulama senaryosunun basamaklı bir hatasına neden olabilir.  Tetikleyici, Load Balancer özgün hedefi (gereç örneği) işaretlemesine neden olacak ve sonra tüm uygulama senaryonuzu devre dışı bırakabilmesi için aralıklı bir araştırma hatası olabilir. Bunun yerine gereç durumunu araştırma. Sistem durumu sinyalinin belirlenmesi için araştırmanın seçimi, ağ sanal gereçleri (NVA) senaryolarında önemli bir noktadır ve bu senaryolar için uygun sistem durumu sinyalinin ne olduğunu öğrenmek için uygulama satıcınıza başvurmanız gerekir.

Güvenlik Duvarı ilkeleriniz için araştırmanın [kaynak IP 'si](#probesource) izin vermezseniz, örneğinize ulaşamadığından sistem durumu araştırması başarısız olur.  Buna karşılık, yük dengeleyici örneğinizin sistem durumu araştırma hatası nedeniyle aşağı işaretler.  Bu yanlış yapılandırma, yük dengeli uygulama senaryonuzun başarısız olmasına neden olabilir.

Örneğinizi işaretlemek için Load Balancer sistem durumu araştırması için, tüm Azure [ağ güvenlik gruplarında](../virtual-network/security-overview.md) ve yerel güvenlik duvarı ILKELERINDE bu IP adresine izin vermeniz **gerekir** .  Varsayılan olarak, her ağ güvenlik grubu, durum araştırma trafiğine izin vermek için AzureLoadBalancer [hizmet etiketini](../virtual-network/security-overview.md#service-tags) içerir.

Bir sistem durumu araştırma hatasını test etmek veya tek bir örneği işaretlemek isterseniz, sistem durumu araştırmasını (hedef bağlantı noktası veya [kaynak IP](#probesource)) açıkça engellemek ve bir araştırmanın başarısızlığının benzetimini yapmak için bir [ağ güvenlik grubu](../virtual-network/security-overview.md) kullanabilirsiniz.

VNet 'i 168.63.129.16 içeren, Microsoft 'un sahip olduğu IP adresi aralığıyla yapılandırmayın.  Bu tür yapılandırmalarda sistem durumu araştırmasının IP adresi ile çakışacaktır ve senaryonuzun sürmesine neden olabilir.

Sanal makinenizde birden fazla arabirimi varsa, temel alınan arabirimde araştırma yanıt Sigortası gerekir.  Bu adresi VM 'de her arabirim için kaynak ağ adresi çevirisi yapmanız gerekebilir.

[TCP zaman damgalarını](https://tools.ietf.org/html/rfc1323)etkinleştirmeyin.  TCP zaman damgalarının etkinleştirilmesi, sanal makinenin Konuk işletim sistemi TCP yığını tarafından bırakılan TCP paketlerinin, ilgili uç noktayı işaretlemek Load Balancer neden olan sistem durumu araştırmalarının başarısız olmasına neden olur.  Güvenlik sağlamlaştırılmış VM görüntülerinde varsayılan olarak TCP zaman damgaları etkinleştirilir ve devre dışı bırakılmalıdır.

## <a name="monitoring"></a>İzleme

Hem genel hem de iç [Standard Load Balancer](load-balancer-standard-overview.md) uç ve arka uç örnek araştırma durumu Azure İzleyicisi üzerinden çok boyutlu ölçümler olarak kullanıma sunar. Bu ölçümler diğer Azure hizmetleri veya iş ortağı uygulamaları tarafından tüketilebilir. 

Temel genel Load Balancer, Azure Izleyici günlükleri aracılığıyla arka uç havuzu başına özetlenen durum araştırma durumunu gösterir.  Azure Izleyici günlükleri, iç temel yük dengeleyiciler için kullanılamaz.  [Azure izleyici günlüklerini](load-balancer-monitor-log.md) kullanarak genel yük dengeleyici araştırma sistem durumunu ve araştırma sayısını denetleyebilirsiniz. Günlüğe kaydetme, Power BI veya Azure operasyonel İçgörüler ile yük dengeleyici sistem durumu hakkındaki istatistiklerdir sağlamak için kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

- HTTPS araştırmaları, bir istemci sertifikası ile karşılıklı kimlik doğrulamasını desteklemez.
- TCP zaman damgaları etkinleştirildiğinde sistem durumu araştırmaları başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- [PowerShell kullanarak Resource Manager'da herkese açık yük dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-arm-ps.md)
- [Sistem durumu araştırmaları için REST API](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- Yeni sistem durumu araştırması becerileriyle ile istek [Load Balancer'ın Uservoice](https://aka.ms/lbuservoice)
