---
title: Azure'da yüksek kullanılabilirlik bağlantı noktalarına genel bakış
titleSuffix: Azure Load Balancer
description: Bir dahili yük dengeleyicisi üzerinde yüksek kullanılabilirlik bağlantı noktaları yük dengeleme hakkında bilgi edinin.
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
ms.openlocfilehash: 5ada709350802344bfa65cce269735baa416edf6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234440"
---
# <a name="high-availability-ports-overview"></a>Yüksek kullanılabilirlik bağlantı noktalarına genel bakış

Azure Standart Yük Dengeleyicisi, dahili yük dengeleyicisi kullanırken tüm bağlantı noktalarında aynı anda TCP ve UDP akışlarını yüklemenize yardımcı olur. 

Yüksek kullanılabilirlik (HA) bağlantı noktaları yük dengeleme kuralı, dahili bir Standart Yük Dengeleyicisi üzerinde yapılandırılan bir yük dengeleme kuralının bir çeşididir. Dahili bir Standart Yük Dengeleyicisi'nin tüm bağlantı noktalarına gelen tüm TCP ve UDP akışlarını dengelemek için tek bir kural sağlayarak yük dengeleyicisinin kullanımını basitleştirebilirsiniz. Yük dengeleme kararı akış başına verilir. Bu eylem aşağıdaki beş tuple bağlantıdayanmaktadır: kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokol

