---
title: Azure sanal ağında genel IPv6 adres öneki
titlesuffix: Azure Virtual Network
description: Azure sanal ağında genel IPv6 adres öneki hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2019
ms.author: kumud
ms.openlocfilehash: 8254a7d86d5cadc2ddc03940f4ab2d08de74bd86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72965166"
---
# <a name="reserved-public-ipv6-address-prefix-preview"></a>Ayrılmış genel IPv6 adresi öneki (Önizleme)

Azure'da, çift yığın (IPv4+IPv6) sanal ağlar (VNet) ve sanal makineler (VM' ler) Internet bağlantılarına sahip olmadıkları ndan varsayılan olarak güvenlidir. Azure'dan elde ettiğiniz genel IPv6 adresleriyle Azure Yük Bakiyeleri ve VM'lerinize kolayca IPv6 Internet bağlantısı ekleyebilirsiniz.

Rezerve ettiğiniz tüm genel IP'ler, seçtiğiniz bir Azure bölgesi yle ve Azure aboneliğinizle ilişkilidir. Ayrılmış (statik) bir IPv6 genel IP'sini aboneliğinizdeki Azure Yük Dengeleyicileri veya VM'lerden herhangi biri arasında taşıyabilirsiniz. IPv6 genel IP'sini tamamen ayrıştırabilirsiniz ve hazır olduğunuzda kullanımınız için kullanılacaktır.

> [!WARNING]
> Genel IP adreslerinizi yanlışlıkla silmemek için dikkatli olun. Herkese açık bir IP'yi silerseniz aboneliğinizden kaldırır ve aboneliğinizi kurtaramazsınız (Azure desteği yardımıyla bile).

Tek tek IPv6 adreslerini ayırmanın yanı sıra, kullanımınız için bitişik Azure IPv6 adresleri (IP öneki olarak bilinir) aralıklarını ayırabilirsiniz.  Tek tek IP adreslerine benzer şekilde, ayrılmış önekler seçtiğiniz bir Azure bölgesiyle ve Azure aboneliğinizle ilişkilendirilir. Öngörülebilir, bitişik bir adres aralığının rezerve edileb'sinin birçok kullanımı vardır. Örneğin, statik IP aralıklarınız şirket içi güvenlik duvarlarına kolayca programlanabildiği için, Azure tarafından barındırılan uygulamalarınızın şirketiniz ve müşterileriniz tarafından IP *beyaz listesini* büyük ölçüde basitleştirebilirsiniz.  Gerektiğinde IP önekinizden tek tek genel IP'ler oluşturabilirsiniz ve bu *returned* bireysel Genel IP'leri sildiğinizde, bunları daha sonra yeniden kullanabilmeniz için ayrılmış aralığınıza döndürülürler. IP Önek'inizdeki tüm IP adresleri, önekinizi silene kadar özel kullanımınız için ayrılmıştır.

> [!Important]
> Azure Sanal Ağı için IPv6 şu anda genel önizlemededir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="ipv6-prefix-sizes"></a>IPv6 önek boyutları
Aşağıdaki genel IP öneki boyutları kullanılabilir:

-  Minimum IPv6 Önek boyutu: /127 = 2 adres
-  Maksimum IPv6 Önek boyutu: /124 = 16 adres

Önek boyutu Sınıfsız Etki Alanları Yönlendirme (CIDR) maske boyutu olarak belirtilir. Örneğin, /128 maskesi, IPv6 adresleri 128 bit'den oluştuğundan tek bir IPv6 adresini temsil eder.

## <a name="pricing"></a>Fiyatlandırma
 
Hem tek tek IP adresleri hem de IP aralıkları olan Azure Genel IP'leri kullanmaya ilişkin maliyetler için [Genel IP Adresi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/ip-addresses/)bakın.

## <a name="limitations"></a>Sınırlamalar
IPv6, Temel Genel IP'lerde yalnızca Uygulamanızı (VM'ler veya yük dengeleyicileri) Azure'da siler ve yeniden dağıttığınızda IPv6 adresinin değişeceği anlamına gelen "dinamik" ayırma yla desteklenir. Standart Internal yük dengeleyicileri de atandıkları alt ağ içinden dinamik ayırmayı destekleyebilir, ancak Standart IPv6 Public IP'nin desteği yalnızca statik (ayrılmış) tahsisidir.  

En iyi uygulama olarak, IPv6 uygulamalarınız için Standart Genel IP'leri ve Standart Yük Dengeleyicileri kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
- Genel bir [IPv6 adresi öneki](ipv6-reserve-public-ip-address-prefix.md)ayırın.
- [IPv6 adresleri](ipv6-overview.md)hakkında daha fazla bilgi edinin.
- Azure'da genel IP'leri (Hem IPv4 hem de IPv6) [nasıl oluşturup kullanacağınız](virtual-network-public-ip-address.md) hakkında bilgi edinin.
