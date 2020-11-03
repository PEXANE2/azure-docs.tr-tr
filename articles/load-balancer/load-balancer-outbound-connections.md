---
title: Giden proxy Azure Load Balancer
description: Azure Load Balancer giden internet bağlantısı için proxy olarak nasıl kullanılacağını açıklar
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/13/2020
ms.author: allensu
ms.openlocfilehash: 185bb47677e978a3098f39024995da6399f90658
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93241778"
---
# <a name="outbound-proxy-azure-load-balancer"></a>Giden proxy Azure Load Balancer

Azure yük dengeleyici, giden internet bağlantısı için bir proxy olarak kullanılabilir. Yük dengeleyici, arka uç örnekleri için giden bağlantıyı sağlar. 

Bu yapılandırma, **kaynak ağ adresi çevirisi 'ni (SNAT)** kullanır. SNAT, arka ucun IP adresini yük dengeleyicinizin genel IP adresine yeniden yazar. 

SNAT, arka uç örneğinin **IP** 'yi aşağı olarak etkinleştirmesine izin vermez. Bu aşağı doğru, dış kaynakların arka uç örneklerine doğrudan bir adres olmasını önler. 

Arka uç örnekleri arasında bir IP adresi paylaşımı, statik genel IP 'lerin maliyetini azaltır ve IP 'nin bilinen genel IP 'lerden gelen trafiğe izin verme gibi senaryoları destekler. 

## <a name="sharing-ports-across-resources"></a><a name ="snat"></a> Kaynaklar arasında bağlantı noktalarını paylaşma

Bir yük dengeleyicinin arka uç kaynakları, örnek düzeyi genel IP (ıLPıP) adreslerine sahip değilse, genel yük dengeleyicinin ön uç IP 'si aracılığıyla giden bağlantı kurar.

Bağlantı noktaları, ayrı akışları korumak için kullanılan benzersiz tanımlayıcılar oluşturmak için kullanılır. Internet, bu ayrımı sağlamak için beş tanımlama grubu kullanır.

Beş demet aşağıdakilerden oluşur:

* Hedef IP
* Hedef bağlantı noktası
* Kaynak IP
* Bu ayrımı sağlamak için kaynak bağlantı noktası ve protokol.

Gelen bağlantılar için bir bağlantı noktası kullanılıyorsa, bu bağlantı noktasındaki gelen bağlantı istekleri için bir **dinleyici** olur ve giden bağlantılar için kullanılamaz. 

Giden bir bağlantı kurmak için, hedefe iletişim kurmak ve ayrı bir trafik akışının bakımını yapmak üzere bir bağlantı noktası sağlamak üzere **geçici bir bağlantı noktası** kullanılması gerekir. 

Her IP adresi 65.535 bağlantı noktasına sahiptir. İlk 1024 bağlantı noktaları, **sistem bağlantı noktaları** olarak ayrılmıştır. Her bağlantı noktası, TCP ve UDP için gelen veya giden bağlantılar için kullanılabilir. 

Azure, kalan bağlantı noktalarında, **kısa ömürlü bağlantı noktaları** olarak kullanılmak üzere 64.000 sağlar. Bir IP adresi, ön uç IP yapılandırması olarak eklendiğinde, bu kısa ömürlü bağlantı noktaları SNAT için kullanılabilir.

Giden kuralları aracılığıyla, bu SNAT bağlantı noktaları, giden bağlantılar için yük dengeleyicinin genel IP 'leri paylaşmasını sağlamak üzere arka uç örneklerine dağıtılabilir.

Her arka uç örneği için konaktaki ağ, bir giden bağlantının parçası olan paketlere SNAT 'yi kullanacaktır. Ana bilgisayar kaynak IP 'yi genel IP 'lerden birine yeniden yazar. Ana bilgisayar her giden paketin kaynak bağlantı noktasını SNAT bağlantı noktalarından birine yeniden yazar.

## <a name="exhausting-ports"></a><a name="scenarios"></a> Bağlantı noktaları tüketilme

Aynı hedef IP ve hedef bağlantı noktasına yapılan her bağlantı için bir SNAT bağlantı noktası kullanılır. Bu bağlantı, arka uç örneğinden veya **istemcisinden** bir **sunucuya** ayrı bir **trafik akışı** sağlar. Bu işlem, sunucuya trafiğin ele alınacağı ayrı bir bağlantı noktası sağlar. Bu işlem olmadan, istemci makinesi bir paketin bir parçası olduğu farkında değildir.

Birden çok tarayıcıyı giderek düşünün https://www.microsoft.com :

* Hedef IP = 23.53.254.142
* Hedef bağlantı noktası = 443
* Protokol = TCP

Geri dönüş trafiği için farklı hedef bağlantı noktaları olmadan (bağlantıyı kurmak için kullanılan SNAT bağlantı noktası), istemcinin bir sorgu sonucunu başka bir şekilde ayırabilmeyecektir.

Giden bağlantılar veri bloğu alabilir. Bir arka uç örneğine yetersiz bağlantı noktası tahsis edilebilir. **Bağlantı yeniden kullanımı** etkin OLMADıĞıNDA, SNAT **bağlantı noktası tükenmesi** riski artar.

Bağlantı noktası tükenmesi gerçekleştiğinde, hedef IP 'ye yönelik yeni giden bağlantılar başarısız olur. Bağlantı noktası kullanılabilir olduğunda bağlantı başarılı olur. Bu tükenme, bir IP adresinden 64.000 bağlantı noktası birçok arka uç örneğine ince yayıldığı zaman meydana gelir. SNAT bağlantı noktası tükenmesi 'ni azaltma hakkında yönergeler için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/load-balancer/troubleshoot-outbound-connection).  

