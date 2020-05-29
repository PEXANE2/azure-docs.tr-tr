---
title: Giden kuralları-Azure Load Balancer
description: Bu öğrenme yoluyla giden ağ adresi çevirileri tanımlamak için giden kurallarını kullanmaya başlayın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 7/17/2019
ms.author: allensu
ms.openlocfilehash: e6c7464eb1bf51a4e42d0db98d92459dc39fbb11
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170826"
---
# <a name="load-balancer-outbound-rules"></a>Giden kuralları Load Balancer

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

## <a name="outbound-rule"></a>Giden kuralı

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

### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a>Birden çok IP adresiyle giden NAT 'yi ölçeklendirme

Giden kuralı yalnızca tek bir genel IP adresiyle kullanılabilir ancak giden kuralları, giden NAT ölçeklendirme için yapılandırma yükünü kolaylaştırır. Büyük ölçekli senaryoları planlamak için birden çok IP adresi kullanabilir ve giden kurallarını, [SNAT tükenmesi](troubleshoot-outbound-connection.md#snatexhaust) açısından yüksek desenleri azaltmak için kullanabilirsiniz.  

Ön uç tarafından sağlanan her ek IP adresi, Load Balancer SNAT bağlantı noktası olarak kullanılacak olan 64.000 kısa ömürlü bağlantı noktaları sağlar. Yük Dengeleme veya gelen NAT kurallarında tek bir ön uç olduğunda, giden kuralı ön uç kavramını genişletir ve kural başına birden fazla ön uç sağlar.  Kural başına birden çok ön uç ile, kullanılabilir SNAT bağlantı noktalarının miktarı her genel IP adresi ile çarpılır ve büyük senaryolar desteklenir.

Ayrıca, [Genel BIR IP önekini](https://aka.ms/lbpublicipprefix) doğrudan giden bir kuralla kullanabilirsiniz.  Genel IP önekini kullanmak, Azure dağıtımından kaynaklanan akışların daha kolay ölçeklendirilmesi ve Basitleştirilmiş listesini sağlar. Load Balancer kaynağı içinde bir ön uç IP yapılandırmasını doğrudan genel IP adresi ön ekine başvuracak şekilde yapılandırabilirsiniz.  Bu, genel IP öneki üzerinde özel olarak denetim Load Balancer sağlar ve giden kuralı, giden bağlantılar için genel IP ön eki içinde bulunan tüm genel IP adreslerini otomatik olarak kullanır.  Genel IP öneki aralığı içindeki IP adreslerinden her biri, Load Balancer SNAT bağlantı noktası olarak kullanılacak IP adresi başına 64.000 kısa ömürlü bağlantı noktaları sağlar.   

Giden kuralı genel IP ön ekinin tamamen denetimine sahip olması gerektiği için, bu seçeneği kullandığınızda genel IP önekinden tek tek genel IP adresi kaynakları oluşturulabilir.  Daha ayrıntılı denetime ihtiyacınız varsa genel IP önekinden tek başına genel IP adresi kaynağı oluşturabilir ve bir giden kuralının ön ucunda tek tek genel IP adresleri atayabilirsiniz.

### <a name="tune-snat-port-allocation"></a><a name="snatports"></a>SNAT bağlantı noktası ayırmasını ayarla

[Arka uç havuzu boyutuna göre OTOMATIK SNAT bağlantı noktası ayırmayı](load-balancer-outbound-connections.md#preallocatedports) ayarlamak ve otomatik SNAT bağlantı noktası ayırmanın sağladığı daha fazla veya daha az ayırmak için giden kurallarını kullanabilirsiniz.

VM başına 10.000 SNAT bağlantı noktası (NIC IP yapılandırması) ayırmak için aşağıdaki parametreyi kullanın.
 

          "allocatedOutboundPorts": 10000

Giden bir kuralın tüm ön uçlarından gelen her genel IP adresi, SNAT bağlantı noktaları olarak kullanılmak üzere 64.000 kısa ömürlü bağlantı noktasına kadar katkıda bulunur.  Load Balancer SNAT bağlantı noktalarını 8 ' in katları halinde ayırır. 8 ' den bölünemez bir değer sağlarsanız, yapılandırma işlemi reddedilir.  Genel IP adresi sayısına göre kullanılabilir olandan daha fazla SNAT bağlantı noktası ayırmaya çalışırsanız, yapılandırma işlemi reddedilir.  Örneğin, VM başına 10.000 bağlantı noktası ve arka uç havuzunda 7 VM 'Ler ayırırsanız tek bir genel IP adresi paylaşabilir, yapılandırma reddedilir (7 x 10.000 SNAT bağlantı noktaları > 64.000 SNAT bağlantı noktaları).  Senaryoyu etkinleştirmek için giden kuralının ön ucunda daha fazla genel IP adresi ekleyebilirsiniz.

Bağlantı noktası sayısı için 0 belirterek, [arka uç havuzu boyutuna bağlı olarak OTOMATIK SNAT bağlantı noktası ayırmaya](load-balancer-outbound-connections.md#preallocatedports) geri dönebilirsiniz. Bu durumda, ilk 50 sanal makine örnekleri 1024 bağlantı noktalarını alacak şekilde, 51-100 sanal makine örnekleri tabloya göre 512 ve bu şekilde devam eder.

### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a>Denetim giden akış boşta kalma zaman aşımı

Giden kuralları, giden akış boşta kalma zaman aşımını denetlemek ve uygulamanızın gereksinimleriyle eşleştirmek için bir yapılandırma parametresi sağlar.  Çıkış boşta kalma zaman aşımları varsayılan olarak 4 dakikadır.  Parametresi, belirli bir kuralla eşleşen akışlar için boşta kalma zaman aşımı için dakika sayısı kadar olan 4 ila 120 arasında bir değer kabul eder.

Giden boşta kalma zaman aşımını 1 saate ayarlamak için aşağıdaki parametreyi kullanın:

          "idleTimeoutInMinutes": 60

### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a><a name="tcpreset"></a>Boşta kalma zaman aşımı durumunda TCP sıfırlamayı etkinleştir

Load Balancer varsayılan davranışı, giden boşta kalma zaman aşımı ile erişildiğinde akışı sessizce düşürülemiyor.  Enabletcönayar parametresi ile, daha öngörülebilir bir uygulama davranışını etkinleştirebilir ve giden boşta kalma zaman aşımı süresi dolduğunda çift yönlü TCP sıfırlaması (TCP RST) göndermek isteyip istemediğinizi kontrol edebilirsiniz. 

Giden kuralda TCP sıfırlamayı etkinleştirmek için aşağıdaki parametreyi kullanın:

           "enableTcpReset": true

Bölge kullanılabilirliği dahil Ayrıntılar için [boşta kalma zaman aşımı durumunda TCP sıfırlamayı](https://aka.ms/lbtcpreset) gözden geçirin.

### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a>Tek bir kuralla hem TCP hem de UDP aktarım protokollerini destekler

Büyük olasılıkla giden kuralın Aktarım Protokolü için "tümü" ni kullanmak isteyeceksiniz, ancak aynı zamanda giden kuralını belirli bir aktarım protokolüne de uygulayabilirsiniz.

Protokolü TCP ve UDP olarak ayarlamak için aşağıdaki parametreyi kullanın:

          "protocol": "All"

### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a>Yük Dengeleme kuralı için giden NAT 'yi devre dışı bırakma

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

### <a name="reuse-existing-or-define-new-backend-pools"></a>Varolanı yeniden kullan veya yeni arka uç havuzlarını tanımla

Giden kuralları, kuralın uygulanacağı VM grubunu tanımlamaya yönelik yeni bir kavram sunmaz.  Bunun yerine, Yük Dengeleme kuralları için de kullanılan bir arka uç havuzu kavramını kullanır. Bunu, var olan bir arka uç havuzu tanımını yeniden kullanıp veya bir giden kuralı için özel olarak oluşturarak yapılandırmayı basitleştirmek için kullanabilirsiniz.

## <a name="scenarios"></a>Senaryolar

### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a>Belirli bir genel IP adresi kümesine giden bağlantılar

Giden bağlantıların, belirli bir genel IP adresi kümesinden kaynaklanacak şekilde görüntülenmesini sağlamak için bir giden kuralı kullanabilirsiniz.  Bu kaynak genel IP adresi bir yük dengeleme kuralı veya bir yük dengeleme kuralı tarafından kullanılandan farklı bir genel IP adresi kümesi tarafından kullanılan ile aynı olabilir.  

1. [Genel IP öneki](https://aka.ms/lbpublicipprefix) (veya genel IP önekinden genel IP adresleri) oluştur
2. Genel bir Standart Yük Dengeleyici oluşturma
3. Kullanmak istediğiniz genel IP önekine (veya genel IP adreslerine) başvuran ön uçlar oluşturun
4. Arka uç havuzunu yeniden kullanma veya arka uç havuzu oluşturma ve VM 'Leri genel Load Balancer arka uç havuzuna yerleştirme
5. Ön uçların kullanıldığı bu VM 'Ler için genel Load Balancer giden NAT programlamak için bir giden kuralı yapılandırın
   
Yük Dengeleme kuralının giden için kullanılmasını istemiyorsanız, Yük Dengeleme kuralında [gıden SNAT 'yi devre dışı bırakmanız](#disablesnat) gerekir.

### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a>SNAT bağlantı noktası ayırmayı Değiştir

[Arka uç havuzu boyutuna göre OTOMATIK SNAT bağlantı noktası ayırmayı](load-balancer-outbound-connections.md#preallocatedports)ayarlamak için giden kurallarını kullanabilirsiniz.

Örneğin, giden NAT için tek bir genel IP adresi paylaşan iki sanal makineniz varsa, varsayılan 1024 bağlantı noktasından ayrılan SNAT bağlantı noktası sayısını, SNAT tükenmesi ile karşılaşıyorsanız yükseltmek isteyebilirsiniz. Her genel IP adresi en fazla 64.000 kısa ömürlü bağlantı noktasına katkıda bulunabilir.  Tek bir genel IP adresi ön ucuna sahip bir giden kuralı yapılandırırsanız, arka uç havuzundaki sanal makinelere Toplam 64.000 SNAT bağlantı noktası dağıtabilirsiniz.  İki VM için, en fazla 32.000 SNAT bağlantı noktası bir giden kuralla ayrılabilir (2x 32.000 = 64.000).

[Giden bağlantıları](load-balancer-outbound-connections.md) ve [SNAT](load-balancer-outbound-connections.md#snat) bağlantı noktalarının nasıl ayrıldığı ve kullanıldığı hakkındaki ayrıntıları gözden geçirin.

### <a name="enable-outbound-only"></a><a name="outboundonly"></a>Yalnızca giden trafiği etkinleştir

Bir VM grubu için giden NAT sağlamak üzere ortak bir Standart Load Balancer kullanabilirsiniz. Bu senaryoda, herhangi bir ek kurala gerek duymadan bir giden kuralını kendisi kullanabilirsiniz.

#### <a name="outbound-nat-for-vms-only-no-inbound"></a>Yalnızca VM 'Ler için giden NAT (gelen)

Ortak bir Standart Load Balancer tanımlayın, sanal makineleri arka uç havuzuna yerleştirin ve giden NAT 'yi programlamak ve giden bağlantıları belirli bir genel IP adresinden kaynaklanacak şekilde yapılandırın. Ayrıca, giden bağlantıların kaynağını bir arada listelemek için genel bir IP öneki de kullanabilirsiniz.

1. Ortak Standart Load Balancer oluşturun.
2. Bir arka uç havuzu oluşturun ve VM 'Leri genel Load Balancer arka uç havuzuna yerleştirin.
3. Bu VM 'Ler için giden NAT programlamak için genel Load Balancer giden bir kural yapılandırın.

#### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>İç Standart Load Balancer senaryolar için giden NAT

Dahili bir Standart Load Balancer kullanılırken, giden bağlantı açıkça bildirildiği sürece giden NAT kullanılamaz. Bir iç Standart Load Balancer arkasındaki VM 'Ler için giden bağlantıyı aşağıdaki adımlarla oluşturmak üzere giden bağlantı kuralı kullanarak giden bağlantıyı tanımlayabilirsiniz:

1. Ortak Standart Load Balancer oluşturun.
2. Bir arka uç havuzu oluşturun ve VM 'Leri iç Load Balancer ek olarak ortak Load Balancer arka uç havuzuna yerleştirin.
3. Bu VM 'Ler için giden NAT programlamak için genel Load Balancer giden bir kural yapılandırın.

#### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Giden NAT için hem TCP & UDP protokollerini ortak bir Standart Load Balancer etkinleştirin

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

## <a name="next-steps"></a>Sonraki adımlar

- [Giden bağlantılar için Load Balancer](load-balancer-outbound-connections.md)kullanma hakkında bilgi edinin.
- [Standart Load Balancer](load-balancer-standard-overview.md)hakkında bilgi edinin.
- [Boşta kalma zaman aşımı durumunda çift yönlü TCP sıfırlaması](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Azure clı 2,0 ile giden kurallarını yapılandırın](configure-load-balancer-outbound-cli.md).