HA bağlantı noktaları yük dengeleme kuralları, sanal ağlardaki ağ sanal cihazları (NV'ler) için yüksek kullanılabilirlik ve ölçek gibi kritik senaryolarda size yardımcı olur. Bu özellik, çok sayıda bağlantı noktasının yük dengeli olması gerektiğinde de yardımcı olabilir. 

Ön uç ve arka uç bağlantı noktalarını **0** ve protokol **tümünü**ayarladığınızda HA bağlantı noktaları yük dengeleme kuralları yapılandırılır. İç yük dengeleyici kaynağı, bağlantı noktası numarasından bağımsız olarak tüm TCP ve UDP akışlarını dengeler

## <a name="why-use-ha-ports"></a>Neden HA bağlantı noktalarını kullanıyorsun?

### <a name="network-virtual-appliances"></a><a name="nva"></a>Ağ sanal aletleri

Azure iş yükünüzü birden çok güvenlik tehdidinden korumak için NV'leri kullanabilirsiniz. Bu senaryolarda NV'leri kullandığınızda, bunlar güvenilir ve yüksek kullanılabilir olmalıdır ve talep için ölçeklendirmeleri gerekir.

Bu hedeflere yalnızca dahili yük bakiyecünüzün arka uç havuzuna NVA örnekleri ekleyerek ve bir HA bağlantı noktası yük dengeleyici kuralını yapılandırarak ulaşabilirsiniz.

NVA HA senaryoları için HA bağlantı noktaları aşağıdaki avantajları sunar:
- Her örnek başına sağlık sondasıyla sağlıklı örneklere hızlı bir şekilde başarısız olun
- *N-etkin*örneklere ölçeklendirme yle daha yüksek performans sağlayın
- *n*-etkin ve etkin-pasif senaryolar sağlama
- Cihazların izlenmesi için Apache ZooKeeper düğümleri gibi karmaşık çözümlere olan ihtiyacı ortadan kaldırın

Aşağıdaki diyagram, hub ve kollu sanal ağ dağıtımı sunar. Sözcüler, güvenilen alanı terk etmeden önce trafiği hub sanal ağına ve NVA üzerinden zorla tünel haline getirirler. NVA'lar, HA bağlantı noktaları yapılandırması olan dahili bir Standart Yük Dengeleyicisinin arkasındadır. Tüm trafik buna göre işlenebilir ve iletilebilir. Aşağıdaki diyagramda gösterilmiş olarak yapılandırıldığında, BIR HA Ports yük dengeleme kuralı ayrıca giriş ve çıkış trafiği için akış simetrisi sağlar.

<a node="diagram"></a>
![HA modunda dağıtılan NVA'lar ile hub ve kollu sanal ağ diyagramı](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> NV'ler kullanıyorsanız, sağlayıcılarıyla HA bağlantı noktalarını en iyi şekilde nasıl kullanacaklarını ve hangi senaryoların destekleniyi öğreneceklerini doğrulayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Çok sayıda bağlantı noktasının yük dengelemesi

Çok sayıda bağlantı noktasının yük dengelemesini gerektiren uygulamalar için HA bağlantı noktalarını da kullanabilirsiniz. HA bağlantı noktalarına sahip dahili bir [Standart Yük Dengeleyicisi](load-balancer-standard-overview.md) kullanarak bu senaryoları basitleştirebilirsiniz. Tek bir yük dengeleme kuralı, her bağlantı noktası için bir tane olmak üzere birden çok ayrı yük dengeleme kuralının yerini alır.

## <a name="region-availability"></a>Bölge kullanılabilirliği

HA bağlantı noktaları özelliği tüm genel Azure bölgelerinde kullanılabilir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Dahili bir Standart Yük Dengeleyicisi üzerinde tek, kayan olmayan IP (Doğrudan Sunucu İadesi olmayan) HA-port yapılandırması

Bu yapılandırma temel bir HA bağlantı noktası yapılandırmasıdır. Aşağıdakileri yaparak tek bir ön uç IP adresi üzerinde bir HA bağlantı noktası yük dengeleme kuralını yapılandırabilirsiniz:
1. Standart Yük Dengeleyicisini yapılandırırken, Yük Dengeleyici si konfigürasyonundaki **HA bağlantı noktalarını** onay kutusunu seçin.
2. **Kayan IP**için **Devre Dışı'yı**seçin.

Bu yapılandırma, geçerli yük dengeleyici kaynağında başka bir yük dengeleme kuralı yapılandırmasına izin vermez. Ayrıca, verilen arka uç örnekleri kümesi için başka bir iç yük dengeleyici kaynak yapılandırmasına izin verilmez.

Ancak, bu HA bağlantı noktaları kuralına ek olarak arka uç örnekleri için ortak bir Standart Yük Dengeleyicisi yapılandırabilirsiniz.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Dahili bir Standart Yük Dengeleyicisi üzerinde tek, kayan IP (Direct Server Return) HA-port yapılandırması

Benzer şekilde yük dengeleyicinizi, **Kayan IP'yi** **Etkin**olarak ayarlayarak tek bir ön uçla HA **Bağlantı Noktası** ile yük dengeleme kuralı kullanacak şekilde yapılandırabilirsiniz. 

Bu yapılandırmayı kullanarak, daha fazla kayan IP yük dengeleme kuralları ve/veya genel yük dengeleyicisi ekleyebilirsiniz. Ancak, bu yapılandırmanın üstünde kayan olmayan BIR IP, HA bağlantı noktaları yük dengeleme yapılandırması kullanamazsınız.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Dahili Standart Yük Dengeleyicisi üzerinde birden fazla HA-port yapılandırması

Senaryonuz aynı arka uç havuzu için birden fazla HA bağlantı noktası ön ucunu yapılandırmanızı gerektiriyorsa, aşağıdakileri yapabilirsiniz: 
- Tek bir dahili Standart Yük Dengeleyici kaynağı için birden fazla ön uç özel IP adresini yapılandırın.
- Her kuralın tek bir benzersiz ön uç IP adresinin seçildiği birden çok yük dengeleme kuralını yapılandırın.
- HA **bağlantı noktaları** seçeneğini seçin ve tüm yük dengeleme kuralları için **Kayan IP'yi** **Etkin** olarak ayarlayın.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>Aynı arka uç örneğinde HA bağlantı noktalarına sahip bir dahili yük dengeleyicisi ve genel yük dengeleyicisi

Ha bağlantı noktalarına sahip tek bir dahili Standart Yük Dengeleyicisi ile birlikte arka uç kaynakları için *bir* ortak Standart Yük Dengeleyici kaynağını yapılandırabilirsiniz.

>[!NOTE]
>Bu özellik şu anda Azure Kaynak Yöneticisi şablonları aracılığıyla kullanılabilir, ancak Azure portalı üzerinden kullanılamaz.

## <a name="limitations"></a>Sınırlamalar

- HA bağlantı noktaları yük dengeleme kuralları yalnızca dahili Standart Yük Dengeleyicisi için kullanılabilir.
- HA bağlantı noktaları yük dengeleme kuralı ile HA dışı bağlantı noktaları yük dengeleme kuralının birleştirilmesi desteklenmez.
- Varolan IP parçaları HA Ports yük dengeleme kuralları tarafından ilk paketle aynı hedefe iletilir.  BIR UDP veya TCP paketini parçalayan IP desteklenmez.
- Akış simetrisi (öncelikle NVA senaryoları için) arka uç örneği ve tek bir NIC (ve tek IP yapılandırması) ile desteklenir, yalnızca yukarıdaki diyagramda gösterildiği gibi ve HA Bağlantı Noktaları yük dengeleme kuralları kullanıldığında. Başka bir senaryoda sağlanmaz. Bu, iki veya daha fazla Yük Dengeleyici kaynağının ve ilgili kurallarının bağımsız kararlar verdiği ve hiçbir zaman koordine olmadığı anlamına gelir. [Ağ sanal cihazları](#nva)için açıklama ve diyagrama bakın. Birden fazla NIC kullanıyorsanız veya NVA'yı genel ve dahili Yük Dengeleyicisi arasında sandviç yaparken, akış simetrisi kullanılamaz.  Bu konuda, yanıtların aynı NVA'ya ulaşmasına izin vermek için cihazın IP'sine giriş akışını kaynak olarak çözebilirsiniz.  Ancak, tek bir NIC kullanmanızı ve yukarıdaki diyagramda gösterilen referans mimarisini kullanmanızı şiddetle öneririz.


## <a name="next-steps"></a>Sonraki adımlar

- [Ha bağlantı noktalarını dahili standart yük dengeleyicisi üzerinde yapılandırın](load-balancer-configure-ha-ports.md)
- [Standart Yük Dengeleyicisi hakkında bilgi edinin](load-balancer-standard-overview.md)
