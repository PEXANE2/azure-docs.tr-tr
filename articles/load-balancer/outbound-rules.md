---
title: Giden kuralları Azure Load Balancer
description: Bu makalede, Azure Load Balancer ile internet trafiğinin çıkışı denetlemek için giden kuralların nasıl yapılandırılacağı açıklanır.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/13/2020
ms.author: allensu
ms.openlocfilehash: 51810876e3636b7023ce9c9318a071636bb00c4c
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "92002655"
---
# <a name="outbound-rules-azure-load-balancer"></a><a name="outboundrules"></a>Giden kuralları Azure Load Balancer

Giden kuralları genel standart yük dengeleyici giden SNAT 'yi (kaynak ağ adresi çevirisi) yapılandırmanıza olanak tanır. Bu yapılandırma, yük dengeleyicinizin genel IP 'leri bir ara sunucu olarak kullanmanıza olanak tanır.

Bu yapılandırma şunları sunar:

* IP kendini gizleyen
* İzin verilenler listelerinizi basitleştirin.
* Dağıtım için genel IP kaynakları sayısını azaltır.

Giden kurallarla, giden internet bağlantısı üzerinde tam bildirime dayalı denetiminiz vardır. Giden kuralları, bu özelliği özel gereksinimlerinize göre ölçeklendirmenize ve ayarlamanıza olanak sağlar. 

Giden kuralları yalnızca arka uç VM 'sinin örnek düzeyi genel IP adresi (ıLPıP) yoksa izlenir.

