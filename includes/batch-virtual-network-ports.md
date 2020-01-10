---
title: include dosyası
description: include dosyası
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 07/16/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 98f5269c27643e7ce6c0aaf9b359503a124d9232
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75663113"
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

**Ek ağ kaynakları** - Batch, sanal ağı içeren kaynak grubuna otomatik olarak ek ağ kaynakları atar. Her 50 adanmış düğüm (veya her 20 düşük öncelikli düğüm) için Batch şunları atar: 1 ağ güvenlik grubu (NSG), 1 genel IP adresi ve 1 yük dengeleyici. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../articles/azure-resource-manager/management/azure-subscription-service-limits.md) sınırlıdır. Büyük havuzlar için bu kaynaklardan birinde veya daha fazlasında kota artışı istemeniz gerekebilir.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

İşlem düğümlerinde görev zamanlayabilmek için alt ağın Batch hizmetinden gelen iletişim isteklerine, Azure Depolama veya diğer kaynaklarla iletişim kurabilmek için de giden iletişim isteklerine izin vermesi gerekir. Batch, Sanal Makine yapılandırmasındaki havuzlar için VM'lere ekli ağ arabirimleri (NIC) düzeyinde NSG'ler ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

* Batch hizmet rolü IP adreslerinden 29876 ve 29877 numaralı bağlantı noktalarına gelen TCP trafiği. 
* Uzaktan erişime izin vermek için 22 (Linux düğümleri) veya 3389 (Windows düğümler) numaralı bağlantı noktasından gelen TCP trafiği. Linux üzerinde bazı çok örnekli görevler (MPı gibi) için, Batch işlem düğümlerini içeren alt ağdaki IP 'Ler için SSH bağlantı noktası 22 trafiğe de izin vermeniz gerekir.
* Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.
* İnternete giden herhangi bir bağlantı noktasında giden trafik.

> [!IMPORTANT]
> Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar.

Batch kendi NSG'lerini yapılandırdığından alt ağ düzeyinde NSG belirtmenize gerek yoktur. Ancak belirtilen alt ağ ile ilişkilendirilmiş Ağ Güvenlik Grupları (NSG) ve/veya güvenlik duvarı varsa gelen ve giden güvenlik kurallarını aşağıdaki tablolarda gösterilen şekilde yapılandırın. 3389 (Windows) veya 22 (Linux) bağlantı noktasındaki gelen trafiği yalnızca dış kaynaklardan gelen havuz VM 'lerine uzaktan erişime izin vermeniz gerekiyorsa yapılandırın. Bu ayar havuz VM'lerinin kullanılabilir durumda olması için şart değildir. MPı gibi belirli çok örnekli görevleri kullanıyorsanız, Linux için 22 numaralı bağlantı noktasında sanal ağ alt ağ trafiğini etkinleştirmeniz gerektiğini unutmayın.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak hizmet etiketi | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Yok | `BatchNodeManagement` [hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) | * | Herhangi biri | 29876-29877 | TCP | Allow |
| Gerekirse, Linux çok örnekli görevler için işlem düğümlerine ve/veya işlem düğümü alt ağına uzaktan erişim için Kullanıcı kaynak IP 'Leri. | Yok | * | Herhangi biri | 3389 (Windows), 22 (Linux) | TCP | Allow |

**Giden güvenlik kuralları**

| Kaynak | Kaynak bağlantı noktaları | Hedef | Hedef hizmet etiketi | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Herhangi biri | * | [Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) | `Storage` (Batch hesabınızla ve VNet ile aynı bölgede) | 443 | TCP | Allow |

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

Havuz düğümlerine RDP erişimine izin vermeniz gerekiyorsa, Windows için bağlantı noktası 3389 üzerinde gelen trafiği yapılandırın. Bu ayar havuz düğümlerinin kullanılabilir durumda olması için şart değildir.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- |
Herhangi biri <br /><br />Bunun için "tümüne izin ver" izni gerekli olsa da Batch hizmeti her düğümün düzeyinde Batch harici IP adreslerini filtreleyen bir ACL kuralı uygular. | * | Herhangi biri | 10100, 20100, 30100 | TCP | Allow |
| İşlem düğümlerine RDP erişimine izin vermek için isteğe bağlı. | * | Herhangi biri | 3389 | TCP | Allow |

**Giden güvenlik kuralları**

| Kaynak | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- |
| Herhangi biri | * | Herhangi biri | 443  | Herhangi biri | Allow |
