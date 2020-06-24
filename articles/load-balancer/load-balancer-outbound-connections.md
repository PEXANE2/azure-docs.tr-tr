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
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 0d16823f920695f84db74122c9a2ac07de0abdb2
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84907083"
---
# <a name="outbound-connections-in-azure"></a>Azure’da giden bağlantılar

Azure Load Balancer, bazı farklı mekanizmalarda müşteri dağıtımları için giden bağlantı sağlar. Bu makalede senaryoların ne olduğu, ne zaman uygulandığı, nasıl çalıştığı ve nasıl yönetileceği açıklanmaktadır. Azure Load Balancer aracılığıyla giden bağlantılarla ilgili sorun yaşıyorsanız, [giden bağlantılar için sorun giderme kılavuzuna](../load-balancer/troubleshoot-outbound-connection.md)bakın.

>[!NOTE] 
>Bu makalede yalnızca Kaynak Yöneticisi dağıtımları ele alınmaktadır. Azure 'daki tüm klasik dağıtım senaryoları için [giden bağlantıları (klasik)](load-balancer-outbound-connections-classic.md) gözden geçirin.

Azure 'daki bir dağıtım, genel IP adresi alanında Azure dışındaki uç noktalarla iletişim kurabilir. Bir örnek, genel IP adresi alanındaki bir hedefe giden akışı başlattığında, Azure özel IP adresini dinamik olarak genel bir IP adresiyle eşleştirir. Bu eşleme oluşturulduktan sonra, bu çıkış kaynaklı akış için geri dönüş trafiği akışın kaynaklandığı özel IP adresine de ulaşabilir.

