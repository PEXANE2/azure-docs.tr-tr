---
title: Hizmetiniz için HA'yı ölçeklendirmek ve sağlamak için sağlık sondaları
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure Yük Bakiyesi arkasındaki örnekleri izlemek için sistem sondalarını nasıl kullanacağınızı öğrenin
services: load-balancer
documentationcenter: na
author: asudbring
manager: kumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: ec1507e09a183f8d466a456b70151861f5f0e82c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159447"
---
# <a name="load-balancer-health-probes"></a>Load Balancer durum araştırmaları

Azure Yük Dengeleyicisi ile yük dengeleme kurallarını kullanırken, Yük Bakiyesi'nin arka uç uç noktası durumunu algılamasına izin vermek için sistem durumu sondalarını belirtmeniz gerekir.  Sistem durumu sondası ve sonda yanıtlarının yapılandırması, hangi arka uç havuzu örneklerinin yeni akışlar alacağını belirler. Bir uygulamanın hatasını arka uç uç noktasında algılamak için sistem durumu sondalarını kullanabilirsiniz. Ayrıca, bir sistem durumu sondasına özel bir yanıt oluşturabilir ve yük veya planlanan kapalı kalma süresini yönetmek için akış denetimi için sistem sondasını kullanabilirsiniz. Bir sistem durumu sondası başarısız olduğunda, Yük Dengeleyicisi ilgili sağlıksız örneğe yeni akışlar göndermeyi durdurur. Giden bağlantı etkilenmez, yalnızca gelen bağlantı etkilenir.

Sağlık sondaları birden çok protokolü destekler. Belirli bir sağlık sondası protokolünün kullanılabilirliği Yük Dengeleyici SKU'ya göre değişir.  Ayrıca, hizmetin davranışı bu tabloda gösterildiği gibi Yük Dengeleyici SKU göre değişir:

