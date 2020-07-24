---
title: Azure’da giden bağlantılar
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure 'un ortak internet hizmetleriyle iletişim kurması için VM 'nin nasıl çalıştığı açıklanır.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/24/2020
ms.author: allensu
ms.openlocfilehash: a2292dc789938b8bde709728f5bbffe661529cc2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87072636"
---
# <a name="outbound-connections-in-azure"></a>Azure’da giden bağlantılar

Azure Load Balancer, farklı mekanizmalarda giden bağlantı sağlar. Bu makalede senaryolar ve bunların nasıl yönetileceği açıklanmaktadır. Azure Load Balancer aracılığıyla giden bağlantılarla ilgili sorun yaşıyorsanız, [giden bağlantılar için sorun giderme kılavuzuna](../load-balancer/troubleshoot-outbound-connection.md)bakın.

>[!NOTE]
>Bu makalede Kaynak Yöneticisi dağıtımları ele alınmaktadır. Microsoft, üretim iş yükleri için Kaynak Yöneticisi önerir.

## <a name="terminology"></a>Terminoloji

| Terim | Uygulanabilir protokoller | Ayrıntılar|
| ---------|---------| -------|
| Kaynak ağ adresi çevirisi (SNAT) | TCP, UDP | Azure 'daki bir dağıtım, genel IP adresi alanında Azure dışındaki uç noktalarla iletişim kurabilir. Bir örnek, genel IP adresi alanındaki bir hedefe giden akışı başlattığında, Azure özel IP adresini dinamik olarak genel bir IP adresiyle eşleştirir. Bu eşleme oluşturulduktan sonra, bu çıkış kaynaklı akış için geri dönüş trafiği akışın kaynaklandığı özel IP adresine de ulaşabilir. Azure, bu işlevi gerçekleştirmek için **kaynak ağ adresi çevirisini (SNAT)** kullanır.|
| Bağlantı noktası, SNAT (PAT)| TCP, UDP |  Birden çok özel IP adresi tek bir genel IP adresinin arkasında olduğunda Azure, özel IP adreslerini maske/gizleme için **bağlantı noktası adres çevirisini (Pat)** kullanır. Kısa ömürlü bağlantı noktaları, PAT için kullanılır ve havuz boyutuna bağlı olarak [önceden ayrılır](#preallocatedports) . Ortak bir Load Balancer kaynağı, adanmış genel IP adreslerine sahip olmayan VM örnekleriyle ilişkilendirildiğinde, her giden bağlantı kaynağı yeniden yazılır. Kaynak, sanal ağ özel IP adresi alanından yük dengeleyicinin ön uç genel IP adresine yeniden yazılır. Genel IP adresi alanında, akışın 5 demet (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) benzersiz olmalıdır. Bağlantı noktası, TCP veya UDP IP protokolleriyle birlikte kullanılabilir. Çok sayıda akış tek bir genel IP adresinden kaynaklandığından, bu, özel kaynak IP adresini yeniden yazdıktan sonra bunu gerçekleştirmek için kısa ömürlü bağlantı noktaları (SNAT bağlantı noktaları) kullanılır. Geçici bağlantı noktası, SNAT bağlantı noktalarını UDP ve TCP 'ye karşı farklı şekilde ayırır.|
| SNAT bağlantı noktaları| TCP | SNAT bağlantı noktaları, belirli bir genel IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktalarıdır. Tek bir hedef IP adresine, bağlantı noktasına akış başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresine, bağlantı noktasına ve protokolüne birden çok TCP akışı için, her TCP akışı tek bir SNAT bağlantı noktası kullanır. Bu, akışların aynı ortak IP adresinden kaynaklandıklarında benzersiz olmasını sağlar ve aynı hedef IP adresine, bağlantı noktasına ve protokolüne gider. Her biri farklı bir hedef IP adresi, bağlantı noktası ve protokole birden çok akış, tek bir SNAT bağlantı noktasını paylaşır. Hedef IP adresi, bağlantı noktası ve protokol, genel IP adres alanındaki akışları ayırt etmek için ek kaynak bağlantı noktalarına gerek olmadan akış yapar.|
|SNAT bağlantı noktaları | UDP | UDP SNAT bağlantı noktaları, TCP SNAT bağlantı noktalarından farklı bir algoritma tarafından yönetilir.  Load Balancer, UDP için "bağlantı noktası kısıtlanmış koni NAT" olarak bilinen bir algoritma kullanır.  Hedef IP adresi, bağlantı noktasından bağımsız olarak her akış için bir SNAT bağlantı noktası kullanılır.|
| Tüken | - | SNAT bağlantı noktası kaynakları tükendiğinde, mevcut akışlar SNAT bağlantı noktalarını yayınlana kadar giden akışlar başarısız olur. Flow kapandığında geri kazanır SNAT bağlantı noktalarını Load Balancer ve boştaki akışlardan geri kazanma SNAT bağlantı noktaları için [4 dakikalık bir boşta kalma zaman aşımı süresi](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) kullanır. UDP SNAT bağlantı noktaları, kullanılan algoritmadaki fark nedeniyle TCP SNAT bağlantı noktalarından genellikle çok daha hızlı tükeiyor. Testi bu fark göz önüne alarak tasarlamanızı ve ölçeklendirmeniz gerekir.|
| SNAT bağlantı noktası yayın davranışı | TCP | Sunucu/istemci FINACK gönderirse, SNAT bağlantı noktası 240 saniye sonra serbest bırakılır. Bir RST görülüyorsa, SNAT bağlantı noktası 15 saniye sonra serbest bırakılır. Boşta kalma zaman aşımına ulaşıldığında, bağlantı noktası serbest bırakılır.|
| SNAT bağlantı noktası yayın davranışı | UDP |Boşta kalma zaman aşımına ulaşıldığında, bağlantı noktası serbest bırakılır.|
| SNAT bağlantı noktası yeniden kullanımı | TCP, UDP | Bağlantı noktası yayımlandıktan sonra, bağlantı noktası gerektiğinde yeniden kullanılabilir.  SNAT bağlantı noktalarını, belirli bir senaryo için en düşük ve en yüksek olan kullanılabilir bir sıra olarak düşünebilirsiniz ve yeni bağlantılar için kullanılabilir ilk SNAT bağlantı noktası kullanılır.|

### <a name="port-allocation-algorithm"></a><a name="preallocatedports"></a>Bağlantı noktası ayırma algoritması

Azure, PAT kullanılırken arka uç havuzunun boyutuna bağlı olarak kullanılabilir olan önceden ayrılmış SNAT bağlantı noktalarının sayısını belirlemede bir algoritma kullanır. Bir yük dengeleyiciyle ilişkili her genel IP adresi için, her IP Aktarım Protokolü için SNAT bağlantı noktaları olarak 64.000 bağlantı noktası bulunur. Aynı sayıda SNAT bağlantı noktası, sırasıyla UDP ve TCP için önceden ayrılır ve IP Aktarım Protokolü başına bağımsız olarak kullanılır.  Ancak, SNAT bağlantı noktası kullanımı, akışın UDP veya TCP olmasına bağlı olarak farklılık belirtir. Giden akışlar oluşturulduğunda, bu bağlantı noktaları dinamik olarak (önceden ayrılan sınıra kadar) ve akış kapandığında veya [boşta kalma zaman aşımları](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) gerçekleştiğinde serbest bırakılır. Bağlantı noktaları yalnızca akışları benzersiz hale getirmek için gerekliyse kullanılır.

#### <a name="default-snat-ports-allocated"></a><a name="snatporttable"></a>Ayrılan varsayılan SNAT bağlantı noktaları

Aşağıdaki tabloda, arka uç havuz boyutlarının katmanları için SNAT bağlantı noktası ön ayırmaları gösterilmektedir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına önceden ayrılmış SNAT bağlantı noktaları |
| --- | --- |
| 1-50 | 1.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 |

Arka uç havuzunuzun boyutunu değiştirmek, sağlanan akışlarınızdan bazılarını etkileyebilir:

- Arka uç havuzu boyutu bir sonraki katmana artıyorsa ve geçiş yaptığında, önceden ayrılan SNAT bağlantı noktalarınızın yarısı sonraki daha büyük arka uç havuz katmanına geçiş sırasında geri kazanılır. Geri kazanılan bir SNAT bağlantı noktasıyla ilişkili akışlar zaman aşımına uğrar ve yeniden oluşturulmalıdır. Yeni bir akış deneniyorsa, önceden ayrılan bağlantı noktaları kullanılabildiği sürece akış hemen başarılı olur.
- Arka uç havuzu boyutu düşürüyorsa ve daha düşük bir katmana geçerse, kullanılabilir SNAT bağlantı noktalarının sayısı artar. Bu durumda, ayrılmış olan SNAT bağlantı noktaları ve ilgili akışları etkilenmez.

## <a name="outbound-connections-scenario-overview"></a><a name="scenarios"></a>Giden bağlantılar senaryosuna genel bakış

| Senaryo | Yöntem | IP protokolleri | Açıklama |
|  --- | --- | --- | --- |
|  1. genel IP adresine sahip VM (Azure Load Balancer ile veya olmayan) | SNAT, bağlantı noktası aşağı olarak kullanılmıyor | TCP, UDP, ıCMP, ESP | Azure, tüm giden akışlar için örneğin NIC 'in IP yapılandırmasına atanan genel IP 'yi kullanır. Örnekte, tüm kısa ömürlü bağlantı noktaları kullanılabilir. VM 'nin yük dengeli olup olmadığı önemi yoktur. Bu senaryo diğerlerine göre önceliklidir. Bir VM 'ye atanan genel IP, 1:1 ilişkidir (1: çok) ve durum bilgisiz 1:1 NAT olarak uygulanır. |
| 2. bir VM ile ilişkili genel Load Balancer (VM/örnek üzerinde genel IP adresi yok) | Load Balancer ön uçları kullanarak bağlantı noktası geçici olarak (PAT) SNAT | TCP, UDP | Bu senaryoda, arka uç havuzuyla genel IP ön ucu arasında bağlantı oluşturmak için Load Balancer kaynağın bir yük dengeleyici kuralıyla yapılandırılması gerekir. Bu kural yapılandırmasını tamamlamayın, Senaryo 3 ' te açıklanmaktadır. Durum araştırmasının başarılı olması için kuralın arka uç havuzunda çalışma dinleyicisi olması gerekmez. VM bir giden akış oluşturduğunda, Azure, giden akışın özel kaynak IP adresini SNAT aracılığıyla genel Load Balancer ön uç 'nin genel IP adresine çevirir. Yük dengeleyicinin ön uç genel IP adresinin kısa ömürlü bağlantı noktaları, VM 'ler tarafından kaynaklı bireysel akışları ayırt etmek için kullanılır. SNAT, giden akışlar oluşturulduğunda, [önceden ayrılmış kısa ömürlü bağlantı noktalarını](#preallocatedports) dinamik olarak kullanır. Bu bağlamda, SNAT için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları olarak adlandırılır. SNAT bağlantı noktaları, [varsayılan SNAT bağlantı noktaları ayrılmış tablosunda](#snatporttable)açıklandığı şekilde önceden ayrılır. |
| 3. VM (Load Balancer, genel IP adresi yok) veya temel Iç Load Balancer ilişkili sanal makine | Bağlantı noktası geçici olarak SNAT (PAT) | TCP, UDP | VM bir giden akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini ortak kaynak IP adresine çevirir. Bu genel IP adresi **yapılandırılamaz**, ayrılamaz ve ABONELIĞIN genel IP kaynak sınırına göre sayılmaz. VM 'yi veya kullanılabilirlik kümesini veya sanal makine ölçek kümesini yeniden dağıtıyorsanız, bu genel IP adresi yayımlanır ve yeni bir genel IP adresi istenir. IP adreslerini beyaz listeye almak için bu senaryoyu kullanmayın. Bunun yerine, giden davranışı açıkça bildirdiğiniz Senaryo 1 veya 2 ' yi kullanın. SNAT bağlantı noktaları, [varsayılan SNAT bağlantı noktaları ayrılmış tablosunda](#snatporttable)açıklandığı şekilde önceden ayrılır.

## <a name="outbound-rules"></a><a name="outboundrules"></a>Giden kuralları

 Giden kuralları, genel [Standart Load Balancer](load-balancer-standard-overview.md)giden ağ adresi çevirisini yapılandırmayı basitleştirir.  Bu özelliği özel gereksinimlerinize göre ölçeklendirmeye ve ayarlamaya yönelik giden bağlantı üzerinde tam bildirime dayalı denetiminiz vardır. Bu bölüm yukarıda açıklanan senaryo 2 ' yi (B) genişletir.

![Giden kuralları Load Balancer](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Giden kuralları kullanarak, giden NAT 'ı sıfırdan tanımlamak için Load Balancer kullanabilirsiniz. Ayrıca varolan giden NAT 'nin davranışını ölçeklendirebilir ve ayarlayabilirsiniz.

Giden kuralları denetlemenize izin verir:

- Hangi sanal makinelerin genel IP adreslerine çevrilmesi gerekir.
- giden SNAT bağlantı noktalarının ayrılması gerekir.
- için giden çeviri sağlayacak olan protokoller.
- giden bağlantı boşta kalma zaman aşımı (4-120 dakika) için kullanılacak süre.
- boşta kalma zaman aşımı durumunda TCP sıfırlaması gönderileceğini belirtir
- tek bir kuralla hem TCP hem de UDP aktarım protokolleri

### <a name="outbound-rule-definition"></a>Giden kuralı tanımı

Tüm Load Balancer kuralları gibi giden kurallar da yük dengeleme ve gelen NAT kuralları ile aynı tanıdık sözdizimini izler: **ön uç**  +  **parametreleri**  +  **arka uç havuzu**. Giden bir kural _, arka uç havuzu tarafından belirtilen tüm sanal makineler_ IÇIN giden NAT 'yi _ön_uca çevrilecek şekilde yapılandırır.  _Parametreler_ , giden NAT algoritması üzerinde ek ayrıntılı denetim sağlar.

### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a>Birden çok IP adresiyle giden NAT 'yi ölçeklendirme

Ön uç tarafından sağlanan her ek IP adresi, Load Balancer SNAT bağlantı noktası olarak kullanılacak ek 64.000 kısa ömürlü bağlantı noktaları sağlar. Büyük ölçekli senaryoları planlamak için birden çok IP adresi kullanabilir ve giden kurallarını, [SNAT tükenmesi](troubleshoot-outbound-connection.md#snatexhaust) açısından yüksek desenleri azaltmak için kullanabilirsiniz.  

[Genel BIR IP önekini](https://aka.ms/lbpublicipprefix) doğrudan giden bir kuralla de kullanabilirsiniz.  Genel IP önekini kullanmak, Azure dağıtımından kaynaklanan akışların daha kolay ölçeklendirilmesi ve Basitleştirilmiş listesini sağlar. Load Balancer kaynağı içinde bir ön uç IP yapılandırmasını doğrudan genel IP adresi ön ekine başvuracak şekilde yapılandırabilirsiniz.  Bu, genel IP öneki üzerinde özel olarak denetim Load Balancer sağlar ve giden kuralı, giden bağlantılar için genel IP ön eki içinde bulunan tüm genel IP adreslerini otomatik olarak kullanır.  Genel IP öneki içindeki IP adreslerinden her biri, Load Balancer SNAT bağlantı noktası olarak kullanılacak IP adresi başına ek 64.000 kısa ömürlü bağlantı noktaları sağlar.

### <a name="outbound-flow-idle-timeout-and-tcp-reset"></a><a name="idletimeout"></a>Giden akış boşta kalma zaman aşımı ve TCP sıfırlaması

Giden kuralları, giden akış boşta kalma zaman aşımını denetlemek ve uygulamanızın gereksinimleriyle eşleştirmek için bir yapılandırma parametresi sağlar. Çıkış boşta kalma zaman aşımları varsayılan olarak 4 dakikadır. [Boşta kalma zaman aşımlarını yapılandırma](load-balancer-tcp-idle-timeout.md#tcp-idle-timeout)hakkında bilgi edinebilirsiniz. Load Balancer varsayılan davranışı, giden boşta kalma zaman aşımı ile erişildiğinde akışı sessizce düşürülemiyor.  `enableTCPReset`Parametresiyle, daha öngörülebilir bir uygulama davranışını etkinleştirebilir ve giden boşta kalma zaman aşımı süresi dolduğunda çift yönlü TCP sıfırlaması (TCP RST) göndermek isteyip istemediğinizi kontrol edebilirsiniz. Bölge kullanılabilirliği dahil Ayrıntılar için [boşta kalma zaman aşımı durumunda TCP sıfırlamayı](https://aka.ms/lbtcpreset) gözden geçirin.

### <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Giden bağlantıyı önlemek

Yük Dengeleme kuralları, giden NAT 'nin otomatik olarak programlamayı sağlar. Ancak, bazı senaryolar, davranışı denetlemenize veya iyileştirmenize olanak tanımak için, Yük Dengeleme kuralı tarafından giden NAT otomatik programlamayı devre dışı bırakmanızı veya zorunlu kılmanızı gerektirir.  
Bu parametreyi iki şekilde kullanabilirsiniz:

1. Bir yük dengeleme kuralı için giden SNAT 'yi devre dışı bırakarak giden SNAT için gelen IP adresinin kullanılması için isteğe bağlı gizleme
  
2. Gelen ve giden için aynı anda kullanılan bir IP adresinin giden SNAT parametrelerini ayarlayın.  Giden bir kuralın denetim geçirmesine izin vermek için otomatik giden NAT programlaması devre dışı bırakılmalıdır.  Örneğin, gelen için de kullanılan bir adresin SNAT bağlantı noktası ayırmasını değiştirmek için `disableOutboundSnat` parametresinin true olarak ayarlanması gerekir.  Gelen bir IP adresinin parametrelerini yeniden tanımlamak için bir giden kuralı kullanmayı denerseniz ve yük dengeleme kuralının giden NAT programlamayı yayımlamadıysanız, bir giden kuralı yapılandırma işlemi başarısız olur.

>[!IMPORTANT]
> Bu parametreyi true olarak ayarlarsanız ve giden bağlantıyı tanımlamak için bir giden kuralı yoksa, sanal makinenizin giden bağlantısı olmayacaktır.  VM 'nizin veya uygulamanızın bazı işlemleri giden bağlantıya sahip olmaya bağlı olabilir. Senaryonuzun bağımlılıklarını anladığınızdan ve bu değişikliği yapmanın etkileri olduğuna emin olun.

Bazen bir VM 'nin giden akış oluşturmasına izin verilmesi istenmeyen bir hale gelir. Ya da giden akışlarla hangi hedeflere ulaşılamayabileceğini veya hangi hedeflerin gelen akışlara başlayabileceğini yönetmek için bir gereksinim olabilir. Bu durumda, VM 'nin ulaşabileceği hedefleri yönetmek için [ağ güvenlik gruplarını](../virtual-network/security-overview.md) kullanabilirsiniz. Ayrıca, NSG 'leri kullanarak hangi ortak hedefin gelen akışları başlatabileceğini yönetebilirsiniz.

Yük dengeli bir VM 'ye NSG uyguladığınızda, [hizmet etiketlerine](../virtual-network/security-overview.md#service-tags) ve [varsayılan güvenlik kurallarına](../virtual-network/security-overview.md#default-security-rules)dikkat edin. VM 'nin Azure Load Balancer durum araştırma isteklerini alabilmeniz gerekir.

Bir NSG AZURE_LOADBALANCER varsayılan etiketten durum araştırma isteklerini engelliyorsa, sanal makine sistem durumu araştırmanız başarısız olur ve VM aşağı işaretlenir. Load Balancer, bu VM 'ye yeni akış göndermeyi durduruyor.

## <a name="scenarios-with-outbound-rules"></a>Giden kuralları olan senaryolar

| # | Senaryo| Ayrıntılar|
|---|---|---|
| I | Belirli bir genel IP adresi kümesine giden bağlantılar| Giden bağlantıların, belirli bir genel IP adresi kümesinden kaynaklanacak şekilde görüntülenmesini sağlamak için bir giden kuralı kullanabilirsiniz.  Bu kaynak genel IP adresi bir yük dengeleme kuralı veya bir yük dengeleme kuralı tarafından kullanılandan farklı bir genel IP adresi kümesi tarafından kullanılan ile aynı olabilir.  1. [Genel IP öneki](https://aka.ms/lbpublicipprefix) (veya genel IP önekinden ortak IP adresleri) oluştur 2. Ortak Standart Load Balancer 3 oluşturun. 4 kullanmak istediğiniz genel IP ön ekine (veya genel IP adreslerine) başvuran ön uçlar oluşturun. Bir arka uç havuzunu yeniden kullanın veya arka uç havuzu oluşturun ve VM 'Leri genel Load Balancer 5 ' in bir arka uç havuzuna yerleştirin. Ön uçların kullanıldığı bu VM 'Ler için giden NAT programı için genel Load Balancer giden bir kural yapılandırın. Yük Dengeleme kuralının giden için kullanılmasını istemiyorsanız, Yük Dengeleme kuralında giden SNAT 'yi devre dışı bırakmanız gerekir.
| SATA | SNAT bağlantı noktası ayırmayı Değiştir| [Arka uç havuzu boyutuna göre OTOMATIK SNAT bağlantı noktası ayırmayı](load-balancer-outbound-connections.md#preallocatedports)ayarlamak için giden kurallarını kullanabilirsiniz. Örneğin, giden NAT için tek bir genel IP adresi paylaşan iki sanal makineniz varsa, varsayılan 1024 bağlantı noktasından ayrılan SNAT bağlantı noktası sayısını, SNAT tükenmesi ile karşılaşıyorsanız yükseltmek isteyebilirsiniz. Her genel IP adresi en fazla 64.000 kısa ömürlü bağlantı noktasına katkıda bulunabilir.  Tek bir genel IP adresi ön ucuna sahip bir giden kuralı yapılandırırsanız, arka uç havuzundaki sanal makinelere Toplam 64.000 SNAT bağlantı noktası dağıtabilirsiniz.  İki VM için, en fazla 32.000 SNAT bağlantı noktası bir giden kuralla ayrılabilir (2x 32.000 = 64.000). Varsayılan olarak ayrılan SNAT bağlantı noktalarını ayarlamak için giden kurallarını kullanabilirsiniz. Varsayılan SNAT bağlantı noktası ayırmasının sağladığından daha fazla veya daha az ayırabilirsiniz. Giden bir kuralın tüm ön uçlarından gelen her genel IP adresi, SNAT bağlantı noktaları olarak kullanılmak üzere 64.000 kısa ömürlü bağlantı noktasına kadar katkıda bulunur.  Load Balancer SNAT bağlantı noktalarını 8 ' in katları halinde ayırır. 8 ' den bölünemez bir değer sağlarsanız, yapılandırma işlemi reddedilir.  Genel IP adresi sayısına göre kullanılabilir olandan daha fazla SNAT bağlantı noktası ayırmaya çalışırsanız, yapılandırma işlemi reddedilir.  Örneğin, VM başına 10.000 bağlantı noktası ve arka uç havuzunda 7 VM 'Ler ayırırsanız tek bir genel IP adresi paylaşabilir, yapılandırma reddedilir (7 x 10.000 SNAT bağlantı noktaları > 64.000 SNAT bağlantı noktaları).  Senaryoyu etkinleştirmek için giden kuralının ön ucunda daha fazla genel IP adresi ekleyebilirsiniz. Bağlantı noktası sayısı için 0 belirterek, [arka uç havuzu boyutuna bağlı olarak varsayılan SNAT bağlantı noktası ayırmaya](load-balancer-outbound-connections.md#preallocatedports) geri dönebilirsiniz. Bu durumda, ilk 50 sanal makine örnekleri 1024 bağlantı noktalarını alacak şekilde, 51-100 sanal makine örnekleri [tabloya](#snatporttable)göre 512 ve bu şekilde devam eder.|
| III| Yalnızca giden trafiği etkinleştir | Bir VM grubu için giden NAT sağlamak üzere ortak bir Standart Load Balancer kullanabilirsiniz. Bu senaryoda, herhangi bir ek kurala gerek duymadan bir giden kuralını kendisi kullanabilirsiniz.|
| IV | Yalnızca VM 'Ler için giden NAT (gelen) | Ortak bir Standart Load Balancer tanımlayın, sanal makineleri arka uç havuzuna yerleştirin ve giden NAT 'yi programlamak ve giden bağlantıları belirli bir genel IP adresinden kaynaklanacak şekilde yapılandırın. Ayrıca, giden bağlantıların kaynağını bir arada listelemek için genel bir IP öneki de kullanabilirsiniz. 1. Ortak Standart Load Balancer oluşturun. 2. Bir arka uç havuzu oluşturun ve VM 'Leri genel Load Balancer arka uç havuzuna yerleştirin. 3. Bu VM 'Ler için giden NAT programlamak için genel Load Balancer giden bir kural yapılandırın.
| V| İç Standart Load Balancer senaryolar için giden NAT| Dahili bir Standart Load Balancer kullanılırken, giden bağlantı açıkça bildirildiği sürece giden NAT kullanılamaz. Bir iç Standart Load Balancer arkasındaki VM 'Ler için giden bağlantıyı şu adımlarla oluşturmak üzere giden bağlantı kuralı kullanarak giden bağlantıyı tanımlayabilirsiniz: 1. Ortak Standart Load Balancer oluşturun. 2. Bir arka uç havuzu oluşturun ve VM 'Leri iç Load Balancer ek olarak ortak Load Balancer arka uç havuzuna yerleştirin. 3. Bu VM 'Ler için giden NAT programlamak için genel Load Balancer giden bir kural yapılandırın.|
| VI | Giden NAT için hem TCP & UDP protokollerini ortak bir Standart Load Balancer etkinleştirin | Ortak bir Standart Load Balancer kullanıldığında, belirtilen otomatik giden NAT programlama, Yük Dengeleme kuralının Aktarım Protokolü ile eşleşir. 1. Yük Dengeleme kuralında giden SNAT 'yi devre dışı bırakın. 2. Aynı Load Balancer bir giden kuralı yapılandırın. 3. VM 'niz tarafından zaten kullanılan arka uç havuzunu yeniden kullanın. 4. Giden kuralının bir parçası olarak "protokol": "tümü" ni belirtin. Yalnızca gelen NAT kuralları kullanıldığında, giden NAT sağlanmaz. 1. VM 'Leri bir arka uç havuzuna yerleştirin. 2. Genel IP adresleri veya genel IP öneki 3 olan bir veya daha fazla ön uç IP yapılandırması tanımlayın. Aynı Load Balancer bir giden kuralı yapılandırın. 4. Giden kuralının bir parçası olarak "Protocol": "All" belirtin |


## <a name="limitations"></a>Sınırlamalar

- Ön uç IP adresi başına kullanılabilir en fazla kısa ömürlü bağlantı noktası sayısı 64.000 ' dir.
- Yapılandırılabilir giden boşta kalma zaman aşımı aralığı 4 ila 120 dakikadır (240 ila 7200 saniye).
- Load Balancer giden NAT için ıCMP 'yi desteklemez.
- Giden kuralları, yalnızca bir NIC 'in birincil IP yapılandırmasına uygulanabilir.  Birden çok NIC desteklenir.
- VNet ve diğer Microsoft Platformu Hizmetleri olmayan Web çalışanı rolleri, ön VNet Hizmetleri ve diğer platform hizmetleri işlevinin yan etkisi nedeniyle yalnızca bir iç Standart Load Balancer kullanıldığında erişilebilir. İlgili hizmetin kendisi veya temel alınan platform hiçbir bildirimde bulunmaksızın değişmeksizin, bu yan etkiye güvenmeyin. Yalnızca dahili Standart Load Balancer kullandığınızda istenirse, açıkça giden bağlantı oluşturmanız gerektiğini varsaymanız gerekir. Bu makalede açıklanan senaryo 3 yok.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- [Azure Load Balancer hakkında sık sorulan sorularımıza](load-balancer-faqs.md)bakın.
- Standart genel Load Balancer [giden kuralları](load-balancer-outbound-rules-overview.md) hakkında daha fazla bilgi edinin.
- [Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
- Azure 'daki diğer bazı anahtar [ağ özellikleri](../networking/networking-overview.md) hakkında bilgi edinin.