Azure, bu işlevi gerçekleştirmek için kaynak ağ adresi çevirisini (SNAT) kullanır. Birden çok özel IP adresi tek bir genel IP adresinin arkasında olduğunda, Azure özel IP adreslerine maske eklemek için [bağlantı noktası adres çevirisini (Pat)](#pat) kullanır. Kısa ömürlü bağlantı noktaları, PAT için kullanılır ve havuz boyutuna bağlı olarak [önceden ayrılır](#preallocatedports) .

Birden çok [giden senaryo](#scenarios)vardır. Bu senaryoları gerektiğinde birleştirebilirsiniz. Özellikleri, kısıtlamaları ve desenleri, dağıtım modeliniz ve Uygulama senaryonuz için uygun olarak anlamak üzere dikkatlice gözden geçirin. [Bu senaryoları yönetme](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)kılavuzunu gözden geçirin.

>[!IMPORTANT] 
>Standart Load Balancer ve standart genel IP, giden bağlantıya yeni yetenekler ve farklı davranışlar getirir.  Bunlar temel SKU 'Lar ile aynı değildir.  Standart SKU 'Lar ile çalışırken giden bağlantı isterseniz, standart genel IP adresleriyle veya standart ortak Load Balancer açıkça tanımlamanız gerekir.  Bu, dahili bir Standart Load Balancer kullanırken giden bağlantı oluşturmayı içerir.  Her zaman standart bir genel Load Balancer giden kuralları kullanmanızı öneririz.  [3. senaryo](#defaultsnat) standart SKU ile kullanılamaz.  Bu, bir iç Standart Load Balancer kullanıldığında, giden bağlantı istenirse arka uç havuzundaki VM 'Ler için giden bağlantı oluşturmak üzere gerekli adımları uygulamanız gerekir.  Giden bağlantı bağlamında, tek bir tek başına VM, tüm VM 'ler bir kullanılabilirlik kümesinde, bir VMSS içindeki tüm örnekler bir grup olarak davranır. Bu, bir kullanılabilirlik kümesindeki tek bir VM standart SKU ile ilişkili ise, bu kullanılabilirlik kümesindeki tüm sanal makine örnekleri, tek bir örnek doğrudan ilişkili olmasa bile standart SKU ile ilişkilendirildikleri kurallarla aynı kurallara göre davranır. Bu davranış, bir yük dengeleyiciye bağlı birden çok ağ arabirimi kartına sahip tek başına bir VM söz konusu olduğunda da gözlemlenmiştir. Tek başına bir NIC eklenirse aynı davranışa sahip olur. Genel kavramları anlamak için tüm belgeyi dikkatle gözden geçirin, SKU 'Lar arasındaki farklılıklar için [Standart Load Balancer](load-balancer-standard-overview.md) gözden geçirin ve [giden kuralları](load-balancer-outbound-rules-overview.md)gözden geçirin.  Giden kuralları kullanmak, giden bağlantıların tüm yönleri üzerinde ayrıntılı denetim sağlar.

## <a name="scenario-overview"></a><a name="scenarios"></a>Senaryoya genel bakış

Azure Load Balancer ve ilgili kaynaklar [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)kullanırken açıkça tanımlanır.  Azure Şu anda Azure Resource Manager kaynaklarına giden bağlantı sağlamak için üç farklı yöntem sunmaktadır. 

| SKU'lar | Senaryo | Yöntem | IP protokolleri | Description |
| --- | --- | --- | --- | --- |
| Standart, temel | [1. örnek düzeyi genel IP adresine sahip VM (Load Balancer ile veya olmayan)](#ilpip) | SNAT, bağlantı noktası aşağı olarak kullanılmıyor | TCP, UDP, ıCMP, ESP | Azure, örneğin NIC 'in IP yapılandırmasına atanan genel IP 'yi kullanır. Örnekte, tüm kısa ömürlü bağlantı noktaları kullanılabilir. Standart Load Balancer kullanırken, sanal makineye genel IP atanmışsa [giden kuralları](load-balancer-outbound-rules-overview.md) desteklenmez. |
| Standart, temel | [2. bir VM ile ilişkili ortak Load Balancer (örnekte genel IP adresi yok)](#lb) | Load Balancer ön uçları kullanarak bağlantı noktası geçici olarak (PAT) SNAT | TCP, UDP |Azure, genel Load Balancer ön uçlarının genel IP adresini birden çok özel IP adresi ile paylaşır. Azure, ön uçların kısa ömürlü bağlantı noktalarını kullanır. Standart Load Balancer kullanırken, giden bağlantıyı açıkça tanımlamak için [giden kurallarını](load-balancer-outbound-rules-overview.md) kullanmanız gerekir. |
| hiçbiri veya temel | [3. tek başına VM (Load Balancer yok, genel IP adresi yok)](#defaultsnat) | Bağlantı noktası geçici olarak SNAT (PAT) | TCP, UDP | Azure, SNAT için bir genel IP adresi otomatik olarak atar, bu genel IP adresini kullanılabilirlik kümesinin birden çok özel IP adresi ile paylaşır ve bu genel IP adresinin kısa ömürlü bağlantı noktalarını kullanır. Bu senaryo, önceki senaryolar için bir geri dönüş olur. Görünürlük ve denetime ihtiyacınız varsa bunu önermiyoruz. |

Bir VM 'nin genel IP adresi alanında Azure dışındaki uç noktalarla iletişim kurmasını istemiyorsanız, erişimi gerektiği şekilde engellemek için ağ güvenlik grupları (NSG 'ler) kullanabilirsiniz. [Giden bağlantının önlenmesi](#preventoutbound) bölümünde NSG 'ler daha ayrıntılı şekilde anlatılmaktadır. Bir sanal ağın herhangi bir giden erişimi olmadan tasarlanması, uygulanması ve yönetilmesi ile ilgili yönergeler bu makalenin kapsamı dışındadır.

### <a name="scenario-1-vm-with-public-ip-address"></a><a name="ilpip"></a>Senaryo 1: ortak IP adresine sahip VM

Bu senaryoda, sanal makineye atanmış bir genel IP vardır. Giden bağlantılar söz konusu olduğunda, VM 'nin yük dengeli olup olmadığı önemi yoktur. Bu senaryo diğerlerine göre önceliklidir. Ortak bir IP adresi kullanıldığında, VM tüm giden akışlar için genel IP adresini kullanır.  

Bir VM 'ye atanan genel IP, 1:1 ilişkidir (1: çok) ve durum bilgisiz 1:1 NAT olarak uygulanır.  Bağlantı noktası (PAT) kullanılmaz ve VM 'nin kullanılabilir tüm kısa ömürlü bağlantı noktaları kullanılabilir.

Uygulamanız birçok giden akışı başlatırsa ve SNAT bağlantı noktası tükenmesi ile karşılaşırsanız, [SNAT kısıtlamalarını azaltmak Için genel BIR IP adresi](../load-balancer/troubleshoot-outbound-connection.md#assignilpip)atamayı göz önünde bulundurun. [SNAT tükenmesi yönetimini](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) tamamen inceleyin.

### <a name="scenario-2-load-balanced-vm-without-a-public-ip-address"></a><a name="lb"></a>Senaryo 2: genel IP adresi olmayan yük dengeli VM

Bu senaryoda, VM ortak Load Balancer arka uç havuzunun bir parçasıdır. SANAL makineye atanmış bir genel IP adresi yok. Load Balancer kaynak, arka uç havuzuyla genel IP ön ucu arasında bağlantı oluşturmak için bir yük dengeleyici kuralıyla yapılandırılmalıdır.

Bu kural yapılandırmasını tamamlamayın, davranış [genel IP olmadan tek BAŞıNA VM](#defaultsnat)için senaryoda açıklanacaktır. Durum araştırmasının başarılı olması için kuralın arka uç havuzunda çalışma dinleyicisi olması gerekmez.

Yük dengeli VM bir giden akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini genel Load Balancer ön ucu genel IP adresine çevirir. Azure, bu işlevi gerçekleştirmek için SNAT 'yi kullanır. Azure, genel bir IP adresinin arkasında birden çok özel IP adresini geçici olarak çözmek için [Pat](#pat) 'yi de kullanır. 

Yük dengeleyicinin genel IP adresi ön ucu için kısa ömürlü bağlantı noktaları, VM 'nin kaynaklandığı bireysel akışları ayırt etmek için kullanılır. SNAT, giden akışlar oluşturulduğunda, [önceden ayrılmış kısa ömürlü bağlantı noktalarını](#preallocatedports) dinamik olarak kullanır. Bu bağlamda, SNAT için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları olarak adlandırılır.

SNAT bağlantı noktaları, [SNAT ve Pat 'ı anlama](#snat) bölümünde açıklandığı gibi önceden ayrılır. Bu, tükenebilir sınırlı bir kaynaktır. Bunların [nasıl kullanıldığını anlamak önemlidir.](#pat) Bu tüketim için nasıl tasarlanacağını ve gerekirse etkisini anlamak için, [SNAT tükenmesi yönetimini](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)gözden geçirin.

[Birden çok genel IP adresi Load Balancer temel ile ilişkilendirildiğinde](load-balancer-multivip-overview.md), bu genel IP adreslerinden herhangi biri giden akışlar için aday olur ve rastgele bir seçilir.  

Load Balancer temel ile giden bağlantıların sistem durumunu izlemek için, Load Balancer ve [Uyarı olay günlüklerinin](load-balancer-monitor-log.md#alert-event-log) [Azure IZLEYICI günlüklerini](load-balancer-monitor-log.md) kullanarak SNAT bağlantı noktası Tükenme iletilerini izleyebilirsiniz.

### <a name="scenario-3-standalone-vm-without-a-public-ip-address"></a><a name="defaultsnat"></a>Senaryo 3: genel IP adresi olmayan tek başına VM

Bu senaryoda, VM ortak bir Load Balancer havuzunun parçası değildir (bir iç Standart Load Balancer havuzunun parçası değildir) ve kendisine atanmış bir genel IP adresi yoktur. VM bir giden akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini ortak kaynak IP adresine çevirir. Bu giden akış için kullanılan genel IP adresi yapılandırılamaz ve aboneliğin genel IP kaynak sınırına göre sayılmaz. Bu genel IP adresi size ait değil ve ayrılamaz. VM 'yi veya kullanılabilirlik kümesini veya sanal makine ölçek kümesini yeniden dağıtıyorsanız, bu genel IP adresi yayımlanır ve yeni bir genel IP adresi istenir. IP adreslerini beyaz listeye almak için bu senaryoyu kullanmayın. Bunun yerine, çıkış senaryosunu ve giden bağlantı için kullanılacak genel IP adresini açıkça bildirdiğiniz diğer iki senaryonun birini kullanın.

>[!IMPORTANT] 
>Bu senaryo __yalnızca__ bir iç temel Load Balancer eklendiğinde de geçerlidir. Bir VM 'ye dahili Standart Load Balancer eklendiğinde Senaryo 3 __kullanılamaz__ .  Dahili bir Standart Load Balancer kullanmaya ek olarak [Senaryo 1](#ilpip) veya [Senaryo 2](#lb) ' i açıkça oluşturmanız gerekir.

Azure, bu işlevi gerçekleştirmek için bağlantı noktası geçici ([Pat](#pat)) ile SNAT 'yi kullanır. Bu senaryo, kullanılan IP adresi üzerinde denetim olmaması dışında [Senaryo 2](#lb)' ye benzer. Bu senaryo 1 ve 2 ' nin bulunmadığı durumlarda için bir geri dönüş senaryosudur. Giden adres üzerinde denetim istiyorsanız bu senaryoyu önermiyoruz. Giden bağlantılar uygulamanızın önemli bir parçasıysa, başka bir senaryo seçmeniz gerekir.

SNAT bağlantı noktaları, [SNAT ve Pat 'ı anlama](#snat) bölümünde açıklandığı gibi önceden ayrılır.  Bir kullanılabilirlik kümesini paylaşan VM 'lerin sayısı, hangi ön ayırma katmanının uygulanacağını belirler.  Kullanılabilirlik kümesi olmayan tek başına VM, ön ayırmayı belirleme (1024 SNAT bağlantı noktası) amaçları için etkin bir şekilde 1 havuzu olur. SNAT bağlantı noktaları, tükenebilir sınırlı bir kaynaktır. Bunların [nasıl kullanıldığını anlamak önemlidir.](#pat) Bu tüketim için nasıl tasarlanacağını ve gerekirse etkisini anlamak için, [SNAT tükenmesi yönetimini](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)gözden geçirin.

### <a name="multiple-combined-scenarios"></a><a name="combinations"></a>Çoklu, Birleşik senaryolar

Belirli bir sonuca ulaşmak için yukarıdaki bölümlerde açıklanan senaryoları birleştirebilirsiniz. Birden çok senaryo mevcut olduğunda, bir öncelik sırası geçerlidir: [Senaryo 1](#ilpip) [Senaryo 2](#lb) ve [3](#defaultsnat)' ten önceliklidir. [Senaryo 2](#lb) geçersiz kılmaları [Senaryo 3](#defaultsnat).

Bir örnek, uygulamanın sınırlı sayıda hedefe giden bağlantıları yoğun bir şekilde kullandığı ancak aynı zamanda gelen akışları bir Load Balancer ön uç üzerinden aldığından Azure Resource Manager bir dağıtımdır. Bu durumda, yardım için 1 ve 2 senaryolarını birleştirebilirsiniz. Ek desenler için, [SNAT tükenmesi yönetimini](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust)gözden geçirin.

### <a name="multiple-frontends-for-outbound-flows"></a><a name="multife"></a>Giden akışlar için birden çok ön uç

#### <a name="standard-load-balancer"></a>Standart Load Balancer

Standart Load Balancer, [birden çok (genel) IP ön uçları](load-balancer-multivip-overview.md) mevcut olduğunda giden akışlar için tüm adayları aynı anda kullanır. Giden bağlantılar için bir yük dengeleme kuralı etkinleştirildiyse, her ön uç, kullanılabilir önceden ayrılmış SNAT bağlantı noktalarının sayısını çarpar.

Yeni bir yük dengeleme kuralı seçeneği ile giden bağlantılar için bir ön uç IP adresinin kullanılmasını Önle seçeneğini belirleyebilirsiniz:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Normalde, `disableOutboundSnat` seçeneği _false_ olur ve bu kuralın, Yük Dengeleme kuralının arka uç havuzundaki ilişkili VM 'ler için giden SNAT programları olduğunu belirtir. `disableOutboundSnat`Load Balancer, bu yük dengeleme kuralının arka uç havuzundaki VM 'ler için giden bağlantılar için ilgili ön uç IP adresini kullanmasını engellemek üzere _true_ olarak değiştirilebilir.  Ayrıca, dış akışlar için de belirli bir IP adresini aynı zamanda [birden çok, Birleşik senaryolarda](#combinations) da açıklandığı gibi atayabilirsiniz.

#### <a name="load-balancer-basic"></a>Load Balancer temel

Load Balancer temel, dış akışlar için [birden çok (genel) IP ön uçları](load-balancer-multivip-overview.md) aday olduğunda giden akışlar için kullanılacak tek bir ön uç seçer. Bu seçim yapılandırılamaz ve seçim algoritmasını rastgele olacak şekilde göz önünde bulundurmanız gerekir. Giden akışlar için belirli bir IP adresini [birden çok, Birleşik senaryolar](#combinations)bölümünde açıklandığı gibi belirtebilirsiniz.

### <a name="availability-zones"></a><a name="az"></a>Kullanılabilirlik Alanları

[Kullanılabilirlik alanları ile standart Load Balancer](load-balancer-standard-availability-zones.md)kullanırken, bölgesel olarak yedekli ön uçlar, bölgesel olarak YEDEKLI giden SNAT BAĞLANTıLARı ve SNAT programlama içi bölge hatası verebilir.  ZGen ön uçları kullanıldığında, giden SNAT bağlantıları Fate, ait oldukları bölge ile paylaşılır.

## <a name="understanding-snat-and-pat"></a><a name="snat"></a>SNAT ve PAT 'yi anlama

### <a name="port-masquerading-snat-pat"></a><a name="pat"></a>Bağlantı noktası, SNAT (PAT)

Ortak bir Load Balancer kaynağı, adanmış genel IP adreslerine sahip olmayan VM örnekleriyle ilişkilendirildiğinde, her giden bağlantı kaynağı yeniden yazılır. Kaynak, sanal ağ özel IP adresi alanından yük dengeleyicinin ön uç genel IP adresine yeniden yazılır. Genel IP adresi alanında, akışın 5 demet (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) benzersiz olmalıdır. Bağlantı noktası, TCP veya UDP IP protokolleriyle birlikte kullanılabilir.

Çok sayıda akış tek bir genel IP adresinden kaynaklandığından, bu, özel kaynak IP adresini yeniden yazdıktan sonra bunu gerçekleştirmek için kısa ömürlü bağlantı noktaları (SNAT bağlantı noktaları) kullanılır. Geçici bağlantı noktası, SNAT bağlantı noktalarını UDP ve TCP 'ye karşı farklı şekilde ayırır.

#### <a name="tcp-snat-ports"></a><a name="tcp"></a>TCP SNAT bağlantı noktaları

Tek bir hedef IP adresine, bağlantı noktasına akış başına bir SNAT bağlantı noktası kullanılır. Aynı hedef IP adresine, bağlantı noktasına ve protokolüne birden çok TCP akışı için, her TCP akışı tek bir SNAT bağlantı noktası kullanır. Bu, akışların aynı ortak IP adresinden kaynaklandıklarında benzersiz olmasını sağlar ve aynı hedef IP adresine, bağlantı noktasına ve protokolüne gider. 

Her biri farklı bir hedef IP adresi, bağlantı noktası ve protokole birden çok akış, tek bir SNAT bağlantı noktasını paylaşır. Hedef IP adresi, bağlantı noktası ve protokol, genel IP adres alanındaki akışları ayırt etmek için ek kaynak bağlantı noktalarına gerek olmadan akış yapar.

#### <a name="udp-snat-ports"></a><a name="udp"></a>UDP SNAT bağlantı noktaları

UDP SNAT bağlantı noktaları, TCP SNAT bağlantı noktalarından farklı bir algoritma tarafından yönetilir.  Load Balancer, UDP için "bağlantı noktası kısıtlanmış koni NAT" olarak bilinen bir algoritma kullanır.  Hedef IP adresi, bağlantı noktasından bağımsız olarak her akış için bir SNAT bağlantı noktası kullanılır.

#### <a name="snat-port-reuse"></a>SNAT bağlantı noktası yeniden kullanımı

Bağlantı noktası yayımlandıktan sonra, bağlantı noktası gerektiğinde yeniden kullanılabilir.  SNAT bağlantı noktalarını, belirli bir senaryo için en düşük ve en yüksek olan kullanılabilir bir sıra olarak düşünebilirsiniz ve yeni bağlantılar için kullanılabilir ilk SNAT bağlantı noktası kullanılır. 
 
#### <a name="exhaustion"></a>Tüken

SNAT bağlantı noktası kaynakları tükendiğinde, mevcut akışlar SNAT bağlantı noktalarını yayınlana kadar giden akışlar başarısız olur. Flow kapandığında geri kazanır SNAT bağlantı noktalarını Load Balancer ve boştaki akışlardan geri kazanma SNAT bağlantı noktaları için [4 dakikalık bir boşta kalma zaman aşımı süresi](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) kullanır.

UDP SNAT bağlantı noktaları, kullanılan algoritmadaki fark nedeniyle TCP SNAT bağlantı noktalarından genellikle çok daha hızlı tükeiyor. Testi bu fark göz önüne alarak tasarlamanızı ve ölçeklendirmeniz gerekir.

Genellikle SNAT bağlantı noktası tükenmesi ' ne yol açabilecek koşulları azaltmak için, SNAT 'yi [yönetme](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) bölümünü gözden geçirin.

### <a name="ephemeral-port-preallocation-for-port-masquerading-snat-pat"></a><a name="preallocatedports"></a>Bağlantı noktası için geçici bağlantı noktası önayırması (PAT)

Azure, bağlantı noktası geçici olarak ([Pat](#pat)) kullanılırken arka uç havuzunun boyutuna bağlı olarak kullanılabilir önceden ayrılmış SNAT bağlantı noktalarının sayısını belirlemede bir algoritma kullanır. SNAT bağlantı noktaları, belirli bir genel IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktalarıdır. Bir yük dengeleyiciyle ilişkili her genel IP adresi için, her IP Aktarım Protokolü için SNAT bağlantı noktaları olarak 64.000 bağlantı noktası bulunur.

Aynı sayıda SNAT bağlantı noktası, sırasıyla UDP ve TCP için önceden ayrılır ve IP Aktarım Protokolü başına bağımsız olarak kullanılır.  Ancak, SNAT bağlantı noktası kullanımı, akışın UDP veya TCP olmasına bağlı olarak farklılık belirtir.

>[!IMPORTANT]
>Standart SKU SNAT programlama, IP Aktarım Protokolü başına ve yük dengeleme kuralından türetilir.  Yalnızca bir TCP Yük Dengeleme kuralı varsa, SNAT yalnızca TCP için kullanılabilir. Yalnızca bir TCP Yük Dengeleme kuralınız varsa ve UDP için giden SNAT gerekiyorsa, aynı ön uçta aynı arka uç havuzuna bir UDP Yük Dengeleme kuralı oluşturun.  Bu, UDP için SNAT programlamayı tetikler.  Çalışma kuralı veya sistem durumu araştırması gerekli değildir.  Yük Dengeleme kuralında belirtilen Aktarım Protokolü ne olursa olsun, temel SKU SNAT her zaman her iki IP Aktarım Protokolü için SNAT.

Azure, SNAT bağlantı noktalarını her VM 'nin NIC 'sinin IP yapılandırmasına önceden ayırır. Havuza bir IP yapılandırması eklendiğinde, bu IP yapılandırması için, arka uç havuz boyutuna bağlı olarak SNAT bağlantı noktaları önceden ayrılır. Giden akışlar oluşturulduğunda, [Pat](#pat) dinamik olarak (önceden ayrılan sınıra kadar) kullanır ve akış kapandığında veya [boşta kalma zaman aşımları](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) olduğunda bu bağlantı noktalarını yayınlar.

Aşağıdaki tabloda, arka uç havuz boyutlarının katmanları için SNAT bağlantı noktası ön ayırmaları gösterilmektedir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına önceden ayrılmış SNAT bağlantı noktaları|
| --- | --- |
| 1-50 | 1.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 |

>[!NOTE]
> [Birden çok](load-balancer-multivip-overview.md)ön uç içeren standart Load Balancer kullanırken, her bir ön uç IP adresi önceki tabloda kullanılabilir SNAT bağlantı noktalarının sayısını çarpar. Örneğin, her biri ayrı bir ön uç IP adresi olan 2 Yük Dengeleme kuralıyla 50 VM 'nin bir arka uç havuzu, kural başına 2048 (2x 1024) SNAT bağlantı noktasını kullanır. Bkz. [birden çok ön uç](#multife)için ayrıntılar.

Kullanılabilir SNAT bağlantı noktası sayısının, akış sayısına doğrudan çevrilemez olduğunu unutmayın. Tek bir SNAT bağlantı noktası, birden çok benzersiz hedef için yeniden kullanılabilir. Bağlantı noktaları yalnızca akışları benzersiz hale getirmek için gerekliyse kullanılır. Tasarım ve azaltma Kılavuzu için, [Bu tüketilme kaynağını yönetme](../load-balancer/troubleshoot-outbound-connection.md#snatexhaust) ve [Pat](#pat)'yi açıklayan bölüm hakkında bölümüne bakın.

Arka uç havuzunuzun boyutunu değiştirmek, sağlanan akışlarınızdan bazılarını etkileyebilir. Arka uç havuzu boyutu bir sonraki katmana artıyorsa ve geçiş yaptığında, önceden ayrılan SNAT bağlantı noktalarınızın yarısı sonraki daha büyük arka uç havuz katmanına geçiş sırasında geri kazanılır. Geri kazanılan bir SNAT bağlantı noktasıyla ilişkili akışlar zaman aşımına uğrar ve yeniden oluşturulmalıdır. Yeni bir akış deneniyorsa, önceden ayrılan bağlantı noktaları kullanılabildiği sürece akış hemen başarılı olur.

Arka uç havuzu boyutu düşürüyorsa ve daha düşük bir katmana geçerse, kullanılabilir SNAT bağlantı noktalarının sayısı artar. Bu durumda, ayrılmış olan SNAT bağlantı noktaları ve ilgili akışları etkilenmez.

SNAT bağlantı noktaları ayırmaları, IP aktarım protokolüne özgüdür (TCP ve UDP ayrı tutulur) ve aşağıdaki koşullarda yayımlanır:

### <a name="tcp-snat-port-release"></a>TCP SNAT bağlantı noktası sürümü

- Sunucu/istemci FINACK gönderirse, SNAT bağlantı noktası 240 saniye sonra serbest bırakılır.
- Bir RST görülüyorsa, SNAT bağlantı noktası 15 saniye sonra serbest bırakılır.
- Boşta kalma zaman aşımına ulaşıldığında, bağlantı noktası serbest bırakılır.

### <a name="udp-snat-port-release"></a>UDP SNAT bağlantı noktası sürümü

- Boşta kalma zaman aşımına ulaşıldığında, bağlantı noktası serbest bırakılır.

## <a name="discovering-the-public-ip-that-a-vm-uses"></a><a name="discoveroutbound"></a>Bir VM 'nin kullandığı genel IP 'yi keşfetme
Giden bir bağlantının genel kaynak IP adresini belirlemenin birçok yolu vardır. OpenDNS, sanal makinenizin genel IP adresini gösterebilmeniz için bir hizmet sağlar. 

Nslookup komutunu kullanarak, myip.opendns.com adı için OpenDNS çözümleyiciye bir DNS sorgusu gönderebilirsiniz. Hizmet, sorguyu göndermek için kullanılan kaynak IP adresini döndürür. Aşağıdaki sorguyu sanal makinenizde çalıştırdığınızda, yanıt o VM için kullanılan genel IP 'dir:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Giden bağlantıyı önlemek
Bazen bir VM 'nin giden akış oluşturmasına izin verilmesi istenmeyen bir hale gelir. Ya da giden akışlarla hangi hedeflere ulaşılamayabileceğini veya hangi hedeflerin gelen akışlara başlayabileceğini yönetmek için bir gereksinim olabilir. Bu durumda, VM 'nin ulaşabileceği hedefleri yönetmek için [ağ güvenlik gruplarını](../virtual-network/security-overview.md) kullanabilirsiniz. Ayrıca, NSG 'leri kullanarak hangi ortak hedefin gelen akışları başlatabileceğini yönetebilirsiniz.

Yük dengeli bir VM 'ye NSG uyguladığınızda, [hizmet etiketlerine](../virtual-network/security-overview.md#service-tags) ve [varsayılan güvenlik kurallarına](../virtual-network/security-overview.md#default-security-rules)dikkat edin. VM 'nin Azure Load Balancer durum araştırma isteklerini alabilmeniz gerekir. 

Bir NSG AZURE_LOADBALANCER varsayılan etiketten durum araştırma isteklerini engelliyorsa, sanal makine sistem durumu araştırmanız başarısız olur ve VM aşağı işaretlenir. Load Balancer, bu VM 'ye yeni akış göndermeyi durduruyor.

## <a name="connections-to-azure-storage-in-the-same-region"></a>Azure depolama ile aynı bölgedeki bağlantılar

Yukarıdaki senaryolar aracılığıyla giden bağlantıların olması, VM ile aynı bölgedeki depolamaya bağlanmak için gerekli değildir. Bunu istemiyorsanız, yukarıda açıklandığı gibi ağ güvenlik gruplarını (NSG 'ler) kullanın. Diğer bölgelerdeki depolama bağlantısı için giden bağlantı gereklidir. Lütfen aynı bölgedeki bir VM 'den depolamaya bağlanırken, depolama tanılama günlüklerindeki kaynak IP adresinin, sanal makinenizin genel IP adresi değil, iç sağlayıcı adresi olacağını unutmayın. Depolama hesabınıza erişimi aynı bölgedeki bir veya daha fazla sanal ağ alt ağında bulunan VM 'Lerle kısıtlamak istiyorsanız, depolama hesabı güvenlik duvarını yapılandırırken ortak IP adresinizi değil [sanal ağ hizmet uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) kullanın. Hizmet uç noktaları yapılandırıldıktan sonra, sanal ağ özel IP adresinizi, iç sağlayıcı adresinde değil, depolama tanılama günlüklerinizin içinde görürsünüz.

## <a name="azure-load-balancer-outbound-rules"></a><a name="outboundrules"></a>Giden kuralları Azure Load Balancer

Azure Load Balancer, gelen öğesine ek olarak bir sanal ağdan giden bağlantı sağlar.  Giden kuralları, genel [Standart Load Balancer](load-balancer-standard-overview.md)giden ağ adresi çevirisini yapılandırmayı basitleştirir.  Bu özelliği özel gereksinimlerinize göre ölçeklendirmeye ve ayarlamaya yönelik giden bağlantı üzerinde tam bildirime dayalı denetiminiz vardır.

![Giden kuralları Load Balancer](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Giden kuralları ile Load Balancer şu şekilde kullanabilirsiniz: 
- giden NAT 'yi sıfırdan tanımlayın.
- Varolan giden NAT 'nin davranışını ölçeklendirin ve ayarlayın. 

Giden kuralları denetlemenize izin verir:
- Hangi sanal makinelerin genel IP adreslerine çevrilmesi gerekir. 
- [gıden SNAT bağlantı noktalarının](load-balancer-outbound-connections.md#snat) ayrılması gerekir.
- için giden çeviri sağlayacak olan protokoller.
- giden bağlantı boşta kalma zaman aşımı (4-120 dakika) için kullanılacak süre.
- boşta kalma zaman aşımı durumunda TCP sıfırlaması gönderileceğini belirtir

Giden kuralları, [giden bağlantılar](load-balancer-outbound-connections.md) makalesinde açıklanan [Senaryo 2](load-balancer-outbound-connections.md#lb) ' yi genişleterek senaryo önceliği olduğu gibi kalır.

### <a name="outbound-rule"></a>Giden kuralı

Tüm Load Balancer kuralları gibi giden kurallar da yük dengeleme ve gelen NAT kuralları ile aynı tanıdık sözdizimini izler:

**ön uç**  +  **Parametreler**  +  **arka uç havuzu**

Giden bir kural _, arka uç havuzu tarafından belirtilen tüm sanal makineler_ IÇIN giden NAT 'yi _ön_uca çevrilecek şekilde yapılandırır.  Ve _Parametreler_ giden NAT algoritması üzerinde ek ayrıntılı denetim sağlar.

API sürümü "2018-07-01", aşağıdaki gibi yapılandırılmış bir giden kural tanımına izin verir:

```json
      "outboundRules": [
        {
          "frontendIPConfigurations": [ list_of_frontend_ip_configuations ],
          "allocatedOutboundPorts": number_of_SNAT_ports,
          "idleTimeoutInMinutes": 4 through 66,
          "enableTcpReset": true | false,
          "protocol": "Tcp" | "Udp" | "All",
          "backendAddressPool": backend_pool_reference,
        }
      ]
```

>[!NOTE]
>Etkin giden NAT yapılandırması, tüm giden kuralların ve yük dengeleme kurallarının bir bileşiği olur. Giden kuralları Yük Dengeleme kurallarına göre artımlıdır. Bir VM için birden çok kural geçerli olduğunda, etkin giden NAT çevirisini yönetmek üzere [bir yük dengeleme kuralı için gıden NAT 'yi devre dışı bırakmayı](#disablesnat) gözden geçirin. Yük Dengeleme kuralıyla aynı genel IP adresini kullanan bir giden kuralı tanımlarken [gıden SNAT 'yi devre dışı bırakmanız](#disablesnat) gerekir.

#### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a>Birden çok IP adresiyle giden NAT 'yi ölçeklendirme

Giden kuralı yalnızca tek bir genel IP adresiyle kullanılabilir ancak giden kuralları, giden NAT ölçeklendirme için yapılandırma yükünü kolaylaştırır. Büyük ölçekli senaryoları planlamak için birden çok IP adresi kullanabilir ve giden kurallarını, [SNAT tükenmesi](troubleshoot-outbound-connection.md#snatexhaust) açısından yüksek desenleri azaltmak için kullanabilirsiniz.  

Ön uç tarafından sağlanan her ek IP adresi, Load Balancer SNAT bağlantı noktası olarak kullanılacak olan 64.000 kısa ömürlü bağlantı noktaları sağlar. Yük Dengeleme veya gelen NAT kurallarında tek bir ön uç olduğunda, giden kuralı ön uç kavramını genişletir ve kural başına birden fazla ön uç sağlar.  Kural başına birden çok ön uç ile, kullanılabilir SNAT bağlantı noktalarının miktarı her genel IP adresi ile çarpılır ve büyük senaryolar desteklenir.

Ayrıca, [Genel BIR IP önekini](https://aka.ms/lbpublicipprefix) doğrudan giden bir kuralla kullanabilirsiniz.  Genel IP önekini kullanmak, Azure dağıtımından kaynaklanan akışların daha kolay ölçeklendirilmesi ve Basitleştirilmiş listesini sağlar. Load Balancer kaynağı içinde bir ön uç IP yapılandırmasını doğrudan genel IP adresi ön ekine başvuracak şekilde yapılandırabilirsiniz.  Bu, genel IP öneki üzerinde özel olarak denetim Load Balancer sağlar ve giden kuralı, giden bağlantılar için genel IP ön eki içinde bulunan tüm genel IP adreslerini otomatik olarak kullanır.  Genel IP öneki aralığı içindeki IP adreslerinden her biri, Load Balancer SNAT bağlantı noktası olarak kullanılacak IP adresi başına 64.000 kısa ömürlü bağlantı noktaları sağlar.   

Giden kuralı genel IP ön ekinin tamamen denetimine sahip olması gerektiği için, bu seçeneği kullandığınızda genel IP önekinden tek tek genel IP adresi kaynakları oluşturulabilir.  Daha ayrıntılı denetime ihtiyacınız varsa genel IP önekinden tek başına genel IP adresi kaynağı oluşturabilir ve bir giden kuralının ön ucunda tek tek genel IP adresleri atayabilirsiniz.

#### <a name="tune-snat-port-allocation"></a><a name="snatports"></a>SNAT bağlantı noktası ayırmasını ayarla

[Arka uç havuzu boyutuna göre OTOMATIK SNAT bağlantı noktası ayırmayı](load-balancer-outbound-connections.md#preallocatedports) ayarlamak ve otomatik SNAT bağlantı noktası ayırmanın sağladığı daha fazla veya daha az ayırmak için giden kurallarını kullanabilirsiniz.

VM başına 10.000 SNAT bağlantı noktası (NIC IP yapılandırması) ayırmak için aşağıdaki parametreyi kullanın.
 

          "allocatedOutboundPorts": 10000

Giden bir kuralın tüm ön uçlarından gelen her genel IP adresi, SNAT bağlantı noktaları olarak kullanılmak üzere 64.000 kısa ömürlü bağlantı noktasına kadar katkıda bulunur.  Load Balancer SNAT bağlantı noktalarını 8 ' in katları halinde ayırır. 8 ' den bölünemez bir değer sağlarsanız, yapılandırma işlemi reddedilir.  Genel IP adresi sayısına göre kullanılabilir olandan daha fazla SNAT bağlantı noktası ayırmaya çalışırsanız, yapılandırma işlemi reddedilir.  Örneğin, VM başına 10.000 bağlantı noktası ve arka uç havuzunda 7 VM 'Ler ayırırsanız tek bir genel IP adresi paylaşabilir, yapılandırma reddedilir (7 x 10.000 SNAT bağlantı noktaları > 64.000 SNAT bağlantı noktaları).  Senaryoyu etkinleştirmek için giden kuralının ön ucunda daha fazla genel IP adresi ekleyebilirsiniz.

Bağlantı noktası sayısı için 0 belirterek, [arka uç havuzu boyutuna bağlı olarak OTOMATIK SNAT bağlantı noktası ayırmaya](load-balancer-outbound-connections.md#preallocatedports) geri dönebilirsiniz. Bu durumda, ilk 50 sanal makine örnekleri 1024 bağlantı noktalarını alacak şekilde, 51-100 sanal makine örnekleri tabloya göre 512 ve bu şekilde devam eder.

#### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a>Denetim giden akış boşta kalma zaman aşımı

Giden kuralları, giden akış boşta kalma zaman aşımını denetlemek ve uygulamanızın gereksinimleriyle eşleştirmek için bir yapılandırma parametresi sağlar.  Çıkış boşta kalma zaman aşımları varsayılan olarak 4 dakikadır.  Parametresi, belirli bir kuralla eşleşen akışlar için boşta kalma zaman aşımı için dakika sayısı kadar olan 4 ila 120 arasında bir değer kabul eder.

Giden boşta kalma zaman aşımını 1 saate ayarlamak için aşağıdaki parametreyi kullanın:

          "idleTimeoutInMinutes": 60

#### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a><a name="tcpreset"></a>Boşta kalma zaman aşımı durumunda TCP sıfırlamayı etkinleştir

Load Balancer varsayılan davranışı, giden boşta kalma zaman aşımı ile erişildiğinde akışı sessizce düşürülemiyor.  Enabletcönayar parametresi ile, daha öngörülebilir bir uygulama davranışını etkinleştirebilir ve giden boşta kalma zaman aşımı süresi dolduğunda çift yönlü TCP sıfırlaması (TCP RST) göndermek isteyip istemediğinizi kontrol edebilirsiniz. 

Giden kuralda TCP sıfırlamayı etkinleştirmek için aşağıdaki parametreyi kullanın:

           "enableTcpReset": true

Bölge kullanılabilirliği dahil Ayrıntılar için [boşta kalma zaman aşımı durumunda TCP sıfırlamayı](https://aka.ms/lbtcpreset) gözden geçirin.

#### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a>Tek bir kuralla hem TCP hem de UDP aktarım protokollerini destekler

Büyük olasılıkla giden kuralın Aktarım Protokolü için "tümü" ni kullanmak isteyeceksiniz, ancak aynı zamanda giden kuralını belirli bir aktarım protokolüne de uygulayabilirsiniz.

Protokolü TCP ve UDP olarak ayarlamak için aşağıdaki parametreyi kullanın:

          "protocol": "All"

#### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a>Yük Dengeleme kuralı için giden NAT 'yi devre dışı bırakma

Daha önce belirtildiği gibi, Yük Dengeleme kuralları giden NAT 'nin otomatik olarak programlamayı sağlar. Ancak, bazı senaryolar, davranışı denetlemenize veya iyileştirmenize olanak tanımak için, Yük Dengeleme kuralı tarafından giden NAT otomatik programlamayı devre dışı bırakmanızı veya zorunlu kılmanızı gerektirir.  Giden kurallarında, otomatik giden NAT programlamayı durdurmak için önemli olan senaryolar bulunur.

Bu parametreyi iki şekilde kullanabilirsiniz:
- Giden NAT için gelen IP adresinin kullanılması için isteğe bağlı gizleme.  Giden kuralları Yük Dengeleme kurallarına ve bu parametre kümesiyle artımlı olarak, giden kuralı denetimde olur.
  
- Gelen ve giden için aynı anda kullanılan bir IP adresinin giden NAT parametrelerini ayarlayın.  Giden bir kuralın denetim geçirmesine izin vermek için otomatik giden NAT programlaması devre dışı bırakılmalıdır.  Örneğin, gelen için de kullanılan bir adresin SNAT bağlantı noktası ayırmasını değiştirmek için bu parametrenin true olarak ayarlanması gerekir.  Gelen bir IP adresinin parametrelerini yeniden tanımlamak için bir giden kuralı kullanmayı denerseniz ve yük dengeleme kuralının giden NAT programlamayı yayımlamadıysanız, bir giden kuralı yapılandırma işlemi başarısız olur.

>[!IMPORTANT]
> Bu parametreyi true olarak ayarlarsanız ve giden bağlantıyı tanımlamak için bir giden kuralı (veya [örnek düzeyi genel IP senaryosu](load-balancer-outbound-connections.md#ilpip) ) yoksa, sanal makineniz giden bağlantıya sahip olmayacaktır.  VM 'nizin veya uygulamanızın bazı işlemleri giden bağlantıya sahip olmaya bağlı olabilir. Senaryonuzun bağımlılıklarını anladığınızdan ve bu değişikliği yapmanın etkileri olduğuna emin olun.

Yük Dengeleme kuralında bu yapılandırma parametresiyle giden SNAT 'yi devre dışı bırakabilirsiniz:

```json
      "loadBalancingRules": [
        {
          "disableOutboundSnat": true
        }
      ]
```

DisableOutboundSNAT parametresi false olarak ayarlanır; Bu, Yük Dengeleme kuralının Yük Dengeleme **kuralı yapılandırmasının** bir yansıtma görüntüsü olarak OTOMATIK giden NAT sağladığı anlamına gelir.  

Yük Dengeleme kuralında disableOutboundSnat değerini true olarak ayarlarsanız, Yük Dengeleme kuralı, aksi halde otomatik giden NAT programlamanın denetimini yayınlar.  Yük Dengeleme kuralının bir sonucu olarak giden SNAT devre dışı bırakıldı.

#### <a name="reuse-existing-or-define-new-backend-pools"></a>Varolanı yeniden kullan veya yeni arka uç havuzlarını tanımla

Giden kuralları, kuralın uygulanacağı VM grubunu tanımlamaya yönelik yeni bir kavram sunmaz.  Bunun yerine, Yük Dengeleme kuralları için de kullanılan bir arka uç havuzu kavramını kullanır. Bunu, var olan bir arka uç havuzu tanımını yeniden kullanıp veya bir giden kuralı için özel olarak oluşturarak yapılandırmayı basitleştirmek için kullanabilirsiniz.

### <a name="scenarios"></a>Senaryolar

#### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a>Belirli bir genel IP adresi kümesine giden bağlantılar

Giden bağlantıların, belirli bir genel IP adresi kümesinden kaynaklanacak şekilde görüntülenmesini sağlamak için bir giden kuralı kullanabilirsiniz.  Bu kaynak genel IP adresi bir yük dengeleme kuralı veya bir yük dengeleme kuralı tarafından kullanılandan farklı bir genel IP adresi kümesi tarafından kullanılan ile aynı olabilir.  

1. [Genel IP öneki](https://aka.ms/lbpublicipprefix) (veya genel IP önekinden genel IP adresleri) oluştur
2. Genel bir Standart Yük Dengeleyici oluşturma
3. Kullanmak istediğiniz genel IP önekine (veya genel IP adreslerine) başvuran ön uçlar oluşturun
4. Arka uç havuzunu yeniden kullanma veya arka uç havuzu oluşturma ve VM 'Leri genel Load Balancer arka uç havuzuna yerleştirme
5. Ön uçların kullanıldığı bu VM 'Ler için genel Load Balancer giden NAT programlamak için bir giden kuralı yapılandırın
   
Yük Dengeleme kuralının giden için kullanılmasını istemiyorsanız, Yük Dengeleme kuralında [gıden SNAT 'yi devre dışı bırakmanız](#disablesnat) gerekir.

#### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a>SNAT bağlantı noktası ayırmayı Değiştir

[Arka uç havuzu boyutuna göre OTOMATIK SNAT bağlantı noktası ayırmayı](load-balancer-outbound-connections.md#preallocatedports)ayarlamak için giden kurallarını kullanabilirsiniz.

Örneğin, giden NAT için tek bir genel IP adresi paylaşan iki sanal makineniz varsa, varsayılan 1024 bağlantı noktasından ayrılan SNAT bağlantı noktası sayısını, SNAT tükenmesi ile karşılaşıyorsanız yükseltmek isteyebilirsiniz. Her genel IP adresi en fazla 64.000 kısa ömürlü bağlantı noktasına katkıda bulunabilir.  Tek bir genel IP adresi ön ucuna sahip bir giden kuralı yapılandırırsanız, arka uç havuzundaki sanal makinelere Toplam 64.000 SNAT bağlantı noktası dağıtabilirsiniz.  İki VM için, en fazla 32.000 SNAT bağlantı noktası bir giden kuralla ayrılabilir (2x 32.000 = 64.000).

[Giden bağlantıları](load-balancer-outbound-connections.md) ve [SNAT](load-balancer-outbound-connections.md#snat) bağlantı noktalarının nasıl ayrıldığı ve kullanıldığı hakkındaki ayrıntıları gözden geçirin.

#### <a name="enable-outbound-only"></a><a name="outboundonly"></a>Yalnızca giden trafiği etkinleştir

Bir VM grubu için giden NAT sağlamak üzere ortak bir Standart Load Balancer kullanabilirsiniz. Bu senaryoda, herhangi bir ek kurala gerek duymadan bir giden kuralını kendisi kullanabilirsiniz.

##### <a name="outbound-nat-for-vms-only-no-inbound"></a>Yalnızca VM 'Ler için giden NAT (gelen)

Ortak bir Standart Load Balancer tanımlayın, sanal makineleri arka uç havuzuna yerleştirin ve giden NAT 'yi programlamak ve giden bağlantıları belirli bir genel IP adresinden kaynaklanacak şekilde yapılandırın. Ayrıca, giden bağlantıların kaynağını bir arada listelemek için genel bir IP öneki de kullanabilirsiniz.

1. Ortak Standart Load Balancer oluşturun.
2. Bir arka uç havuzu oluşturun ve VM 'Leri genel Load Balancer arka uç havuzuna yerleştirin.
3. Bu VM 'Ler için giden NAT programlamak için genel Load Balancer giden bir kural yapılandırın.

##### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>İç Standart Load Balancer senaryolar için giden NAT

Dahili bir Standart Load Balancer kullanılırken, giden bağlantı açıkça bildirildiği sürece giden NAT kullanılamaz. Bir iç Standart Load Balancer arkasındaki VM 'Ler için giden bağlantıyı aşağıdaki adımlarla oluşturmak üzere giden bağlantı kuralı kullanarak giden bağlantıyı tanımlayabilirsiniz:

1. Ortak Standart Load Balancer oluşturun.
2. Bir arka uç havuzu oluşturun ve VM 'Leri iç Load Balancer ek olarak ortak Load Balancer arka uç havuzuna yerleştirin.
3. Bu VM 'Ler için giden NAT programlamak için genel Load Balancer giden bir kural yapılandırın.

##### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Giden NAT için hem TCP & UDP protokollerini ortak bir Standart Load Balancer etkinleştirin

- Ortak bir Standart Load Balancer kullanıldığında, belirtilen otomatik giden NAT programlama, Yük Dengeleme kuralının Aktarım Protokolü ile eşleşir.  

   1. Yük Dengeleme kuralında giden SNAT 'yi devre dışı bırakın.
   2. Aynı Load Balancer bir giden kuralı yapılandırın.
   3. VM 'niz tarafından zaten kullanılan arka uç havuzunu yeniden kullanın.
   4. Giden kuralının bir parçası olarak "protokol": "tümü" ni belirtin.

- Yalnızca gelen NAT kuralları kullanıldığında, giden NAT sağlanmaz.

   1. VM 'Leri bir arka uç havuzuna yerleştirin.
   2. Genel IP adresleri veya genel IP önekiyle bir veya daha fazla ön uç IP yapılandırması tanımlayın.
   3. Aynı Load Balancer bir giden kuralı yapılandırın.
   4. Giden kuralının bir parçası olarak "Protocol": "All" belirtin


## <a name="limitations"></a>Sınırlamalar
- Ön uç IP adresi başına kullanılabilir en fazla kısa ömürlü bağlantı noktası sayısı 64.000 ' dir.
- Yapılandırılabilir giden boşta kalma zaman aşımı aralığı 4 ila 120 dakikadır (240 ila 7200 saniye).
- Load Balancer giden NAT için ıCMP 'yi desteklemez.
- Giden kuralları, yalnızca bir NIC 'in birincil IP yapılandırmasına uygulanabilir.  Birden çok NIC desteklenir.
- VNet ve diğer Microsoft Platformu Hizmetleri olmayan Web çalışanı rolleri, ön VNet Hizmetleri ve diğer platform hizmetleri işlevinin yan etkisi nedeniyle yalnızca bir iç Standart Load Balancer kullanıldığında erişilebilir. İlgili hizmetin kendisi veya temel alınan platform hiçbir bildirimde bulunmaksızın değişmeksizin, bu yan etkiye güvenmeyin. Yalnızca dahili Standart Load Balancer kullandığınızda istenirse, açıkça giden bağlantı oluşturmanız gerektiğini varsaymanız gerekir. Bu makalede açıklanan [varsayılan SNAT](#defaultsnat) senaryosu 3 kullanılabilir değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- Standart genel Load Balancer [giden kuralları](load-balancer-outbound-rules-overview.md) hakkında daha fazla bilgi edinin.
- [Load Balancer](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
- Azure 'daki diğer bazı anahtar [ağ özellikleri](../networking/networking-overview.md) hakkında bilgi edinin.