| | Standart SKU | Temel SKU |
| --- | --- | --- |
| [Sonda türleri](#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Sonda aşağı davranış](#probedown) | Tüm sondalar aşağı, tüm TCP akışları devam eder. | Tüm sondalar aşağı, tüm TCP akışları nın süresi doluyor. | 


>[!IMPORTANT]
>Güvenilir bir hizmet oluşturmak için aşağıdaki önemli [tasarım kılavuzu](#design) da dahil olmak üzere bu belgeyi bütünüyle gözden geçirin.

>[!IMPORTANT]
>Yük Dengeleyici sağlık probları IP adresinden kaynaklanır 168.63.129.16 ve örnek işaretlemek için problar için engellenmemelidir.  Ayrıntılar için [sonda kaynağı IP adresini](#probesource) gözden geçirin.

>[!IMPORTANT]
>Yapılandırılmış zaman çıkış eşiğine bakılmaksızın, SUNUCU HTTP 200 Ok olmayan herhangi bir durum kodunu döndürürse veya bağlantı TCP sıfırlama yoluyla sonlandırılırsa, HTTP(S) Load Balancer sistem sondaları bir örneği otomatik olarak inceler.

## <a name="probe-configuration"></a><a name="probes"></a>Sonda yapılandırması

Sistem durumu sondası yapılandırması aşağıdaki öğelerden oluşur:

- Tek tek problar arasındaki aralığın süresi
- Sonda farklı bir duruma geçmeden önce gözlemlenmeleri gereken sonda yanıtlarının sayısı
- Sondanın protokolü
- Sondanın bağlantı noktası
- HTTP(S) sondalarını kullanırken HTTP GET için kullanılacak HTTP yolu

>[!NOTE]
>Sonda tanımı, Azure PowerShell, Azure CLI, Şablonlar veya API kullanırken zorunlu değildir veya denetlenmez. Sonda doğrulama testleri yalnızca Azure Portalı kullanılarak yapılır.

## <a name="understanding-application-signal-detection-of-the-signal-and-reaction-of-the-platform"></a>Uygulama sinyalinin anlaşılması, sinyalin algılanması ve platformun reaksiyonu

Sonda yanıtlarının sayısı her ikisi için de geçerlidir

- bir örneğin yukarı olarak işaretlemesine izin veren başarılı sondaların sayısı ve
- bir örneğin aşağı olarak işaretlenebilmelidir neden zamanlanmış sondaların sayısı.

Belirtilen zaman aralığı ve aralık değerleri, bir örneğin yukarı mı yoksa aşağı mı olarak işaretleneceğini belirler.  Sonda yanıtlarının sayısıyla çarpılır aralığın süresi, sonda yanıtlarının algılanılması gereken süreyi belirler.  Ve gerekli sondalar sağlandıktan sonra hizmet tepki verecektir.

Davranışı bir örnekle daha da gösteredebiliriz. Sonda yanıtlarının sayısını 2'ye ve aralığı 5 saniyeye ayarladıysanız, bu 10 saniyelik bir aralık içinde 2 sonda zaman aralığı hatalarının gözlenmeleri gerektiği anlamına gelir.  Sondanın gönderildiği saat, uygulamanız durumu değiştirebildiği zaman eşitlenmedığından, algılama süresini iki senaryoya göre bağlayabiliriz:

1. Uygulamanız ilk sonda gelmeden hemen önce bir zaman çıkış sondası yanıtı üretmeye başlarsa, bu olayların algılanması 10 saniye (2 x 5 saniye aralık) artı uygulamanın süresi nin ilk zaman aralığına sinyal vermeye başlaması sonda geldi.  Bu algılamanın 10 saniyeden biraz daha uzun süreceğini varsayabilirsiniz.
2. Uygulamanız ilk sonda geldikten hemen sonra bir zaman zaman sondası yanıtı üretmeye başlarsa, bu olayların algılanması bir sonraki sonda gelene (ve saatler çıkana kadar) artı başka bir 10 saniye (2 x 5 saniyelik aralıklar) gelene kadar başlamaz.  Bu algılamanın 15 saniyeden az süreceğini varsayabilirsiniz.

Bu örnekte, algılama oluştuktan sonra, platformun bu değişikliğe tepki göstermesi çok az zaman alır.  Bu bir bağlı olarak anlamına gelir 

1. uygulama durumu değiştirmeye başladığında ve
2. bu değişiklik algılandığında ve gerekli ölçütleri (belirtilen aralıkta gönderilen sonda sayısı) karşılandığında ve
3. algılama platformu üzerinden iletildiğinde 

bir zaman-out sonda tepkisi ne kadar az 10 saniye ve biraz üzerinde biraz 15 saniye arasında uygulamadan sinyal bir değişiklik tepki alacağını varsayabiliriz.  Bu örnek, neler olduğunu göstermek için sağlanır, ancak, bu örnekte gösterilen yukarıdaki kaba kılavuz ötesinde tam bir süre tahmin etmek mümkün değildir.
 
## <a name="probe-types"></a><a name="types"></a>Sonda türleri

Sistem durumu sondası tarafından kullanılan protokol aşağıdakilerden birine yapılandırılabilir:

- [TCP dinleyicileri](#tcpprobe)
- [HTTP uç noktaları](#httpprobe)
- [HTTPS uç noktaları](#httpsprobe)

Kullanılabilir protokoller kullanılan Yük Dengeleyici SKU bağlıdır:

|| TCP | HTTP | HTTPS |
| --- | --- | --- | --- |
| Standart SKU |    &#9989; |   &#9989; |   &#9989; |
| Temel SKU |   &#9989; |   &#9989; | &#10060; |

### <a name="tcp-probe"></a><a name="tcpprobe"></a>TCP sondası

TCP sondaları, tanımlanan bağlantı noktasıyla üç yönlü açık TCP el sıkışması gerçekleştirerek bir bağlantı başlatır.  TCP sondaları dört yönlü kapalı TCP el sıkışması ile bağlantıyı sonlandırır.

Minimum sonda aralığı 5 saniye, en az sağlıksız yanıt sayısı 2'dir.  Tüm aralıkların toplam süresi 120 saniyeyi geçemez.

Bir TCP sondası şu anda başarısız olur:
* Örnekteki TCP dinleyicisi zaman acısı döneminde hiç yanıt vermez.  Sonda, sondayı işaretlemeden önce yanıtsız kalmak üzere yapılandırılan zamanlanmış sonda isteklerinin sayısına göre işaretlenir.
* Sonda, örnekten bir TCP sıfırlaması alır.

Aşağıda, bu tür bir sonda yapılandırmasını Kaynak Yöneticisi şablonunda nasıl ifade edebilirsiniz:

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

### <a name="http--https-probe"></a><a name="httpprobe"></a><a name="httpsprobe"></a> HTTP / HTTPS sondası

>[!NOTE]
>HTTPS prob sadece [Standart Yük Dengeleyicisi](load-balancer-standard-overview.md)için kullanılabilir.

HTTP ve HTTPS sondaları TCP sondası üzerine inşa ve belirtilen yol ile bir HTTP GET sorunu. Bu sondaların her ikisi de HTTP GET için göreli yolları destekler. HTTPS sondaları, bir Taşıma Katmanı Güvenliği (TLS, eski adıyla SSL) sarıcı nın eklenmesiyle HTTP sondalarıyla aynıdır. Sistem durumu sondası, zaman anına giren bir HTTP durumu 200 ile yanıt verdiğinde işaretlenir.  Sistem durumu sondası, yapılandırılan sistem durumu sondası bağlantı noktasını varsayılan olarak her 15 saniyede bir denetlemeye çalışır. Minimum sonda aralığı 5 saniyedir. Tüm aralıkların toplam süresi 120 saniyeyi geçemez.

HTTP / HTTPS probları da sonda bağlantı noktası da hizmet kendisi için dinleyici ise yük dengeleyici rotasyon örnekleri kaldırmak için kendi mantık uygulamak için yararlı olabilir. Örneğin, %90 CPU'nun üzerindeyse bir örneği kaldırmaya ve 200'e ait olmayan bir HTTP durumunu döndürmeye karar verebilirsiniz. 

> [!NOTE] 
> HTTPS Probe tüm zincir sha256 minimum imza karma olan tabanlı sertifikaların kullanımını gerektirir.

Bulut Hizmetlerini kullanıyorsanız ve w3wp.exe kullanan web rollerine sahipseniz, web sitenizin otomatik olarak izlenmesini de elde edersiniz. Web sitenizdeki hatalar, 200 olmayan bir durumu yük dengeleyici sondasına döndürer.

Bir HTTP / HTTPS sondası başarısız olduğunda:
* Sonda bitiş noktası 200 (örneğin, 403, 404 veya 500) dışında bir HTTP yanıt kodu döndürür. Bu, sağlık sondasını hemen işaretleyecek. 
* Sonda bitiş noktası, sonda aralığı nın ve 30 saniyelik zaman aralığının en az sırasında hiç yanıt vermez. Sonda çalışmamış olarak işaretleninceye ve tüm zaman aralığının toplamına ulaşılıncaya kadar birden çok sonda isteği yanıtsız kalabilir.
* Sonda uç noktası, TCP sıfırlama yoluyla bağlantıyı kapatır.

Aşağıda, bu tür bir sonda yapılandırmasını Kaynak Yöneticisi şablonunda nasıl ifade edebilirsiniz:

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

### <a name="guest-agent-probe-classic-only"></a><a name="guestagent"></a>Konuk ajan sondası (Yalnızca Klasik)

Bulut hizmeti rolleri (çalışan rolleri ve web rolleri) varsayılan olarak sonda izleme için bir konuk aracı kullanır.  Konuk ajan sondası son çare yapılandırmasıdır.  Her zaman açıkça bir TCP veya HTTP sondası ile bir sağlık sondası kullanın. Konuk ajan sondası, çoğu uygulama senaryosu için açıkça tanımlanmış sondalar kadar etkili değildir.

Konuk ajan sondası VM'deki konuk ajanın kontrolüdür. Daha sonra dinler ve yalnızca örnek Hazır durumda olduğunda bir HTTP 200 Ok yanıtı ile yanıt verir. (Diğer durumlar Meşgul, Geri Dönüşüm veya Durdurmadır.)

Daha fazla bilgi için, [sistem durumu sondaları için hizmet tanımı dosyasını (csdef) yapılandırın](https://msdn.microsoft.com/library/azure/ee758710.aspx) veya [bulut hizmetleri için genel bir yük dengeleyicisi oluşturarak başlayın.](https://docs.microsoft.com/azure/load-balancer/load-balancer-get-started-internet-classic-cloud#check-load-balancer-health-status-for-cloud-services)

Konuk aracı HTTP 200 Ok ile yanıt vermezse, yük dengeleyicisi örneği yanıt vermiyor olarak işaretler. Daha sonra bu örne akış göndermeyi durdurur. Yük dengeleyici örneği denetlemeye devam ediyor. 

Konuk aracı http 200 ile yanıt verirse, yük dengeleyicisi bu örne yeniden yeni akışlar gönderir.

Bir web rolü kullandığınızda, web sitesi kodu genellikle Azure kumaşı veya konuk aracı tarafından izlenilmeyen w3wp.exe'de çalışır. w3wp.exe'deki hatalar (örneğin, HTTP 500 yanıtları) konuk temsilciye bildirilmemiştir. Sonuç olarak, yük dengeleyicisi bu örneği döndürme nin dışına çıkarmaz.

<a name="health"></a>
## <a name="probe-up-behavior"></a><a name="probehealth"></a>Davranışı araştırma

TCP, HTTP ve HTTPS sistem sondaları sağlıklı kabul edilir ve aşağıdaki hallerde arka uç bitiş noktasını sağlıklı olarak işaretler:

* Sağlık sondası VM botlar sonra bir kez başarılı olur.
* Sağlıklı olarak arka uç bitiş noktasını işaretlemek için gereken sonda sayısı elde edilmiştir.

Sağlıklı bir duruma ulaşmış herhangi bir arka uç uç noktası yeni akışlar almaya uygundur.  

> [!NOTE]
> Sağlık sondası dalgalanırsa, yük dengeleyiciarka uç noktasını sağlıklı duruma geri koyması için daha uzun süre bekler. Bu ekstra bekleme süresi kullanıcıyı ve altyapıyı korur ve kasıtlı bir ilkedir.

## <a name="probe-down-behavior"></a><a name="probedown"></a>Sonda aşağı davranış

### <a name="tcp-connections"></a>TCP bağlantıları

Yeni TCP bağlantıları, sağlıklı arka uç bitiş noktası nı kalmayı başarır.

Bir arka uç uç noktasının sistem durumu sondası başarısız olursa, bu arka uç bitiş noktasına kurulan TCP bağlantıları devam eder.

Arka uç havuzundaki tüm örneklerin tüm sondaları başarısız olursa, arka uç havuzuna yeni akışlar gönderilmez. Standart Yük Dengeleyicisi, yerleşik TCP akışlarının devam etmesine izin verecektir.  Temel Yük Dengeleyicisi, varolan tüm TCP akışlarını arka uç havuzuna sonlandırır.
 
Yük Dengeleyici hizmet yoluyla bir geçiş (TCP bağlantıları sona erdirmez) ve akış her zaman istemci ve VM konuk işletim sistemi ve uygulama arasındadır. Tüm sondaları aşağı olan bir havuz, akışı almak ve SYN-ACK ile yanıt vermek için sağlıklı bir arka uç bitiş noktası olmadığından, ön uç TCP bağlantısı açık girişimlerine (SYN) yanıt vermemeye neden olur.

### <a name="udp-datagrams"></a>UDP veri gramları

UDP verigramları sağlıklı arka uç uç noktalarına teslim edilecektir.

UDP bağlantısızve UDP için izlenen akış durumu yoktur. Herhangi bir arka uç uç noktasının sistem durumu sondası başarısız olursa, varolan UDP akışları arka uç havuzunda başka bir sağlıklı örneğe taşınır.

Arka uç havuzundaki tüm örneklerin tüm probları başarısız olursa, Temel ve Standart Yük Dengeleyicileri için varolan UDP akışları sonlandırır.

<a name="source"></a>
## <a name="probe-source-ip-address"></a><a name="probesource"></a>Sonda kaynak IP adresi

Load Balancer, dahili sistem durumu modeli için dağıtılmış bir sondalama hizmeti kullanır. Sonlama hizmeti, VM'lerin bulunduğu her ana bilgisayarda bulunur ve müşterinin yapılandırmasına göre sistem durumu sondaları oluşturmak üzere isteğe bağlı olarak programlanabilir. Sistem durumu sondası trafiği doğrudan sağlık sondasını oluşturan sondalama hizmeti ile müşteri VM'si arasındadır. Tüm Yük Dengeleyici sağlık probları ip adresi 168.63.129.16 kaynak olarak kaynaklanır.  RFC1918 alanı olmayan bir VNet'in içindeki IP adres alanını kullanabilirsiniz.  Microsoft'un sahip olduğu genel olarak ayrılmış bir IP adresi kullanarak, VNet içinde kullandığınız IP adresi alanıyla ip adresi çakışma olasılığını azaltır.  Bu IP adresi tüm bölgelerde aynıdır ve değişmez ve yalnızca dahili Azure platformu bileşeni bu IP adresinden paket kaynak olabileceğinden bir güvenlik riski değildir. 

AzureLoadBalancer hizmet [etiketi, ağ güvenlik gruplarınızdaki](../virtual-network/security-overview.md) bu kaynak IP adresini tanımlar ve varsayılan olarak sistem durumu sonda trafiğine izin verir.

Load Balancer sistem sondalarına ek olarak, [aşağıdaki işlemler bu IP adresini kullanır:](../virtual-network/what-is-ip-address-168-63-129-16.md)

- VM Aracısının platformla iletişim kurarak "Hazır" durumda olduğunu bildirmesini sağlar
- Özel DNS sunucularını tanımlamayan müşterilere filtrelenmiş ad çözünürlüğü sağlamak için DNS sanal sunucusuyla iletişimi sağlar.  Bu filtreleme, müşterilerin yalnızca dağıtımlarının ana bilgisayar adlarını çözümlemesini sağlar.
- VM'nin Azure'daki DHCP hizmetinden dinamik bir IP adresi almasını sağlar.

## <a name="design-guidance"></a><a name="design"></a>Tasarım kılavuzu

Sağlık sondaları, hizmetinizi esnek hale getirmek ve ölçeklendirmek için kullanılır. Hatalı yapılandırma veya kötü tasarım deseni, hizmetin kullanılabilirliğini ve ölçeklenebilirliğini etkileyebilir. Bu belgenin tamamını gözden geçirin ve bu sonda yanıtı işaretlendiğinde veya işaretlendiğinde senaryonuzüzerindeki etkisinin ne olduğunu ve uygulama senaryonuzun kullanılabilirliğini nasıl etkilediğini düşünün.

Uygulamanız için sistem durumu modelini tasarlarken, o örneğin durumunu __ve__ sağladığınız uygulama hizmetini yansıtan bir arka uç bitiş noktası üzerinde bir bağlantı noktasını araştırmanız gerekir.  Uygulama bağlantı noktası ve sonda bağlantı noktasının aynı olması gerekmez.  Bazı senaryolarda, sonda bağlantı noktasının uygulamanızın hizmet verdiği bağlantı noktasından farklı olması istenebilir.  

Bazen uygulamanızın yalnızca uygulama nızın durumunu algılamak için bir sistem durumu sondası yanıtı oluşturması yararlı olabilir, aynı zamanda örneğinizin yeni akışlar alıp almaması gerektiğini doğrudan Load Balancer'a sinyal verebilir.  Sonda yanıtını, uygulamanızın sistem durumu sondasını geçerek bir örne geri basınç ve gaz iletimi oluşturmasına izin vermek için işleyebilir veya uygulamanızın bakımına hazırlanabilir ve senaryonuzu boşaltmaya başlayabilirsiniz.  Standart Yük Dengeleyicisi kullanırken, [bir sonda aşağı](#probedown) sinyali her zaman TCP akışlarının boşta zaman anına veya bağlantı kapatmaya kadar devam etmesine izin verir. 

UDP yük dengelemesi için, arka uç uç noktasından özel bir sistem durumu sondası sinyali oluşturmalı ve UDP uygulamanızın durumunu yansıtacak şekilde ilgili dinleyiciyi hedefleyen bir TCP, HTTP veya HTTPS sistem durumu sondası kullanmalısınız.

[Standart Yük Dengeleyici](load-balancer-standard-overview.md)ile [HA Bağlantı Noktaları yük dengeleme kuralları](load-balancer-ha-ports-overview.md) kullanılırken, tüm bağlantı noktaları yük dengeli ve tek bir sistem sondası yanıtı tüm örneğin durumunu yansıtmalıdır.

Bu yapılandırma senaryonuzda basamaklı hatalara yol açabileceğinden, vnet'inizdeki başka bir örneğe sistem durumu sondasını alan örnek üzerinden bir sistem durumu sondasını çevirmeyin veya proxy etmeyin.  Aşağıdaki senaryoyu göz önünde bulundurun: Bir Yük Dengeleyici kaynağının arka uç havuzunda, aygıtlar için ölçek ve artıklık sağlamak üzere bir üçüncü taraf cihaz kümesi dağıtılır ve sistem durumu sondası, üçüncü taraf cihaz eklerinin veya cihazın arkasındaki diğer sanal makinelere çevirir.  Cihazın arkasındaki diğer sanal makinelere istekler çevirmek veya proxy etmek için kullandığınız aynı bağlantı noktasını araştırrsanız, cihazın arkasındaki tek bir sanal makineden gelen herhangi bir sonda yanıtı cihazın kendisini ölü olarak işaretleyecektir. Bu yapılandırma, cihazın arkasındaki tek bir arka uç bitiş noktası sonucunda tüm uygulama senaryosunun basamaklı bir arızaya yol açabilir.  Tetikleyici, Yük Dengeleyicisinin orijinal hedefi (cihaz örneğini) işaretlemesine ve bunun karşılığında tüm uygulama senaryonuzu devre dışı düşürebilmesine neden olacak aralıklı bir sonda hatası olabilir. Bunun yerine cihazın kendi sağlığını araştırın. Sistem durumu sinyalini belirlemek için sonda seçimi, ağ sanal cihazları (NVA) senaryoları için önemli bir husustur ve bu tür senaryolar için uygun sistem durumu sinyalinin ne olduğu konusunda uygulama satıcınıza danışmalısınız.

Güvenlik duvarı ilkelerinizde sondanın [kaynak IP'sine](#probesource) izin vermezseniz, örneğinize ulaşamadığı için sistem durumu sondası başarısız olur.  Buna karşılık, Yük Dengeleyici sağlık sondası arızası nedeniyle örneğinizi aşağı işaretleecektir.  Bu yanlış yapılandırma, yük dengeli uygulama senaryobaşarısız neden olabilir.

Load Balancer'ın sistem durumu sondasının örneğinizi işaretlemesi için, bu IP adresine azure [ağ güvenlik gruplarında](../virtual-network/security-overview.md) ve yerel güvenlik duvarı ilkelerinde izin **vermelisiniz.**  Varsayılan olarak, her ağ güvenlik grubu, sistem durumu sondatrafiğine izin vermek için AzureLoadBalancer [hizmet etiketini](../virtual-network/security-overview.md#service-tags) içerir.

Bir sistem durumu sondası hatasını sınamak veya tek bir örneği işaretlemek istiyorsanız, sistem durumu sondasını (hedef bağlantı noktası veya [kaynak IP)](#probesource)açıkça engellemek ve bir sondanın başarısızlığını simüle etmek için ağ [güvenlik gruplarını](../virtual-network/security-overview.md) kullanabilirsiniz.

VNet'inizi 168.63.129.16 içeren Microsoft'a ait IP adresi aralığıyla yapılandırmayın.  Bu tür yapılandırmalar sistem durumu sondasının IP adresiyle çarpışacak ve senaryonuzun başarısız lığa neden olabilir.

VM'nizde birden çok arabirim varsa, aldığınız arabirimdeki sondaya yanıt vermenizi sağlamanız gerekir.  Kaynak ağ adresi nin bu adresi VM'de arabirim bazında çevirmesi gerekebilir.

[TCP zaman damgalarını](https://tools.ietf.org/html/rfc1323)etkinleştirme.  TCP zaman damgalarını etkinleştirmek, TCP paketlerinin VM'nin konuk OS TCP yığını tarafından düşürülmesi nedeniyle sistem sondalarının başarısız olmasına neden olabilir ve bu da Yük Dengeleyicisinin ilgili bitiş noktasını işaretlemesine neden olur.  TCP zaman damgaları, güvenlik sertleştirilmiş VM görüntülerinde varsayılan olarak düzenli olarak etkinleştirilir ve devre dışı bırakılmalı.

## <a name="monitoring"></a>İzleme

Hem genel hem de dahili [Standart Yük Dengeleyici,](load-balancer-standard-overview.md) Azure Monitor aracılığıyla uç nokta başına ve arka uç nokta durumu sonda durumunu çok boyutlu ölçümler olarak ortaya çıkarır. Bu ölçümler diğer Azure hizmetleri veya iş ortağı uygulamaları tarafından tüketilebilir. 

Temel genel Yük Dengeleyicisi, Azure Monitor günlükleri aracılığıyla arka uç başına havuz başına özetlenen sistem sondası durumunu ortaya çıkarır.  Azure Monitor günlükleri dahili Temel Yük Dengeleyicileri için kullanılamaz.  Ortak yük dengeleyici sondası sağlık durumunu ve sonda sayısını denetlemek için [Azure Monitor günlüklerini](load-balancer-monitor-log.md) kullanabilirsiniz. Günlüğe kaydetme, yük dengeleyici durumu durumu yla ilgili istatistikler sağlamak için Power BI veya Azure Operasyonel Öngörüleri ile kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

- HTTPS sondaları, istemci sertifikasıyla karşılıklı kimlik doğrulamasını desteklemez.
- TCP zaman damgaları etkinleştirildiğinde Sistem Durumu sondalarının başarısız olacağını varsaymalısınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyicisi](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- [PowerShell'i kullanarak Kaynak Yöneticisi'nde genel yük dengeleyicioluşturmaya başlayın](quickstart-create-standard-load-balancer-powershell.md)
- [Sağlık sondaları için REST API](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerprobes/)
- [Load Balancer'ın Uservoice'u](https://aka.ms/lbuservoice) ile yeni sağlık sondası yetenekleri isteyin