![Giden kuralları Load Balancer](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Giden kurallarla, giden **SNAT** davranışını açık bir şekilde tanımlayabilirsiniz.

Giden kuralları denetlemenize izin verir:

* **Hangi sanal makinelerin hangi ortak IP adreslerine çevrildiği.**
     * İki kural, A ve B IP adresini kullanan arka uç havuzudur, B arka uç havuzu C ve D IP adresini kullanır.
* **Giden SNAT bağlantı noktaları nasıl verilir.**
     * Arka uç havuzu B giden bağlantılar yapan tek havuzdır, tüm SNAT bağlantı noktalarına B arka uç havuzuna izin verin ve bir arka uç havuzu A 'yı yok edin.
* **İçin giden çeviri sağlayacak olan protokoller.**
     * Arka uç havuzu B giden için UDP bağlantı noktalarına ihtiyaç duyuyor. Arka uç havuzunda TCP gerekir. B 'ye TCP bağlantı noktalarına ve UDP bağlantı noktalarına izin verin.
* **Giden bağlantı boşta kalma zaman aşımı (4-120 dakika) için kullanılacak süre.**
     * Keepsıt ile uzun süre çalışan bağlantılar varsa, uzun süre çalışan bağlantılar için boşta kalma bağlantı noktalarını 120 dakikaya kadar ayırın. Yeni bağlantılar için eski bağlantıların terk olduğunu ve sürüm bağlantı noktalarını 4 dakika içinde kabul edin 
* **Boşta kalma zaman aşımı durumunda TCP sıfırlaması gönderileceğini belirtir.**
     * boşta bağlantıları zaman aşımına uğradıklarında, akışın terk olduğunu bilmesi için istemciye ve sunucuya bir TCP RST gönderiyoruz.

## <a name="outbound-rule-definition"></a>Giden kuralı tanımı

Giden kuralları yük dengeleme ve gelen NAT kuralları ile aynı tanıdık sözdizimini izler: **ön uç**  +  **parametreleri**  +  **arka uç havuzu**. 

Giden bir kural _, arka uç havuzu tarafından belirtilen tüm sanal makineler_ IÇIN giden NAT 'yi _ön_uca çevrilecek şekilde yapılandırır.  

_Parametreler_ , giden NAT algoritması üzerinde ek ayrıntılı denetim sağlar.

## <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> Birden çok IP adresiyle giden NAT 'yi ölçeklendirme

Bir ön uç tarafından sağlanan her ek IP adresi, yük dengeleyici için SNAT bağlantı noktası olarak kullanılacak ek 64.000 kısa ömürlü bağlantı noktaları sağlar. 

Büyük ölçekli senaryolar planlamak için birden çok IP adresi kullanın. [SNAT tükenmesi](troubleshoot-outbound-connection.md#snatexhaust)azaltmak için giden kurallarını kullanın. 

[Genel BIR IP önekini](https://aka.ms/lbpublicipprefix) doğrudan giden bir kuralla de kullanabilirsiniz. 

Genel bir IP öneki, dağıtımınızın ölçeklendirilmesini arttırır. Ön ek, Azure kaynaklarınızdan kaynaklanan akışların izin verilenler listesine eklenebilir. Yük dengeleyicide bir ön uç IP yapılandırması genel IP adresi ön ekine başvuracak şekilde yapılandırılabilir.  

Yük dengeleyicinin genel IP öneki üzerinde denetimi vardır. Giden kuralı, giden bağlantılar için genel IP ön eki içinde bulunan tüm genel IP adreslerini otomatik olarak kullanacaktır. 

Genel IP ön eki içindeki IP adreslerinden her biri, yük dengeleyicinin SNAT bağlantı noktası olarak kullanması için IP adresi başına ek 64.000 kısa ömürlü bağlantı noktaları sağlar.

## <a name="outbound-flow-idle-timeout-and-tcp-reset"></a><a name="idletimeout"></a> Giden akış boşta kalma zaman aşımı ve TCP sıfırlaması

Giden kuralları, giden akış boşta kalma zaman aşımını denetlemek ve uygulamanızın gereksinimleriyle eşleştirmek için bir yapılandırma parametresi sağlar. Çıkış boşta kalma zaman aşımları varsayılan olarak 4 dakikadır. Daha fazla bilgi için bkz. [boştaki zaman aşımlarını yapılandırma](load-balancer-tcp-idle-timeout.md). 

Yük dengeleyicinin varsayılan davranışı, giden boşta kalma zaman aşımına ulaşıldığında akışı sessizce düşürülemiyor. `enableTCPReset`Parametresi, öngörülebilir bir uygulama davranışı ve denetimi sunar. Parametresi, giden boşta kalma zaman aşımı süresi dolduğunda çift yönlü TCP sıfırlaması (TCP RST) gönderilmesini belirler. 

Bölge kullanılabilirliği dahil Ayrıntılar için [boşta kalma zaman aşımı durumunda TCP sıfırlamayı](https://aka.ms/lbtcpreset) gözden geçirin.

## <a name="securing-and-controlling-outbound-connectivity-explicitly"></a><a name="preventoutbound"></a>Giden bağlantıyı açıkça güvenli hale getirme ve denetleme

Yük Dengeleme kuralları, giden NAT 'nin otomatik olarak programlamayı sağlar. Bazı senaryolar, giden NAT 'nin otomatik programlamayı Yük Dengeleme kuralıyla devre dışı bırakmanızı veya gerektirmesini gerektirir. Kural aracılığıyla devre dışı bırakmak, davranışı denetlemenize veya iyileştirmenize olanak tanır.  

Bu parametreyi iki şekilde kullanabilirsiniz:

1. Giden SNAT için gelen IP adresini önleme. Yük Dengeleme kuralında giden SNAT 'yi devre dışı bırakın.
  
2. Gelen ve giden için aynı anda kullanılan bir IP adresinin giden **SNAT** parametrelerini ayarlayın. Giden bir kuralın denetim geçirmesine izin vermek için otomatik giden NAT 'nin devre dışı bırakılması gerekir. Gelen için de kullanılan bir adresin SNAT bağlantı noktası ayırmasını değiştirmek için, `disableOutboundSnat` parametrenin true olarak ayarlanması gerekir. 

Gelen için kullanılan bir IP adresini yeniden tanımlamayı denerseniz, giden kuralı yapılandırma işlemi başarısız olur.  Önce yük dengeleme kuralının giden NAT 'ı devre dışı bırakın.

>[!IMPORTANT]
> Bu parametreyi true olarak ayarlarsanız ve giden bağlantıyı tanımlamak için bir giden kuralı yoksa, sanal makinenizin giden bağlantısı olmayacaktır.  VM 'nizin veya uygulamanızın bazı işlemleri giden bağlantıya sahip olmaya bağlı olabilir. Senaryonuzun bağımlılıklarını anladığınızdan ve bu değişikliği yapmanın etkileri olduğuna emin olun.

Bazen bir VM 'nin giden akış oluşturması istenmeyen bir şekilde yapılır. Hangi hedeflerin giden akışları alacağını veya hangi hedeflerin gelen akışlara başlayacağını yönetmek için bir gereksinim olabilir. VM 'nin ulaştığı hedefleri yönetmek için [ağ güvenlik gruplarını](../virtual-network/security-overview.md) kullanın. Hangi ortak hedeflerin gelen akışları başlattığını yönetmek için NSG 'leri kullanın.

Yük dengeli bir VM 'ye NSG uyguladığınızda, [hizmet etiketlerine](../virtual-network/security-overview.md#service-tags) ve [varsayılan güvenlik kurallarına](../virtual-network/security-overview.md#default-security-rules)dikkat edin. 

VM 'nin Azure Load Balancer durum araştırma isteklerini almasına emin olun.

Bir NSG AZURE_LOADBALANCER varsayılan etiketten durum araştırma isteklerini engelliyorsa, sanal makine sistem durumu araştırmanız başarısız olur ve VM kullanılamaz olarak işaretlenir. Yük dengeleyici bu VM 'ye yeni akış göndermeyi durduruyor.

## <a name="limitations"></a>Sınırlamalar

- Ön uç IP adresi başına kullanılabilir en fazla kısa ömürlü bağlantı noktası sayısı 64.000 ' dir.
- Yapılandırılabilir giden boşta kalma zaman aşımı aralığı 4 ila 120 dakikadır (240 ila 7200 saniye).
- Yük dengeleyici giden NAT için ıCMP 'yi desteklemez.
- Giden kuralları, yalnızca bir NIC 'in birincil IP yapılandırmasına uygulanabilir.  VM veya NVA 'nın ikincil IP 'si için giden bir kural oluşturamazsınız. Birden çok NIC desteklenir.
- Bir **kullanılabilirlik kümesindeki** tüm sanal makinelerin, giden bağlantı için arka uç havuzuna eklenmesi gerekir. 
- Bir **sanal makine ölçek kümesindeki** tüm sanal makinelerin giden bağlantı için arka uç havuzuna eklenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Standart Load Balancer](load-balancer-overview.md) hakkında daha fazla bilgi
- [Azure Load Balancer hakkında sık sorulan sorularımızı](load-balancer-faqs.md) inceleyin

