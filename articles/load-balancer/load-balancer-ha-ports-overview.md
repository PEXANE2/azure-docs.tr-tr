---
title: Azure yüksek kullanılabilirlik bağlantı noktaları genel bakış
titleSuffix: Azure Load Balancer
description: İç yük dengeleyici üzerinde yüksek kullanılabilirlik bağlantı noktaları yük dengelemeyi hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2019
ms.author: allensu
ms.openlocfilehash: c6529e2585a7fca2d160d093d303afa02e6f9379
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215071"
---
# <a name="high-availability-ports-overview"></a>Yüksek kullanılabilirlik bağlantı noktaları genel bakış

İç yük dengeleyici kullanıldığında azure standart Load Balancer, tüm bağlantı noktalarındaki Yük Dengeleme TCP ve UDP akışlar aynı anda yardımcı olur. 

Yüksek kullanılabilirlik (HA) bağlantı noktaları Yük Dengeleme kuralı, iç Standart Load Balancer yapılandırılan bir yük dengeleme kuralının bir varyantıdır. Tek bir kural yük dengelemek için bir iç standart Load Balancer'ın tüm bağlantı noktalarında gelen tüm TCP ve UDP akışlar sağlayarak bir yük dengeleyicinin kullanılmasını kolaylaştırabilirsiniz. Yük Dengeleme karar akış yapılır. Bu eylem, şu beş demet bağlantısına dayanır: kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokol

