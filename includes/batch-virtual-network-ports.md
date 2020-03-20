---
title: include dosyası
description: include dosyası
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 03/04/2020
ms.author: labrenne
ms.custom: include file
ms.openlocfilehash: e9460108499ca76d1b149b61cebe3d3081bf6544
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79086269"
---
### <a name="general-requirements"></a>Genel gereksinimler

* Sanal ağın havuzunuzu oluşturmak için kullandığınız Batch hesabıyla aynı abonelikte ve bölgede olması gerekir.

* Sanal ağı kullanan havuzda en fazla 4096 düğüm bulunabilir.

* Havuz için belirtilen alt ağda havuz için hedeflenen VM sayısına yetecek kadar atanmamış IP adresi bulunması gerekir. Başka bir deyişle bu değerin havuzun `targetDedicatedNodes` ve `targetLowPriorityNodes` özelliklerinin toplamı olması gerekir. Alt ağda yeterli sayıda atanmamış IP adresi yoksa havuz işlem düğümlerini kısmen ayırır ve bir yeniden boyutlandırma hatası oluşur.

* Azure Depolama uç noktanızın sanal ağınızda kullanılan özel DNS sunucuları tarafından çözümlenebilmesi gerekir. Özellikle `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` ve `<account>.blob.core.windows.net` biçimindeki URL'ler çözümlenebilir.

Batch havuzunun Sanal Makine yapılandırmasında veya Cloud Services yapılandırmasında olma durumunda göre ek sanal ağ gereksinimleri farklı olabilir. Sanal ağa yapılacak yeni havuz dağıtımları için Sanal Makine yapılandırmasının kullanılması önerilir.

### <a name="pools-in-the-virtual-machine-configuration"></a>Sanal Makine yapılandırmasındaki havuzlar

**Desteklenen ağlar** - Yalnızca Azure Resource Manager tabanlı sanal ağlar

**Alt ağ kimliği** - Alt ağı Batch API'leri ile belirtirken alt ağın *kaynak tanımlayıcısını* kullanın. Alt ağ tanımlayıcısı şu biçimdedir:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**İzinler** - Sanal ağ aboneliği veya kaynak grubu güvenlik ilkelerinin veya kilitlerinin belirli bir kullanıcının sanal ağ yönetim izinlerini kısıtlayıp kısıtlamadığını kontrol edin.

**Ek ağ kaynakları** - Batch, sanal ağı içeren kaynak grubuna otomatik olarak ek ağ kaynakları atar.

> [!IMPORTANT]
>Batch, her 50 adanmış düğüm (veya her 20 düşük öncelikli düğüm) için şunları atar: bir ağ güvenlik grubu (NSG), bir genel IP adresi ve bir yük dengeleyici. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../articles/azure-resource-manager/management/azure-subscription-service-limits.md) sınırlıdır. Büyük havuzlar için bu kaynaklardan birinde veya daha fazlasında kota artışı istemeniz gerekebilir.

#### <a name="network-security-groups-batch-default"></a>Ağ güvenlik grupları: Batch varsayılanı

Alt ağın işlem düğümlerinde görev zamanlayabilmek için Batch hizmetinden gelen iletişim isteklerine, Azure Depolama veya diğer kaynaklarla iletişim kurabilmek için de giden iletişim isteklerine iş yükünüzün gerektirdiği gibi izin vermesi gerekir. Batch, Sanal Makine yapılandırmasındaki havuzlar için VM'lere ekli ağ arabirimleri (NIC) düzeyinde NSG'ler ekler. Bu NSG’ler aşağıdaki ek kurallar ile yapılandırılır:

* `BatchNodeManagement` hizmet etiketine karşılık gelen Batch hizmeti IP adreslerinden 29876 ve 29877 numaralı bağlantı noktalarına gelen TCP trafiği.
* Uzaktan erişime izin vermek için 22 (Linux düğümleri) veya 3389 (Windows düğümler) numaralı bağlantı noktasından gelen TCP trafiği. Linux üzerindeki belirli türde çok örnekli görevler (MPI gibi) için Batch işlem düğümlerini içeren alt ağdaki IP’ler için SSH bağlantı noktası 22 trafiğine de izin vermeniz gerekir. Bu, alt ağ düzeyindeki NSG kurallarına göre engellenebilir (aşağıya bakın).
* Sanal ağa giden herhangi bir bağlantı noktasında giden trafik. Bu, alt ağ düzeyindeki NSG kurallarına göre değiştirilebilir (aşağıya bakın).
* İnternete giden herhangi bir bağlantı noktasında giden trafik. Bu, alt ağ düzeyindeki NSG kurallarına göre değiştirilebilir (aşağıya bakın).