TCP bağlantılarında, yük dengeleyici her hedef IP ve bağlantı noktası için tek bir SNAT bağlantı noktası kullanır. Bu Multiuse aynı SNAT bağlantı noktasıyla aynı hedef IP 'ye birden çok bağlantı sağlar. Bağlantı farklı hedef bağlantı noktalarına değilse bu Multiuse sınırlı olur.

UDP bağlantılarında, yük dengeleyici hedef bağlantı noktasına her ne kadar hedef IP başına bir SNAT bağlantı noktası tüketen bir **bağlantı noktası kısıtlanmış konı NAT** algoritması kullanır. 

Bağlantı noktası sınırsız sayıda bağlantı için yeniden kullanılır. Bağlantı noktası yalnızca hedef IP veya bağlantı noktası farklı olduğunda yeniden kullanılır.

## <a name="port-allocation"></a><a name="preallocatedports"></a> Bağlantı noktası ayırma

Yük dengeleyicinizin ön uç IP 'si olarak atanan her genel IP 'nin, arka uç havuzu üyeleri için 64.000 SNAT bağlantı noktası verilir. Bağlantı noktaları, arka uç havuzu üyeleriyle paylaşılamaz. Bir adet SNAT bağlantı noktası aralığı, dönüş paketlerinin doğru şekilde yönlendirildiğinden emin olmak için yalnızca tek bir arka uç örneği tarafından kullanılabilir. 

SNAT bağlantı noktası ayırmayı yapılandırmak için açık bir giden kuralı kullanmanız önerilir. Bu kural, her arka uç örneği giden bağlantılar için kullanılabilir olan SNAT bağlantı noktası sayısını en üst düzeye çıkarır. 

Yük Dengeleme kuralı aracılığıyla giden SNAT 'nin otomatik olarak tahsisini kullanmanız gerekir; ayırma tablosu, bağlantı noktası ayırmayı tanımlayacaktır.

Aşağıdaki <a name="snatporttable"></a> tabloda, arka uç havuz boyutlarının katmanları IÇIN SNAT bağlantı noktası ön ayırmaları gösterilmektedir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına önceden ayrılmış SNAT bağlantı noktaları |
| --- | --- |
| 1-50 | 1.024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 | 

>[!NOTE]
> En fazla 10 boyutuna sahip bir arka uç havuzunuz varsa, açık bir giden kuralı tanımlarsanız her örnek 64000/10 = 6.400 bağlantı noktasına sahip olabilir. Yukarıdaki tabloya göre her biri yalnızca otomatik ayırmayı seçerseniz 1.024 olur.

## <a name="outbound-rules-and-virtual-network-nat"></a><a name="outboundrules"></a> Giden kuralları ve sanal ağ NAT

Azure Load Balancer giden kuralları ve sanal ağ NAT, bir sanal ağdan çıkış için kullanılabilir seçeneklerdir.

Giden kuralları hakkında daha fazla bilgi için bkz. [giden kuralları](outbound-rules.md).

Azure sanal ağ NAT hakkında daha fazla bilgi için bkz. [Azure sanal ağ NAT nedir?](../virtual-network/nat-overview.md).

## <a name="constraints"></a>Kısıtlamalar

*   **TCP RST** alınıp gönderiliyorsa, bağlantı noktaları 15 saniye sonra serbest bırakılır
*   Bir **Finack** alınmışsa veya gönderilirse 240 saniye sonra bağlantı noktaları serbest bırakılır
*   Bir bağlantı yeni paket gönderilmeden boşta kaldığında, bağlantı noktaları 4 – 120 dakika sonra serbest bırakılır.
  * Bu eşik, giden kuralları aracılığıyla yapılandırılabilir.
*   Her IP adresi, SNAT için kullanılabilecek 64.000 bağlantı noktası sağlar.
*   Her bağlantı noktası, hedef IP adresine yönelik TCP ve UDP bağlantıları için kullanılabilir
  * Hedef bağlantı noktasının benzersiz olup olmadığı bir UDP SNAT bağlantı noktası gerekir. Bir hedef IP 'ye yönelik her UDP bağlantısı için, bir UDP SNAT bağlantı noktası kullanılır.
  * Bir TCP SNAT bağlantı noktası, hedef bağlantı noktaları farklı olduğundan aynı hedef IP 'ye birden fazla bağlantı için kullanılabilir.
*   SNAT tükenmesi, bir arka uç örneği verilen SNAT bağlantı noktalarından çıktığında oluşur. Yük dengeleyici hala kullanılmayan SNAT bağlantı noktalarına sahip olabilir. Bir arka uç örneğinin kullanılan SNAT bağlantı noktaları, belirtilen SNAT bağlantı noktalarını aşarsa, yeni giden bağlantılar kurulamaz.

## <a name="next-steps"></a>Sonraki adımlar

*   [SNAT tükenmesi nedeniyle giden bağlantı hatalarıyla ilgili sorunları giderme](https://docs.microsoft.com/azure/load-balancer/troubleshoot-outbound-connection)
*   [SNAT ölçümlerini gözden geçirin](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-snat-port-usage-and-allocation) ve bunları filtrelemeniz, bölmeniz ve görüntülemeniz için doğru yolu tanıyın.

