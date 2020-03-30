---
title: Giden kurallar - Azure Yük Bakiyesi
description: Bu öğrenme yolu ile, giden ağ adresi çevirilerini tanımlamak için giden kuralları kullanmaya başlayın.
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
ms.openlocfilehash: d419c213b3bcfef3631d68eb9d4cb485291bed31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304200"
---
# <a name="load-balancer-outbound-rules"></a>Yük Dengeleyici giden kuralları

Azure Yük Dengeleyici, gelene ek olarak sanal ağdan giden bağlantı sağlar.  Giden kurallar, ortak [Standart Yük Dengeleyicisi'nin](load-balancer-standard-overview.md)giden ağ adresi çevirisini yapılandırmayı kolaylaştırır.  Bu yeteneği özel gereksinimlerinize göre ölçeklendirmek ve ayarlamak için giden bağlantı üzerinde tam bildirimsel denetime sahipsiniz.

![Yük Dengeleyici giden kuralları](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Giden kurallarla, Load Balancer'ı şu şekilde kullanabilirsiniz: 
- giden NAT'yi sıfırdan tanımlayın.
- ölçeklendirin ve varolan giden NAT'nin davranışını ayarlayın. 

Giden kurallar şunları kontrol etmenizi sağlar:
- hangi sanal makinelerin hangi genel IP adreslerine çevrilmesi gerekir. 
- [giden SNAT bağlantı noktalarının](load-balancer-outbound-connections.md#snat) nasıl ayrılması gerektiği.
- hangi protokoller için giden çeviri sağlamak.
- giden bağlantı boşta zaman ayarı (4-120 dakika) için ne süre kullanılır.
- boşta zaman anına bir TCP Sıfırlama göndermek için olup olmadığını

Giden [kurallar, giden bağlantılar](load-balancer-outbound-connections.md) makalesinde açıklanan [senaryo 2'yi](load-balancer-outbound-connections.md#lb) genişletir ve senaryo önceliği olduğu gibi kalır.

## <a name="outbound-rule"></a>Giden kuralı

Tüm Yük Dengeleyici kuralları gibi, giden kurallar da yük dengeleme ve gelen NAT kurallarıyla aynı tanıdık sözdizimini izler:

**frontend** + **parametreleri** + **arka uç havuzu**

Giden kural, arka uç havuzu _tarafından tanımlanan tüm sanal makineler_ için giden NAT'yi ön _uça_çevrilecek şekilde yapılandırır.  Ve _parametreler_ giden NAT algoritması üzerinde ek ince taneli kontrol sağlar.

API sürümü "2018-07-01" aşağıdaki gibi yapılandırılmış bir giden kural tanımı izin verir:

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
>Etkili giden NAT yapılandırması, tüm giden kuralların ve yük dengeleme kurallarının bir bileşimidir. Giden kurallar dengeleme kurallarını yüklemek için artımlıdır. VM için birden çok kural uygulandığında etkili giden NAT çevirisini yönetmek [için bir yük dengeleme kuralı için giden NAT'nin devre dışı bırakılmasını](#disablesnat) gözden geçirin. Yük dengeleme kuralı yla aynı genel IP adresini kullanan giden bir kural tanımlarken [giden SNAT'yi devre dışı](#disablesnat) kolmalısınız.

### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a>Giden NAT'yi birden çok IP adresiyle ölçeklendirin

Giden kural tek bir genel IP adresiyle kullanılabilirken, giden kurallar giden NAT'yi ölçekleme yapılandırma yükünü hafifletir. Büyük ölçekli senaryolar için plan yapmak için birden çok IP adresi kullanabilir ve [SNAT tükenmeye](load-balancer-outbound-connections.md#snatexhaust) eğilimli desenleri azaltmak için giden kuralları kullanabilirsiniz.  

Bir ön uç tarafından sağlanan her ek IP adresi, Yük Dengeleyicisi'nin SNAT bağlantı noktası olarak kullanması için 64.000 geçici bağlantı noktası sağlar. Yük dengeleme veya gelen NAT kuralları nın tek bir ön ucu olsa da, giden kural ön uç kavramını genişletir ve kural başına birden çok frontends sağlar.  Kural başına birden çok ön uçla, kullanılabilir SNAT bağlantı noktalarının miktarı her ortak IP adresiyle çarpılır ve büyük senaryolar desteklenebilir.

Ayrıca, doğrudan giden bir kuralla [ortak bir IP öneki](https://aka.ms/lbpublicipprefix) kullanabilirsiniz.  Genel IP önekinin kullanılması, Azure dağıtımınızdan kaynaklanan akışların daha kolay ölçeklendirilmesini ve basitleştirilmiş beyaz listesini sağlar. Load Balancer kaynağı içinde bir ön uç IP yapılandırmasını, genel bir IP adresi önekine doğrudan başvurmak için yapılandırabilirsiniz.  Bu, Load Balancer'ın genel IP öneki üzerinde özel denetimini sağlar ve giden kural giden bağlantılar için genel IP öneki içinde bulunan tüm genel IP adreslerini otomatik olarak kullanır.  Ortak IP öneki aralığındaki IP adreslerinin her biri, Yük Dengeleyicisi'nin SNAT bağlantı noktası olarak kullanması için IP adresi başına 64.000 geçici bağlantı noktası sağlar.   

Giden kural ortak IP önekitam denetimine sahip olmalıdır olarak bu seçeneği kullanırken ortak IP öneki oluşturulan tek tek genel IP adresi kaynakları olamaz.  Daha ince taneli denetime ihtiyacınız varsa, ortak IP önekinden tek tek genel IP adresi kaynağı oluşturabilir ve giden bir kuralın ön ucuna tek tek birden fazla genel IP adresi atayabilirsiniz.

### <a name="tune-snat-port-allocation"></a><a name="snatports"></a>SNAT bağlantı noktası tahsisini ayarlayın

Otomatik [SNAT bağlantı noktası tahsisini arka uç havuz boyutuna göre](load-balancer-outbound-connections.md#preallocatedports) ayarlamak ve otomatik SNAT bağlantı noktası ayırmanın sağladığından daha fazla veya daha az ayırmak için giden kuralları kullanabilirsiniz.

VM başına 10.000 SNAT bağlantı noktası (NIC IP yapılandırması) ayırmak için aşağıdaki parametreyi kullanın.
 

          "allocatedOutboundPorts": 10000

Giden kuralın tüm ön uçlarından gelen her genel IP adresi, SNAT bağlantı noktası olarak kullanılmak üzere 64.000'e kadar geçici bağlantı noktası katkıda bulunur.  Yük DengeleyiciSNAT bağlantı noktalarını 8'in katları olarak ayırır. 8 ile bölünemez bir değer sağlarsanız, yapılandırma işlemi reddedilir.  Ortak IP adreslerinin sayısına bağlı olarak kullanılabilir olandan daha fazla SNAT bağlantı noktası ayırmaya çalışırsanız, yapılandırma işlemi reddedilir.  Örneğin, VM başına 10.000 bağlantı noktası ayırırsanız ve arka uç havuzunda 7 VM tek bir genel IP adresini paylaşırsanız, yapılandırma reddedilir (7 x 10.000 SNAT bağlantı noktası > 64.000 SNAT bağlantı noktası).  Senaryoyu etkinleştirmek için giden kuralın ön ucuna daha fazla genel IP adresi ekleyebilirsiniz.

Bağlantı noktası sayısı için 0 belirterek [arka uç havuzu boyutuna göre otomatik SNAT bağlantı noktası ayırmasına](load-balancer-outbound-connections.md#preallocatedports) geri dönebilirsiniz. Bu durumda ilk 50 VM örnekleri 1024 bağlantı noktası alacak, 51-100 VM örnekleri tabloya göre 512 ve benzeri alacak.

### <a name="control-outbound-flow-idle-timeout"></a><a name="idletimeout"></a>Giden akışı boşta denetleme zaman

Giden kurallar, giden akış boşta zaman anına denetim etmek ve uygulamanızın gereksinimleriyle eşleştirmek için bir yapılandırma parametresi sağlar.  Giden boşta zaman ları varsayılan olarak 4 dakikaya kadar.  Parametre, bu özel kuralla eşleşen akışlar için boşta kalan zaman anındakika sayısına kadar 4 ile 120 arasında bir değer kabul eder.

Giden boşta zaman dilimini 1 saatolarak ayarlamak için aşağıdaki parametreyi kullanın:

          "idleTimeoutInMinutes": 60

### <a name="enable-tcp-reset-on-idle-timeout"></a><a name="tcprst"></a><a name="tcpreset"></a> Boşta zaman anında TCP Sıfırlama'yı etkinleştirme

Yük Dengeleyici'nin varsayılan davranışı, giden boşta zaman adedine ulaşıldığında akışı sessizce bırakmaktır.  EnableTCPReset parametresi ile, daha öngörülebilir bir uygulama davranışı etkinleştirebilir ve giden boşta zaman aşımı sırasında çift yönlü TCP Sıfırlama (TCP RST) gönderip göndermemenizi sağlayabilirsiniz. 

Giden bir kuralda TCP Sıfırlama'yı etkinleştirmek için aşağıdaki parametreyi kullanın:

           "enableTcpReset": true

Bölge kullanılabilirliği de dahil olmak üzere ayrıntılar için [boşta zaman anına göre TCP Sıfırlama'yı](https://aka.ms/lbtcpreset) gözden geçirin.

### <a name="support-both-tcp-and-udp-transport-protocols-with-a-single-rule"></a><a name="proto"></a>Hem TCP hem de UDP aktarım protokollerini tek bir kuralla destekleme

Büyük olasılıkla giden kuralın aktarım protokolü için "Tümü"nü kullanmak isteyebilirsiniz, ancak bunu yapmak için bir ihtiyaç varsa, giden kuralı belirli bir aktarım protokolüne de uygulayabilirsiniz.

Protokolü TCP ve UDP olarak ayarlamak için aşağıdaki parametreyi kullanın:

          "protocol": "All"

### <a name="disable-outbound-nat-for-a-load-balancing-rule"></a><a name="disablesnat"></a>Yük dengeleme kuralı için giden NAT'yi devre dışı

Daha önce belirtildiği gibi, yük dengeleme kuralları giden NAT otomatik programlama sağlar. Ancak, bazı senaryolar, davranışı denetlemenize veya hassaslaştırmanıza olanak sağlamak için giden NAT'nin otomatik programlamasını yük dengeleme kuralıyla devre dışı kaldırmanızı gerektirir veya yararlanır.  Giden kurallar, otomatik giden NAT programlamayı durdurmanın önemli olduğu senaryolara sahiptir.

Bu parametreyi iki şekilde kullanabilirsiniz:
- Giden NAT için gelen IP adresini kullanmanın isteğe bağlı olarak bastırılması.  Giden kurallar dengeleme kurallarını yüklemek için artımlı ve bu parametre kümesi ile, giden kural denetim altındadır.
  
- Gelen ve giden için aynı anda kullanılan bir IP adresinin giden NAT parametrelerini ayarlayın.  Giden bir kuralın denetimi ele geçirmesine izin vermek için otomatik giden NAT programlamadevre dışı bırakılmalıdır.  Örneğin, gelen için de kullanılan bir adresin SNAT bağlantı noktası tahsisini değiştirmek için bu parametrenin doğru olarak ayarlanabilmesi gerekir.  Gelen için de kullanılan bir IP adresinin parametrelerini yeniden tanımlamak için giden bir kural kullanmaya çalışırsanız ve yük dengeleme kuralının giden NAT programlamasını yayımlamazsanız, giden kuralı yapılandırma işlemi başarısız olur.

>[!IMPORTANT]
> Bu parametreyi doğru ayarlarsanız ve giden bir kuralınız (veya giden bağlantıyı tanımlamak için [örnek düzeyinde genel IP senaryosu)](load-balancer-outbound-connections.md#ilpip) yoksa, sanal makinenizin giden bağlantısı olmaz.  VM'nizin veya uygulamanızın bazı işlemleri, giden bağlantının kullanılabilir olmasına bağlı olabilir. Senaryonuzun bağımlılıklarını anladığınızdan ve bu değişikliği yapmanın etkisini göz önünde bulundurup düşünün.

Bu yapılandırma parametresi ile yük dengeleme kuralında giden SNAT'yi devre dışı bırakabilirsiniz:

```json
      "loadBalancingRules": [
        {
          "disableOutboundSnat": true
        }
      ]
```

Devre dışı bırakılanSNAT parametresi varsayılan olarak false'a gelir, bu da yük dengeleme **kuralının** yük dengeleme kuralı yapılandırmasının ayna görüntüsü olarak otomatik giden NAT'yi sağladığı anlamına gelir.  

Devre dışı bırakmaOutboundSnat'ı yük dengeleme kuralında doğru şekilde ayarlarsanız, yük dengeleme kuralı aksi takdirde otomatik giden NAT programlamanın denetimini serbest bırakır.  Yük dengeleme kuralı sonucunda giden SNAT devre dışı bırakılır.

### <a name="reuse-existing-or-define-new-backend-pools"></a>Varolan yeniden kullanma veya yeni arka uç havuzları tanımlama

Giden kurallar, kuralın uygulanması gereken VM grubunu tanımlamak için yeni bir kavram getirmez.  Bunun yerine, yük dengeleme kuralları için de kullanılan bir arka uç havuzu kavramını yeniden kullanırlar. Bunu, varolan bir arka uç havuzu tanımını yeniden kullanarak veya özellikle giden bir kural için bir tane oluşturarak yapılandırmayı basitleştirmek için kullanabilirsiniz.

## <a name="scenarios"></a>Senaryolar

### <a name="groom-outbound-connections-to-a-specific-set-of-public-ip-addresses"></a><a name="groom"></a>Belirli bir genel IP adresi kümesine giden bağlantılar

Beyaz liste senaryolarını kolaylaştırmak için, giden bağlantıları belirli bir genel IP adresi kümesinden kaynaklanıyor muş gibi görünmesi için giden bir kural kullanabilirsiniz.  Bu kaynak genel IP adresi, yük dengeleme kuralı veya bir yük dengeleme kuralı tarafından kullanılandan farklı bir genel IP adresi kümesi tarafından kullanılanla aynı olabilir.  

1. [Ortak IP öneki](https://aka.ms/lbpublicipprefix) (veya ortak IP önekinden genel IP adresleri) oluşturun
2. Genel bir Standart Yük Dengeleyici oluşturma
3. Kullanmak istediğiniz genel IP önekine (veya genel IP adreslerine) atıfta bulunan ön uçlar oluşturma
4. Bir arka uç havuzunu yeniden kullanın veya bir arka uç havuzu oluşturun ve VM'leri ortak Yük Dengeleyicisinin arka uç havuzuna yerleştirin
5. Ön uçları kullanarak bu VM'ler için giden NAT'yi programlamak için ortak Yük Dengeleyicisi'nde giden kuralı yapılandırın
   
Giden için yük dengeleme kuralının kullanılmasını istemiyorsanız, [giden SNAT'ı](#disablesnat) yük dengeleme kuralında devre dışı bilmelidir.

### <a name="modify-snat-port-allocation"></a><a name="modifysnat"></a>SNAT bağlantı noktası tahsisini değiştirme

Otomatik [SNAT bağlantı noktası tahsisini arka uç havuz boyutuna göre](load-balancer-outbound-connections.md#preallocatedports)ayarlamak için giden kuralları kullanabilirsiniz.

Örneğin, giden NAT için tek bir genel IP adresini paylaşan iki sanal makineniz varsa, SNAT tükenmesi yaşıyorsanız varsayılan 1024 bağlantı noktalarından ayrılan SNAT bağlantı noktası sayısını artırmak isteyebilirsiniz. Her genel IP adresi 64.000'e kadar geçici bağlantı noktası katkıda bulunabilir.  Giden bir kuralı tek bir genel IP adresi ön uçla yapılandırarak, arka uç havuzundaki VM'lere toplam 64.000 SNAT bağlantı noktası dağıtabilirsiniz.  İki VM için, en fazla 32.000 SNAT bağlantı noktası giden bir kuralla (2x 32.000 = 64.000) ayrılabilir.

[Giden bağlantıları](load-balancer-outbound-connections.md) ve [SNAT](load-balancer-outbound-connections.md#snat) bağlantı noktalarının nasıl tahsis edildiği ve kullanıldığıyla ilgili ayrıntıları gözden geçirin.

### <a name="enable-outbound-only"></a><a name="outboundonly"></a>Yalnızca gideni etkinleştirme

Bir grup VM için giden NAT sağlamak için ortak bir Standart Yük Dengeleyicisi kullanabilirsiniz. Bu senaryoda, ek kurallara gerek kalmadan giden kuralı tek başına kullanabilirsiniz.

#### <a name="outbound-nat-for-vms-only-no-inbound"></a>Yalnızca VM'ler için giden NAT (gelen eki yok)

Ortak bir Standart Yük Dengeleyicisi tanımlayın, VM'leri arka uç havuzuna yerleştirin ve giden NAT'yi programlayacak giden bir kuralı yapılandırın ve giden bağlantıları belirli bir genel IP adresinden kaynaklayacak şekilde damat. Ayrıca, giden bağlantıların kaynağını beyaz listelemayı basitleştiren genel bir IP öneki de kullanabilirsiniz.

1. Genel bir Standart Yük Dengeleyicisi oluşturun.
2. Bir arka uç havuzu oluşturun ve VM'leri ortak Yük Dengeleyicisi'nin arka uç havuzuna yerleştirin.
3. Bu VM'ler için giden NAT'yi programlamak için ortak Yük Dengeleyicisi'nde giden bir kuralı yapılandırın.

#### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>Dahili Standart Yük Dengeleyici senaryoları için giden NAT

Dahili bir Standart Yük Dengeleyicisi kullanırken, giden bağlantı açıkça bildirilene kadar giden NAT kullanılamaz. Bu adımlarla bir dahili Standart Yük Dengeleyiciarkasında VM'ler için giden bağlantı oluşturmak için giden bir kural kullanarak giden bağlantı tanımlayabilirsiniz:

1. Genel bir Standart Yük Dengeleyicisi oluşturun.
2. Bir arka uç havuzu oluşturun ve VM'leri dahili Yük Dengeleyicisine ek olarak kamu Yük Dengeleyicisinin arka uç havuzuna yerleştirin.
3. Bu VM'ler için giden NAT'yi programlamak için ortak Yük Dengeleyicisi'nde giden bir kuralı yapılandırın.

#### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Giden NAT için her iki TCP & UDP protokolünü ortak standart yük dengeleyicisiyle etkinleştirme

- Ortak bir Standart Yük Dengeleyicisi kullanırken, sağlanan otomatik giden NAT programlama, yük dengeleme kuralının aktarım protokolüyle eşleşir.  

   1. Giden SNAT'ı yük dengeleme kuralında devre dışı.
   2. Aynı Yük Dengeleyicisi üzerinde giden kuralı yapılandırın.
   3. VM'leriniz tarafından zaten kullanılan arka uç havuzunu yeniden kullanın.
   4. Giden kuralın bir parçası olarak "protokol": "Tümü" belirtin.

- Yalnızca gelen NAT kuralları kullanıldığında, giden NAT sağlanmaz.

   1. VM'leri arka uç havuzuna yerleştirin.
   2. Ortak IP adresi(es) veya genel IP önekiyle bir veya daha fazla ön uç IP yapılandırması tanımlayın.
   3. Aynı Yük Dengeleyicisi üzerinde giden kuralı yapılandırın.
   4. Giden kuralın bir parçası olarak "protokol": "Tümü" belirtin

## <a name="limitations"></a>Sınırlamalar

- Ön uç IP adresi başına kullanılabilir geçici bağlantı noktası sayısı 64.000'dir.
- Yapılandırılabilir giden boşta zaman aralığı 4 ila 120 dakika (240 ila 7200 saniye) olduğunu.
- Yük Dengeleyici giden NAT için ICMP desteklemez.
- Giden kurallar yalnızca bir NIC'nin birincil IP yapılandırmasına uygulanabilir.  Birden çok NIC desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Giden bağlantılar için Yük Dengeleyicisi'ni](load-balancer-outbound-connections.md)kullanma hakkında bilgi edinin.
- Standart [Yük Dengeleyicisi](load-balancer-standard-overview.md)hakkında bilgi edinin.
- [Boşta zaman diliminde çift yönlü TCP Sıfırlama](load-balancer-tcp-reset.md)hakkında bilgi edinin.
- [Giden kuralları Azure CLI 2.0 ile yapılandırın.](configure-load-balancer-outbound-cli.md)