HA bağlantı noktaları Yük Dengeleme kuralları, sanal ağların içindeki ağ sanal gereçleri (NVA 'lar) için yüksek kullanılabilirlik ve ölçek gibi kritik senaryolarda size yardımcı olur. Özellik bağlantı noktaları, çok sayıda yük dengeli olması gerektiğinde de yardımcı olabilir. 

HA bağlantı noktaları Yük Dengeleme kuralları, ön uç ve arka uç bağlantı noktalarını **0** ' a ve Protokolü **tümüne**ayarladığınızda yapılandırılır. İç yük dengeleyici kaynağı, bağlantı noktası numarasından bağımsız olarak tüm TCP ve UDP akışlarını dengeler

## <a name="why-use-ha-ports"></a>HA bağlantı noktaları neden kullanmalısınız?

### <a name="nva"></a>Ağ sanal cihazları

Azure İş yükünüzün güvenlik tehditlerini birden çok tür güvenliğini sağlamaya yardımcı olmak için nva'ları kullanabilirsiniz. Bu senaryolarda nva'ları kullandığınızda, güvenilir ve yüksek oranda kullanılabilir olması gerekir ve bunlar için isteğe bağlı ölçeği gerekir.

İç load balancer arka uç havuzuna yalnızca NVA örneklerinin ekleyerek bu hedeflere elde edebileceğiniz ve yük dengeleyici kuralı yapılandırma bir HA bağlantı noktaları.

NVA HA senaryoları için HA bağlantı noktaları aşağıdaki avantajları sağlar:
- Hızlı yük devretmeyi sağlıklı örnekleri için örnek başına sistem durumu araştırmaları ile sağlayın
- *N*etkin örneklere genişleme ile daha yüksek performans sağlayın
- *N*-aktif ve aktif-pasif senaryolar sağlama
- Cihazları izleme için Apache ZooKeeper düğümleri gibi karmaşık çözümleri gerekmemesi

Aşağıdaki diyagramda bir merkez ve uç sanal ağ dağıtımı sunar. Uçlar zorlamalı tünel hub sanal ağa ve güvenilen alanı çıkmadan önce NVA aracılığıyla trafiği. HA bağlantı noktaları yapılandırmaya sahip bir iç standart yük dengeleyici arkasında nva'ları var. Tüm trafiği, işlenen ve buna göre iletilir. Aşağıdaki diyagramda göster olarak yapılandırıldığında, bir HA bağlantı noktaları Yük Dengeleme kuralı ek olarak giriş ve çıkış trafiği için Flow simetri sağlar.

<a node="diagram"></a>NVA 'lar, HA modunda dağıtılan merkez ve bağlı sanal ağın 
![diyagramı](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Nva'ları kullanıyorsanız sağlayıcıları ile HA bağlantı noktaları en iyi şekilde kullanmak ve hangi senaryolar desteklenir öğrenmek için onaylayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Bağlantı noktalarını çok sayıda Yük Dengeleme

HA bağlantı noktalarını bağlantı noktalarının çok sayıda Yük Dengeleme gerektiren uygulamalar için de kullanabilirsiniz. Bu senaryoları, HA bağlantı noktalarıyla bir iç [Standart Load Balancer](load-balancer-standard-overview.md) kullanarak basitleştirebilirsiniz. Tek bir yük dengeleyici kuralı, birden fazla bireysel Yük Dengeleme kuralları, her bağlantı noktası için bir tane yerine geçer.

## <a name="region-availability"></a>Bölgelere göre kullanılabilirlik

HA bağlantı noktaları özelliğini tüm genel Azure bölgelerinde kullanılabilir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerindeki tek, değişken olmayan bir IP (olmayan - doğrudan sunucu dönüşü) HA bağlantı noktaları yapılandırma

Bu yapılandırma bir temel HA bağlantı noktaları yapılandırmadır. Yapılandırabileceğiniz bir HA bağlantı noktaları Yük Dengeleme kuralı üzerinde tek bir ön uç IP adresi aşağıdakileri yaparak:
1. Standart Load Balancer yapılandırılırken, Load Balancer kuralı yapılandırmasındaki **ha bağlantı noktaları** onay kutusunu seçin.
2. **Kayan IP**Için **devre dışı**' yı seçin.

Bu yapılandırma diğer Yük Dengeleme kuralı yapılandırma geçerli yük dengeleyici kaynağına izin vermez. İç yük dengeleyici kaynağı için başka bir yapılandırma arka uç örneklerinin verilen kümesine izin verir.

Ancak, genel bir Standard Load Balancer arka uç örneklerinin HA bağlantı noktaları kuralın ek olarak yapılandırabilirsiniz.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici üzerindeki bir tek, kayan IP (doğrudan sunucu dönüşü) HA bağlantı noktaları yapılandırma

Benzer şekilde, **kayan IP** 'yi **etkin**olarak ayarlayarak, yük dengeleyicinizi tek bir ön uç ile **ha bağlantı noktasıyla** bir yük dengeleme kuralı kullanacak şekilde yapılandırabilirsiniz. 

Bu yapılandırmayı kullanarak kayan IP yük dengeleyici kuralları ve/veya herkese açık yük dengeleyici ekleyebilirsiniz. Ancak, HA bağlantı noktaları bir değişken olmayan IP kullanamazsınız. Bu yapılandırma üzerinde Yük Dengeleme yapılandırma.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Standart bir iç yük dengeleyici birden çok yapılandırmada HA bağlantı noktaları

Birden fazla HA bağlantı noktası ön uç için aynı arka uç havuzunu yapılandırma senaryonuz gerektiriyorsa, bunu yapabilirsiniz: 
- Birden fazla ön uç özel IP adresi tek bir iç standart yük dengeleyici kaynağı için yapılandırın.
- Her kural seçilen tek benzersiz bir ön uç IP adresine sahip olduğu birden çok Yük Dengeleme kuralları yapılandırın.
- **Ha bağlantı noktaları** seçeneğini belirleyin ve ardından tüm yük dengeleme kuralları IÇIN **kayan IP** 'yi **etkin** olarak ayarlayın.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>HA bağlantı noktaları ve aynı arka uç örneğinde bir genel yük dengeleyici ile iç yük dengeleyici

Arka uç kaynakları için *bir* genel standart Load Balancer KAYNAĞıNı, ha bağlantı noktalarıyla tek bir iç standart Load Balancer birlikte yapılandırabilirsiniz.

>[!NOTE]
>Bu özellik şu anda Azure Resource Manager şablonları kullanılabilir, ancak Azure portalı üzerinden kullanılabilir değil.

## <a name="limitations"></a>Sınırlamalar

- HA bağlantı noktaları Yük Dengeleme kuralları yalnızca iç Standart Load Balancer için kullanılabilir.
- Birleştirme, bir HA bağlantı noktaları Yük Dengeleme kuralı ve bir HA olmayan bağlantı noktaları Yük Dengeleme kuralı desteklenmiyor.
- Mevcut IP parçaları, ilk paket ile aynı hedefe HA bağlantı noktaları Yük Dengeleme kuralları tarafından iletilir.  IP fragmenting bir UDP veya TCP paketi desteklenmez.
- HA bağlantı noktaları Yük Dengeleme kuralları IPv6 için kullanılamaz.
- Akış simetrileri (öncelikli olarak NVA senaryolarında), yalnızca Yukarıdaki diyagramda gösterildiği gibi kullanıldığında ve HA bağlantı noktaları Yük Dengeleme kuralları kullanıldığında, arka uç örneği ve tek bir NIC (ve tek IP yapılandırması) ile desteklenir. Başka hiçbir senaryoda sağlanmaz. Bu, iki veya daha fazla Load Balancer kaynağı ve ilgili kuralları bağımsız kararlar verirken ve hiçbir şekilde koordine etmediği anlamına gelir. Bkz. [ağ sanal cihazları](#nva)için açıklama ve diyagram. Birden çok NIC kullandığınızda veya bir ortak ve dahili Load Balancer arasında NVA 'yı kullanarak, Flow simetri kullanılamıyor.  Yanıtların aynı NVA 'ya ulaşmasına izin vermek için, giriş akışını gereç IP 'si ile çözmek için kaynak tarafından geçici bir çözüm bulabilirsiniz.  Ancak, tek bir NIC kullanmanız ve Yukarıdaki diyagramda gösterilen başvuru mimarisini kullanmanız önemle önerilir.


## <a name="next-steps"></a>Sonraki adımlar

- [Bir iç Standart Load Balancer HA bağlantı noktalarını yapılandırma](load-balancer-configure-ha-ports.md)
- [Standart Load Balancer hakkında bilgi edinin](load-balancer-standard-overview.md)