> [!IMPORTANT]
> Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar. Buna ek olarak, Batch tarafından eklenen tüm kaynaklara kaynak kilidi uygulanmaması gerekir. Aksi takdirde, bu işlem bir havuzun silinmesi gibi kullanıcı tarafından başlatılan eylemlerin sonucu olarak kaynakların temizlenmesine sebep olabilir.

#### <a name="network-security-groups-specifying-subnet-level-rules"></a>Ağ güvenlik grupları: Alt ağ düzeyi kuralları belirtme

Batch kendi NSG’lerini yapılandırdığı için sanal ağ düzeyinde NSG belirtmeniz gerekmez (yukarıya bakın). Batch işlem düğümlerinin dağıtıldığı bir yerdeki alt ağ ile ilişkilendirilmiş bir NSG’niz varsa veya uygulanan varsayılanları geçersiz kılmak için özel NSG kuralları uygulamak istiyorsanız, bu NSG’yi aşağıdaki tablolarda gösterildiği gibi en azından gelen ve giden güvenlik kurallarıyla yapılandırmanız gerekir.

3389 (Windows) veya 22 (Linux) numaralı bağlantı noktalarına gelen trafiği yalnızca dış kaynaklardaki işlem düğümlerine uzaktan erişim izni vermeniz gerekiyorsa yapılandırın. Belirli MPI çalışma zamanlarıyla çoklu örnek görevleri için destek istiyorsanız Linux’ta 22 numaralı bağlantı noktası kurallarını etkinleştirmeniz gerekir. Havuz işlem düğümlerinin kullanılabilir olması için bu bağlantı noktalarındaki trafiğe kesinlikle izin verilmesi gerekmez.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak hizmeti etiketi | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Yok | `BatchNodeManagement` [Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) (bölgesel varyant kullanılıyorsa Batch hesabınızla aynı bölgede) | * | Herhangi biri | 29876-29877 | TCP | İzin Ver |
| Gerekirse Linux çok örnekli görevler için işlem düğümlerine ve/veya işlem düğümü alt ağlarına uzaktan erişim için kullanıcı kaynağı IP’leri. | Yok | * | Herhangi biri | 3389 (Windows), 22 (Linux) | TCP | İzin Ver |

**Giden güvenlik kuralları**

| Kaynak | Kaynak bağlantı noktaları | Hedef | Hedef hizmet etiketi | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Herhangi biri | * | [Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) | `Storage` (bölgesel varyant kullanılıyorsa Batch hesabınızla aynı bölgede) | 443 | TCP | İzin Ver |

### <a name="pools-in-the-cloud-services-configuration"></a>Bulut Hizmetleri yapılandırmasındaki havuzlar

**Desteklenen sanal ağlar** - Yalnızca klasik sanal ağlar

**Alt ağ kimliği** - Alt ağı Batch API'leri ile belirtirken alt ağın *kaynak tanımlayıcısını* kullanın. Alt ağ tanımlayıcısı şu biçimdedir:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**İzinler** - `Microsoft Azure Batch` hizmet sorumlusu, belirtilen sanal ağ için `Classic Virtual Machine Contributor` Rol Tabanlı Erişim Denetimi (RBAC) rolüne sahip olmalıdır.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

İşlem düğümlerinde görev zamanlayabilmek için alt ağın Batch hizmetinden gelen iletişim isteklerine, Azure Depolama veya diğer kaynaklarla iletişim kurabilmek için de giden iletişim isteklerine izin vermesi gerekir.

Batch iletişimi yalnızca Batch IP adreslerinden havuz düğümlerine gelen iletişime izin verecek şekilde yapılandırdığından NSG belirtmenize gerek yoktur. Ancak belirtilen alt ağ ile ilişkilendirilmiş NSG'ler ve/veya güvenlik duvarı varsa gelen ve giden güvenlik kurallarını aşağıdaki tablolarda gösterilen şekilde yapılandırın. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar.

Windows için 3389 numaralı bağlantı noktalarına gelen trafiği yalnızca havuzdaki düğümlere RDP erişim izni vermeniz gerekiyorsa yapılandırın. Bu ayar havuz düğümlerinin kullanılabilir durumda olması için şart değildir.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- |
Herhangi biri <br /><br />Bunun için "tümüne izin ver" izni gerekli olsa da Batch hizmeti her düğümün düzeyinde Batch harici IP adreslerini filtreleyen bir ACL kuralı uygular. | * | Herhangi biri | 10100, 20100, 30100 | TCP | İzin Ver |
| İşlem düğümlerine RDP erişimi izni vermek için isteğe bağlı. | * | Herhangi biri | 3389 | TCP | İzin Ver |

**Giden güvenlik kuralları**

| Kaynak | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- |
| Herhangi biri | * | Herhangi biri | 443  | Herhangi biri | İzin Ver |
