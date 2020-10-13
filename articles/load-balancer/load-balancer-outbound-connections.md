---
title: Giden bağlantılar
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure 'un ortak internet hizmetleriyle iletişim kurması için VM 'nin nasıl çalıştığı açıklanır.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: contperfq1
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/30/2020
ms.author: allensu
ms.openlocfilehash: 6b9f454c75a10644e86931dc86ebd9514e5431d3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91649805"
---
# <a name="outbound-connections"></a>Giden bağlantılar

Azure Load Balancer, farklı mekanizmalarda giden bağlantı sağlar. Bu makalede senaryolar ve bunların nasıl yönetileceği açıklanmaktadır. 


## <a name="scenarios"></a>Senaryolar

* Ortak IP 'si olan sanal makine.
* Ortak IP olmadan sanal makine.
* Ortak IP olmadan ve standart yük dengeleyici olmadan sanal makine.

### <a name="virtual-machine-with-public-ip"></a><a name="scenario1"></a>Ortak IP 'si olan sanal makine

| İçermektedir | Yöntem | IP protokolleri |
| ---------- | ------ | ------------ |
| Ortak yük dengeleyici veya tek başına | [SNAT (kaynak ağ adresi çevirisi)](#snat) </br> [Pat (bağlantı noktası)](#pat) kullanılmıyor. | TCP (Iletim Denetim Protokolü) </br> UDP (Kullanıcı Datagram Protokolü) </br> ICMP (Internet Denetim Iletisi Protokolü) </br> ESP (Kapsüllenen Güvenlik Yükü) |

#### <a name="description"></a>Açıklama

Azure, tüm giden akışlar için örneğin NIC 'in IP yapılandırmasına atanan genel IP 'yi kullanır. Örnekte, tüm kısa ömürlü bağlantı noktaları kullanılabilir. VM 'nin yük dengeli olup olmadığı önemi yoktur. Bu senaryo diğerlerine göre önceliklidir. 

Bir VM 'ye atanan genel IP, 1:1 ilişkidir (1: çok) ve durum bilgisiz 1:1 NAT olarak uygulanır.

### <a name="virtual-machine-without-public-ip"></a><a name="scenario2"></a>Ortak IP olmadan sanal makine

| İçermektedir | Yöntem | IP protokolleri |
| ------------ | ------ | ------------ |
| Genel yük dengeleyici | [(Bağlantı noktası)](#pat)ile [SNAT](#snat) için yük dengeleyici ön ucu kullanımı.| TCP </br> UDP |

#### <a name="description"></a>Açıklama

Yük dengeleyici kaynağı bir yük dengeleyici kuralıyla yapılandırıldı. Bu kural, arka uç havuzuyla genel IP ön ucu arasında bağlantı oluşturmak için kullanılır. 

Bu kural yapılandırmasını tamamlamazsanız, Senaryo 3 ' te açıklanmaktadır. 

Sistem durumu araştırmasının başarılı olması için dinleyici içeren bir kural gerekli değildir.

Bir VM bir giden akış oluşturduğunda, Azure Kaynak IP adresini ortak yük dengeleyici ön ucu genel IP adresine çevirir. Bu çeviri, [SNAT](#snat)aracılığıyla yapılır. 

Yük dengeleyici ön uç genel IP adresinin kısa ömürlü bağlantı noktaları, VM 'ler tarafından kaynaklı bireysel akışları ayırt etmek için kullanılır. SNAT, giden akışlar oluşturulduğunda, [önceden ayrılmış kısa ömürlü bağlantı noktalarını](#preallocatedports) dinamik olarak kullanır. 

Bu bağlamda, SNAT için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları olarak adlandırılır. SNAT bağlantı noktaları [varsayılan SNAT bağlantı noktaları ayırma tablosunda](#snatporttable)açıklandığı şekilde önceden ayrılır.

### <a name="virtual-machine-without-public-ip-and-without-standard-load-balancer"></a><a name="scenario3"></a>Ortak IP olmadan ve standart yük dengeleyici olmadan sanal makine

| İçermektedir | Yöntem | IP protokolleri |
| ------------ | ------ | ------------ |
|Hiçbiri </br> Temel yük dengeleyici | Bağlantı noktası geçici olarak [SNAT](#snat) [(Pat)](#pat)| TCP </br> UDP | 

#### <a name="description"></a>Açıklama

VM bir giden akış oluşturduğunda, Azure Kaynak IP adresini ortak kaynak IP adresine çevirir. Bu genel IP adresi **yapılandırılamaz** ve ayrılamaz. Bu adres, aboneliğin genel IP kaynak sınırına göre sayılmaz. 

Genel IP adresi serbest bırakılır ve yeniden dağıtıyorsanız yeni bir genel IP istenir: 

* Sanal Makine
* Kullanılabilirlik kümesi
* Sanal makine ölçek kümesi  

Bir izin verilenler listesine IP eklemek için bu senaryoyu kullanmayın. Açık olarak giden davranışı bildirdiğiniz Senaryo 1 veya 2 ' i kullanın. [SNAT](#snat) bağlantı noktaları [varsayılan SNAT bağlantı noktaları ayırma tablosunda](#snatporttable)açıklandığı şekilde önceden ayrılır.



## <a name="port-allocation-algorithm"></a><a name="preallocatedports"></a>Bağlantı noktası ayırma algoritması

Azure, önceden ayrılan [SNAT](#snat) bağlantı noktalarının sayısını belirlemede kullanılan bir algoritma kullanır. Algoritma, arka uç havuzunun boyutundaki bağlantı noktası sayısını temel alır. 

Yük dengeleyiciyle ilişkili her genel IP adresi için 64.000 bağlantı noktası, [SNAT](#snat) bağlantı noktaları olarak kullanılabilir. UDP ve TCP için aynı sayıda [SNAT](#snat) bağlantı noktası önceden ayrılır. Bağlantı noktaları IP protokolü başına bağımsız olarak tüketilebilir. 

[SNAT](#snat) bağlantı noktası kullanımı, akışın UDP veya TCP olmasına bağlı olarak farklılık belirtir. 

Bağlantı noktaları, önceden ayrılan sınıra dinamik olarak tüketilebilir.  Akış kapandığında veya bir boşta kalma zaman aşımı oluştuğunda bağlantı noktaları serbest bırakılır.

Boşta kalma zaman aşımları hakkında daha fazla bilgi için bkz. [Azure Load Balancer giden bağlantılarda sorun giderme](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) 

Bağlantı noktaları yalnızca akışları benzersiz hale getirmek için gerekliyse kullanılır.

### <a name="dynamic-snat-ports-preallocation"></a><a name="snatporttable"></a> Dinamik SNAT bağlantı noktaları ön ayırması

Aşağıdaki tabloda, arka uç havuz boyutlarının katmanları için [SNAT](#snat) bağlantı noktası ön ayırmaları gösterilmektedir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına önceden ayrılmış SNAT bağlantı noktaları |
| --- | --- |
| 1-50 | 1.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 |

Arka uç havuzunuzun boyutunu değiştirmek, sağlanan akışlarınızdan bazılarını etkileyebilir:

- Arka uç havuzu boyutu, tetikleme geçişlerini sonraki katmana yükseltir. Önceden ayrılan [SNAT](#snat) bağlantı noktalarının yarısı, sonraki katmana geçiş sırasında geri kazanılır. 

- Geri kazanılan [SNAT](#snat) bağlantı noktası zaman aşımı ile ilişkili akışlar ve kapatma. Bu akışların yeniden kurulması gerekir. Yeni bir akış deneniyorsa, önceden ayrılan bağlantı noktaları kullanılabildiği sürece akış hemen başarılı olur.

- Arka uç havuzu boyutu daha düşük bir katmana düşürüyorsa ve geçiş yaptığında, kullanılabilir [SNAT](#snat) bağlantı noktalarının sayısı artar. Var olan [SNAT](#snat) bağlantı noktaları ve ilgili akışları etkilenmez.

## <a name="outbound-rules"></a><a name="outboundrules"></a>Giden kuralları

 Giden kuralları ortak [Standart yük dengeleyicinin](load-balancer-standard-overview.md)giden ağ adresi çevirisi yapılandırmasını etkinleştirir.  

> [!NOTE]
> **Azure sanal ağ NAT** , bir sanal ağdaki sanal makineler için giden bağlantı sağlayabilir.  Daha fazla bilgi için bkz. [Azure sanal ağ NAT nedir?](../virtual-network/nat-overview.md) .

Bu özelliği gereksinimlerinize göre ölçeklendirmeye ve ayarlamaya yönelik giden bağlantı üzerinde tam bildirime dayalı denetiminiz vardır.

![Yük dengeleyici giden kuralları](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Giden kuralları kullanarak, giden NAT 'yi sıfırdan tanımlamak için yük dengeleyici kullanabilirsiniz. Ayrıca varolan giden NAT 'nin davranışını ölçeklendirebilir ve ayarlayabilirsiniz.

Giden kuralları denetlemenize izin verir:

- Hangi sanal makinelerin genel IP adreslerine çevrilmesi gerekir.
- Giden [SNAT](#snat) bağlantı noktaları nasıl verilmelidir.
- İçin giden çeviri sağlayacak olan protokoller.
- Giden bağlantı boşta kalma zaman aşımı (4-120 dakika) için kullanılacak süre.
- Boşta kalma zaman aşımı durumunda TCP sıfırlaması gönderileceğini belirtir
- Tek bir kuralla hem TCP hem de UDP aktarım protokolleri

### <a name="outbound-rule-definition"></a>Giden kuralı tanımı

Giden kuralları yük dengeleme ve gelen NAT kuralları ile aynı tanıdık sözdizimini izler: **ön uç**  +  **parametreleri**  +  **arka uç havuzu**. Giden bir kural _, arka uç havuzu tarafından belirtilen tüm sanal makineler_ IÇIN giden NAT 'yi _ön_uca çevrilecek şekilde yapılandırır.  _Parametreler_ , giden NAT algoritması üzerinde ek ayrıntılı denetim sağlar.

### <a name="scale-outbound-nat-with-multiple-ip-addresses"></a><a name="scale"></a> Birden çok IP adresiyle giden NAT 'yi ölçeklendirme

Bir ön uç tarafından sağlanan her ek IP adresi, yük dengeleyici için SNAT bağlantı noktası olarak kullanılacak ek 64.000 kısa ömürlü bağlantı noktaları sağlar. 

Büyük ölçekli senaryolar planlamak için birden çok IP adresi kullanın. [SNAT tükenmesi](troubleshoot-outbound-connection.md#snatexhaust)azaltmak için giden kurallarını kullanın. 

[Genel BIR IP önekini](https://aka.ms/lbpublicipprefix) doğrudan giden bir kuralla de kullanabilirsiniz. 

Genel bir IP öneki, dağıtımınızın ölçeklendirilmesini arttırır. Ön ek, Azure kaynaklarınızdan kaynaklanan akışların izin verilenler listesine eklenebilir. Yük dengeleyicide bir ön uç IP yapılandırması genel IP adresi ön ekine başvuracak şekilde yapılandırılabilir.  

Yük dengeleyicinin genel IP öneki üzerinde denetimi vardır. Giden kuralı, giden bağlantılar için genel IP ön eki içinde bulunan tüm genel IP adreslerini otomatik olarak kullanacaktır. 

Genel IP ön eki içindeki IP adreslerinden her biri, yük dengeleyicinin SNAT bağlantı noktası olarak kullanması için IP adresi başına ek 64.000 kısa ömürlü bağlantı noktaları sağlar.

### <a name="outbound-flow-idle-timeout-and-tcp-reset"></a><a name="idletimeout"></a> Giden akış boşta kalma zaman aşımı ve TCP sıfırlaması

Giden kuralları, giden akış boşta kalma zaman aşımını denetlemek ve uygulamanızın gereksinimleriyle eşleştirmek için bir yapılandırma parametresi sağlar. Çıkış boşta kalma zaman aşımları varsayılan olarak 4 dakikadır. Daha fazla bilgi için bkz. [boştaki zaman aşımlarını yapılandırma](load-balancer-tcp-idle-timeout.md). 

Yük dengeleyicinin varsayılan davranışı, giden boşta kalma zaman aşımına ulaşıldığında akışı sessizce düşürülemiyor. `enableTCPReset`Parametresi, öngörülebilir bir uygulama davranışı ve denetimi sunar. Parametresi, giden boşta kalma zaman aşımı süresi dolduğunda çift yönlü TCP sıfırlaması (TCP RST) gönderilmesini belirler. 

Bölge kullanılabilirliği dahil Ayrıntılar için [boşta kalma zaman aşımı durumunda TCP sıfırlamayı](https://aka.ms/lbtcpreset) gözden geçirin.

### <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Giden bağlantıyı önlemek

Yük Dengeleme kuralları, giden NAT 'nin otomatik olarak programlamayı sağlar. Bazı senaryolar, giden NAT 'nin otomatik programlamayı Yük Dengeleme kuralıyla devre dışı bırakmanızı veya gerektirmesini gerektirir. Kural aracılığıyla devre dışı bırakmak, davranışı denetlemenize veya iyileştirmenize olanak tanır.  

Bu parametreyi iki şekilde kullanabilirsiniz:

1. Giden SNAT için gelen IP adresini önleme. Yük Dengeleme kuralında giden SNAT 'yi devre dışı bırakın.
  
2. Gelen ve giden için aynı anda kullanılan bir IP adresinin giden [SNAT](#snat) parametrelerini ayarlayın. Giden bir kuralın denetim geçirmesine izin vermek için otomatik giden NAT 'nin devre dışı bırakılması gerekir. Gelen için de kullanılan bir adresin SNAT bağlantı noktası ayırmasını değiştirmek için, `disableOutboundSnat` parametrenin true olarak ayarlanması gerekir. 

Gelen için kullanılan bir IP adresini yeniden tanımlamayı denerseniz, giden kuralı yapılandırma işlemi başarısız olur.  Önce yük dengeleme kuralının giden NAT 'ı devre dışı bırakın.

>[!IMPORTANT]
> Bu parametreyi true olarak ayarlarsanız ve giden bağlantıyı tanımlamak için bir giden kuralı yoksa, sanal makinenizin giden bağlantısı olmayacaktır.  VM 'nizin veya uygulamanızın bazı işlemleri giden bağlantıya sahip olmaya bağlı olabilir. Senaryonuzun bağımlılıklarını anladığınızdan ve bu değişikliği yapmanın etkileri olduğuna emin olun.

Bazen bir VM 'nin giden akış oluşturması istenmeyen bir şekilde yapılır. Hangi hedeflerin giden akışları alacağını veya hangi hedeflerin gelen akışlara başlayacağını yönetmek için bir gereksinim olabilir. VM 'nin ulaştığı hedefleri yönetmek için [ağ güvenlik gruplarını](../virtual-network/security-overview.md) kullanın. Hangi ortak hedeflerin gelen akışları başlattığını yönetmek için NSG 'leri kullanın.

Yük dengeli bir VM 'ye NSG uyguladığınızda, [hizmet etiketlerine](../virtual-network/security-overview.md#service-tags) ve [varsayılan güvenlik kurallarına](../virtual-network/security-overview.md#default-security-rules)dikkat edin. VM 'nin Azure Load Balancer durum araştırma isteklerini almasına emin olun.

Bir NSG AZURE_LOADBALANCER varsayılan etiketten durum araştırma isteklerini engelliyorsa, sanal makine sistem durumu araştırmanız başarısız olur ve VM kullanılamaz olarak işaretlenir. Load Balancer, bu VM 'ye yeni akış göndermeyi durduruyor.

## <a name="scenarios-with-outbound-rules"></a>Giden kuralları olan senaryolar

### <a name="outbound-rules-scenarios"></a>Giden kuralları senaryoları

* Belirli bir genel IP veya ön ek kümesine giden bağlantılar yapılandırın.
* [SNAT](#snat) bağlantı noktası ayırmasını değiştirin.
* Yalnızca gideni etkinleştirin.
* Yalnızca VM 'Ler için giden NAT (gelen yok).
* İç standart yük dengeleyici için giden NAT.
* Genel bir standart yük dengeleyici ile giden NAT için TCP & UDP protokollerini etkinleştirin.

### <a name="configure-outbound-connections-to-a-specific-set-of-public-ips-or-prefix"></a><a name="scenario1out"></a>Belirli bir genel IP veya ön ek kümesine giden bağlantıları yapılandırma

#### <a name="details"></a>Ayrıntılar

Giden bağlantıların bir genel IP adresi kümesinden kaynaklanacak şekilde uyarlamak için bu senaryoyu kullanın. Genel IP 'Leri veya önekleri, alma temelli bir izin verme veya reddetme listesine ekleyin.

Bu genel IP veya ön ek, bir yük dengeleme kuralı tarafından kullanılan ile aynı olabilir. 

Yük Dengeleme kuralı tarafından kullanılan farklı bir genel IP veya ön ek kullanmak için:  

1. Genel IP öneki veya genel IP adresi oluşturun.
2. Ortak standart yük dengeleyici oluşturma 
3. Kullanmak istediğiniz genel IP ön ekine veya genel IP adresine başvuran bir ön uç oluşturun. 
4. Arka uç havuzunu yeniden kullanma veya arka uç havuzu oluşturma ve VM 'Leri ortak yük dengeleyicinin arka uç havuzuna yerleştirme
5. Ön uç kullanan VM 'Ler için giden NAT 'yi etkinleştirmek üzere ortak yük dengeleyicide giden bir kural yapılandırın. Yük Dengeleme kuralının giden için kullanılmasını istemiyorsanız, Yük Dengeleme kuralında giden SNAT 'yi devre dışı bırakın.

### <a name="modify-snat-port-allocation"></a><a name="scenario2out"></a>[SNAT](#snat) bağlantı noktası ayırmayı Değiştir

#### <a name="details"></a>Ayrıntılar

[Arka uç havuzu boyutuna göre OTOMATIK SNAT bağlantı noktası ayırmayı](load-balancer-outbound-connections.md#preallocatedports)ayarlamak için giden kurallarını kullanabilirsiniz. 

SNAT tükenmesi yaşarsanız, varsayılan 1024 olan [SNAT](#snat) bağlantı noktalarının sayısını artırın. 

Her genel IP adresi 64.000 kısa ömürlü bağlantı noktasına katkıda bulunur. Arka uç havuzundaki sanal makinelerin sayısı, her sanal makineye dağıtılan bağlantı noktası sayısını belirler. Arka uç havuzundaki bir sanal makinenin en fazla 64.000 bağlantı noktasına erişimi vardır. İki VM için, bir giden kuralla en fazla 32.000 [SNAT](#snat) bağlantı noktasına (2x 32.000 = 64.000) verilebilir. 

Varsayılan olarak verilen SNAT bağlantı noktalarını ayarlamak için giden kurallarını kullanabilirsiniz. Varsayılan [SNAT](#snat) bağlantı noktası ayırmasının sağladığı daha fazlasını veya daha fazlasını verirsiniz. Giden kuralının ön noktasındaki her genel IP adresi, [SNAT](#snat) bağlantı noktaları olarak kullanılmak üzere 64.000 kısa ömürlü bağlantı noktasına kadar katkıda bulunur.  

Yük dengeleyici, [SNAT](#snat) bağlantı noktalarını 8 ' in katları halinde verir. 8 ' den bölünemez bir değer sağlarsanız, yapılandırma işlemi reddedilir. 

Genel IP adresi sayısına göre kullanılabilir olandan daha fazla [SNAT](#snat) bağlantı noktası vermenizi denerseniz, yapılandırma işlemi reddedilir.

VM başına 10.000 bağlantı noktası verirseniz ve arka uç havuzundaki yedi VM tek bir genel IP paylaşıyorsa, yapılandırma reddedilir. 10.000 ile çarpılan yedi, 64.000 bağlantı noktası sınırını aşıyor. Senaryoyu etkinleştirmek için giden kuralının ön ucunda daha fazla genel IP adresi ekleyin. 

Bağlantı noktası sayısı için 0 belirterek [varsayılan bağlantı noktası ayırmaya](load-balancer-outbound-connections.md#preallocatedports) dönün. İlk 50 VM örnekleri 1024 bağlantı noktalarını alacak, 51-100 sanal makine örnekleri en fazla örneğe kadar 512.  Varsayılan SNAT bağlantı noktası ayırma hakkında daha fazla bilgi için bkz. [SNAT bağlantı noktaları ayırma tablosu](#snatporttable).

### <a name="enable-outbound-only"></a><a name="scenario3out"></a>Yalnızca giden trafiği etkinleştir

#### <a name="details"></a>Ayrıntılar

Bir VM grubu için giden NAT sağlamak üzere genel bir standart yük dengeleyici kullanın. Bu senaryoda, herhangi bir ek kurala gerek duymadan, bir giden kuralı kendisi kullanın.

> [!NOTE]
> **Azure sanal ağ NAT** , yük dengeleyiciye gerek duymadan sanal makineler için giden bağlantı sağlayabilir.  Daha fazla bilgi için bkz. [Azure sanal ağ NAT nedir?](../virtual-network/nat-overview.md) .

### <a name="outbound-nat-for-vms-only-no-inbound"></a><a name="scenario4out"></a>Yalnızca VM 'Ler için giden NAT (gelen)

> [!NOTE]
> **Azure sanal ağ NAT** , yük dengeleyiciye gerek duymadan sanal makineler için giden bağlantı sağlayabilir.  Daha fazla bilgi için bkz. [Azure sanal ağ NAT nedir?](../virtual-network/nat-overview.md) .

#### <a name="details"></a>Ayrıntılar

Bu senaryo için:

1. Genel IP veya ön ek oluşturun.
2. Genel bir standart yük dengeleyici oluşturun. 
3. Giden için ayrılmış genel IP veya ön ek ile ilişkili bir ön uç oluşturun.
4. VM 'Ler için bir arka uç havuzu oluşturun.
5. VM 'Leri arka uç havuzuna yerleştirin.
6. Giden NAT 'yi etkinleştirmek için bir giden kuralı yapılandırın.

[SNAT](#snat) bağlantı noktalarını ölçeklendirmek için bir ön ek veya genel IP kullanın. Giden bağlantıların kaynağını bir izin verme veya reddetme listesine ekleyin.

### <a name="outbound-nat-for-internal-standard-load-balancer"></a><a name="scenario5out"></a>İç standart yük dengeleyici için giden NAT

> [!NOTE]
> **Azure sanal ağ NAT** , iç standart yük dengeleyiciyi kullanan sanal makineler için giden bağlantı sağlayabilir.  Daha fazla bilgi için bkz. [Azure sanal ağ NAT nedir?](../virtual-network/nat-overview.md) .

#### <a name="details"></a>Ayrıntılar

İç standart yük dengeleyici için giden bağlantı, açıkça bildirildiği sürece kullanılamaz. 

Daha fazla bilgi için bkz. [yalnızca giden yük dengeleyici yapılandırması](https://docs.microsoft.com/azure/load-balancer/egress-only).


### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a><a name="scenario6out"></a>Genel bir standart yük dengeleyici ile giden NAT için hem TCP & UDP protokollerini etkinleştirin

#### <a name="details"></a>Ayrıntılar

Ortak standart yük dengeleyici kullanıldığında, belirtilen otomatik giden NAT, Yük Dengeleme kuralının Aktarım Protokolü ile eşleşir. 

1. Yük Dengeleme kuralında giden [SNAT](#snat) 'yi devre dışı bırakın. 
2. Aynı yük dengeleyicide giden bir kural yapılandırın.
3. VM 'niz tarafından zaten kullanılan arka uç havuzunu yeniden kullanın. 
4. Giden kuralının bir parçası olarak "protokol": "tümü" ni belirtin. 

Yalnızca gelen NAT kuralları kullanıldığında, giden NAT sağlanmaz. 

1. VM 'Leri bir arka uç havuzuna yerleştirin.
2. Genel IP adresleri veya genel IP önekiyle bir veya daha fazla ön uç IP yapılandırması tanımlayın 
3. Aynı yük dengeleyicide giden bir kural yapılandırın. 
4. Giden kuralının bir parçası olarak "Protocol": "All" belirtin

## <a name="terminology"></a><a name="terms"></a> Terminolojiyi

### <a name="source-network-address-translation"></a><a name="snat"></a>Kaynak ağ adresi çevirisi

| Uygulanabilir protokoller |
|------------------------|
| TCP </br> UDP          |

#### <a name="details"></a>Ayrıntılar

Azure 'daki bir dağıtım, genel IP adresi alanında Azure dışındaki uç noktalarla iletişim kurabilir.

Bir örnek, genel IP adresi olan bir hedefe giden trafiği başlattığında, Azure kaynağın özel IP adresini dinamik olarak genel bir IP adresiyle eşler. 

Bu eşleme oluşturulduktan sonra, bu giden kaynaklı akış için geri dönüş trafiği akışın kaynaklandığı özel IP adresine ulaşır. 

Azure, bu işlevi yapmak için **kaynak ağ adresi çevirisini (SNAT)** kullanır.

### <a name="port-masquerading-snat-pat"></a><a name="pat"></a>Bağlantı noktası, SNAT (PAT)

| Uygulanabilir protokoller |
|------------------------|
| TCP </br> UDP          |

#### <a name="details"></a>Ayrıntılar

Özel IP 'Ler tek bir genel IP adresinin arkasında olduğunda, Azure özel IP adreslerini gizlemek için **bağlantı noktası adres çevirisini (Pat)** kullanır. 

Kısa ömürlü bağlantı noktaları, PAT için kullanılır ve havuz boyutuna bağlı olarak [önceden ayrılır](#preallocatedports) . 

Genel IP 'Leri olmayan bir ortak yük dengeleyici VM 'lerle ilişkilendirildiğinde, her giden bağlantı kaynağı yeniden yazılır. 

Kaynak, sanal ağ özel IP adresinden yük dengeleyicinin ön uç genel IP adresine yeniden yazılır. 

Genel IP adresi alanında, akışın beş kayıt düzeni benzersiz olmalıdır:

* Kaynak IP adresi
* Kaynak bağlantı noktası
* IP Aktarım Protokolü
* Hedef IP adresi
* Hedef bağlantı noktası 

Bağlantı noktası, TCP veya UDP protokolleriyle birlikte kullanılabilir. Birden çok akış tek bir genel IP adresinden kaynaklandığından, SNAT bağlantı noktaları özel kaynak IP adresini yeniden yazmadan sonra kullanılır. Bağlantı noktası geçici olarak, SNAT bağlantı noktalarını UDP ve TCP 'ye karşı farklı şekilde verir.

### <a name="snat-ports-tcp"></a>SNAT bağlantı noktaları (TCP)

| Uygulanabilir protokoller |
|------------------------|
| TCP |

#### <a name="details"></a>Ayrıntılar

SNAT bağlantı noktaları, genel IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktalarıdır. Tek bir hedef IP adresine ve bağlantı noktasına akış başına bir adet SNAT bağlantı noktası kullanılır. 

Aynı hedef IP adresine, bağlantı noktasına ve protokolüne birden çok TCP akışı için, her TCP akışı tek bir SNAT bağlantı noktası kullanır. 

Bu tüketim, akışların aynı genel IP adresinden kaynaklandıklarında benzersiz olmasını sağlar ve şu adrese gider:

* Aynı hedef IP adresi
* Bağlantı noktası
* Protokol 

Birden çok akış tek bir SNAT bağlantı noktasını paylaşır. 

Hedef IP adresi, bağlantı noktası ve protokol, genel IP adres alanındaki akışları ayırt etmek için ek kaynak bağlantı noktalarına gerek olmadan akış yapar.


### <a name="snat-ports-udp"></a>SNAT bağlantı noktaları (UDP)

| Uygulanabilir protokoller |
|------------------------|
| UDP |

#### <a name="details"></a>Ayrıntılar

UDP SNAT bağlantı noktaları, TCP SNAT bağlantı noktalarından farklı bir algoritma tarafından yönetilir.  Yük dengeleyici, UDP için bağlantı noktası kısıtlanmış koni NAT adlı bir algoritma kullanır.

Her akış için herhangi bir hedef IP adresi ve bağlantı noktası için bir SNAT bağlantı noktası kullanılır.


### <a name="exhaustion"></a><a name="exhaustion"></a>Tüken

| Uygulanabilir protokoller |
|------------------------|
| Yok |

#### <a name="details"></a>Ayrıntılar

SNAT bağlantı noktası kaynakları tükendiğinde, mevcut akışlar SNAT bağlantı noktalarını yayınlana kadar giden akışlar başarısız olur. Flow kapandığında yük dengeleyici geri kazanır SNAT bağlantı noktaları.

SNAT bağlantı noktalarını geri kazanmak için yük dengeleyici tarafından dört dakikalık bir [boşta kalma zaman aşımı](../load-balancer/troubleshoot-outbound-connection.md#idletimeout) kullanılır.

Kullanılan algoritmadaki fark nedeniyle UDP SNAT bağlantı noktaları genellikle TCP SNAT bağlantı noktalarından çok daha hızlı tükeiyor. Bu fark nedeniyle test tasarlayın ve ölçeklendirin.

### <a name="snat-port-release-behavior-tcp"></a>SNAT bağlantı noktası sürüm davranışı (TCP)

| Uygulanabilir protokoller |
|------------------------|
| TCP |

#### <a name="details"></a>Ayrıntılar

Bir sunucu ya da istemci bir FINACK gönderdiğinde, 240 saniye sonra bir SNAT bağlantı noktası serbest bırakılır. RST 'nin görüleceği durumda, 15 saniye sonra bir SNAT bağlantı noktası serbest bırakılır. Boşta kalma zaman aşımına ulaşılmışsa, bağlantı noktası serbest bırakılır.

### <a name="snat-port-release-behavior-udp"></a>SNAT bağlantı noktası sürüm davranışı (UDP)

| Uygulanabilir protokoller |
|------------------------|
| TCP |

#### <a name="details"></a>Ayrıntılar

Boşta kalma zaman aşımına ulaşılmışsa, bağlantı noktası serbest bırakılır.

### <a name="snat-port-reuse"></a>SNAT bağlantı noktası yeniden kullanımı

| Uygulanabilir protokoller |
|------------------------|
| TCP </br> UDP |

#### <a name="details"></a>Ayrıntılar

Bağlantı noktası yayımlandıktan sonra, bağlantı noktası yeniden kullanılabilir. SNAT bağlantı noktaları, belirli bir senaryo için en düşük ve en yüksek olan bir sıralamadır ve yeni bağlantılar için kullanılabilir ilk SNAT bağlantı noktası kullanılır.

## <a name="limitations"></a>Sınırlamalar

- Ön uç IP adresi başına kullanılabilir en fazla kısa ömürlü bağlantı noktası sayısı 64.000 ' dir.
- Yapılandırılabilir giden boşta kalma zaman aşımı aralığı 4 ila 120 dakikadır (240 ila 7200 saniye).
- Yük dengeleyici giden NAT için ıCMP 'yi desteklemez.
- Giden kuralları, yalnızca bir NIC 'in birincil IP yapılandırmasına uygulanabilir.  VM veya NVA 'nın ikincil IP 'si için giden bir kural oluşturamazsınız. Birden çok NIC desteklenir.
- Bir iç standart yük dengeleyici kullanıldığında, sanal ağ ve diğer Microsoft Platformu Hizmetleri olmayan Web çalışanı rollerine erişilebilir. Bu erişilebilirlik, ön VNet Hizmetleri ve diğer platform hizmetlerinin nasıl çalıştığı konusunda bir yan etkilik nedeniyle oluşur. İlgili hizmetin kendisi veya temel alınan platform hiçbir bildirimde bulunmaksızın değişmeksizin, bu yan etkiye güvenmeyin. Yalnızca iç standart yük dengeleyici kullandığınızda istenirse, her zaman giden bağlantıyı açıkça oluşturmanız gerektiğini varsayın. Bu makalede açıklanan senaryo 3 yok.

## <a name="next-steps"></a>Sonraki adımlar

Azure Load Balancer aracılığıyla giden bağlantılarla ilgili sorun yaşıyorsanız, [giden bağlantılar için sorun giderme kılavuzuna](../load-balancer/troubleshoot-outbound-connection.md)bakın.

- [Standart yük dengeleyici](load-balancer-standard-overview.md)hakkında daha fazla bilgi edinin.
- [Azure Load Balancer hakkında sık sorulan sorularımıza](load-balancer-faqs.md)bakın.
- Standart ortak yük dengeleyici için [giden kuralları](load-balancer-outbound-rules-overview.md) hakkında daha fazla bilgi edinin.

