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
ms.date: 04/03/2020
ms.author: labrenne
ms.custom: include file
ms.openlocfilehash: dc08dcded6418208751edbffcb5d263db059ec01
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657495"
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
>Her 50 özel düğüm (veya her 20 düşük öncelikli düğüm) için toplu iş ayırmalar: bir ağ güvenlik grubu (NSG), bir genel IP adresi ve bir yük dengeleyicisi. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../articles/azure-resource-manager/management/azure-subscription-service-limits.md) sınırlıdır. Büyük havuzlar için, bu kaynaklardan biri veya birkaçı için kota artışı istemeniz gerekebilir.

#### <a name="network-security-groups-batch-default"></a>Ağ güvenlik grupları: Toplu işlem varsayılan

Alt ağ, Iş yükünüzün gerektirdiği şekilde Azure Depolama veya diğer kaynaklarla iletişim kurmak için Toplu İşlem hizmetinden gelen iletişimin, bilgi işlem düğümlerinde görevleri zamanlayabilmesine ve giden iletişimin de Azure Depolama ile iletişim kurmasına izin vermelidir. Sanal Makine yapılandırmasındaki havuzlar için Toplu, bilgi işlem düğümlerine bağlı ağ arabirimleri (NIC) düzeyine NSG ekler. Bu NSG'ler aşağıdaki ek kurallarla yapılandırılır:

* `BatchNodeManagement` Servis etiketine karşılık gelen Toplu servis IP adreslerinden 29876 ve 29877 bağlantı noktalarındaki gelen TCP trafiği.
* Uzaktan erişime izin vermek için 22 (Linux düğümleri) veya 3389 (Windows düğümler) numaralı bağlantı noktasından gelen TCP trafiği. Linux'taki belirli çok örnekli görev türleri için (MPI gibi), Toplu işlem düğümlerini içeren alt ağdaki IP'ler için SSH bağlantı noktası 22 trafiğine de izin vermeniz gerekir. Bu, alt net düzeyindeki NSG kurallarına göre engellenebilir (aşağıya bakın).
* Sanal ağa giden herhangi bir bağlantı noktasında giden trafik. Bu, alt net düzeyindeki NSG kurallarına göre değiştirilebilir (aşağıya bakın).
* Internet'e giden herhangi bir bağlantı noktasında giden trafik. Bu, alt net düzeyindeki NSG kurallarına göre değiştirilebilir (aşağıya bakın).

> [!IMPORTANT]
> Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar. Ayrıca, Toplu İşlem tarafından oluşturulan herhangi bir kaynağa kaynak kilitleri uygulanmamalıdır, aksi takdirde bu, kullanıcı tarafından başlatılan bir havuz silme gibi eylemler sonucunda kaynakların temizlenmesini önlemeye neden olabilir.

#### <a name="network-security-groups-specifying-subnet-level-rules"></a>Ağ güvenlik grupları: Alt ağ düzeyindeki kuralları belirtme

Toplu iş kendi NSG'lerini yapılandırdığından sanal ağ alt net düzeyinde NSG belirtmek gerekmez (yukarıya bakın). Toplu işlem düğümlerinin dağıtıldığı alt ağla ilişkili bir NSG'niz varsa veya uygulanan varsayılanları geçersiz kılmak için özel NSG kuralları uygulamak istiyorsanız, bu NSG'yi aşağıdaki tablolarda gösterildiği gibi en az gelen ve giden güvenlik kurallarıyla yapılandırmanız gerekir.

Yalnızca dış kaynaklardan gelen bilgi işlem düğümlerine uzaktan erişime izin vermek zorundaysanız, gelen trafiği 3389 (Windows) veya 22 (Linux) bağlantı noktasında yapılandırın. Belirli MPI çalışma sürelerine sahip çok örnekli görevler için destek gerekiyorsa, Linux'taki bağlantı noktası 22 kurallarını etkinleştirmeniz gerekebilir. Bu bağlantı noktalarındaki trafiğin havuz hesaplama düğümlerinin kullanılabilir olması için kesinlikle gerekli değildir.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak servis etiketi | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Yok | `BatchNodeManagement`[Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) (bölgesel varyant kullanıyorsanız, Toplu Iş hesabınızla aynı bölgede) | * | Herhangi biri | 29876-29877 | TCP | İzin Ver |
| Gerekirse Linux çok örnekli görevler için bilgi işlem düğümlerine ve/veya bilgi işlem düğümü alt ağına uzaktan erişmek için kullanıcı kaynağı IP'leri. | Yok | * | Herhangi biri | 3389 (Windows), 22 (Linux) | TCP | İzin Ver |

> [!WARNING]
> Toplu servis IP adresleri zaman içinde değişebilir. Bu nedenle, NSG kuralları `BatchNodeManagement` için hizmet etiketini (veya bölgesel varyantı) kullanmanız önerilir. NSG kurallarını toplu servis IP adresleriyle doğrudan doldurmak önerilmez.

**Giden güvenlik kuralları**

| Kaynak | Kaynak bağlantı noktaları | Hedef | Hedef hizmet etiketi | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Herhangi biri | * | [Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) | `Storage`(bölgesel varyant kullanıyorsanız, Toplu İş hesabınızla aynı bölgede) | 443 | TCP | İzin Ver |

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

RDP'nin havuz düğümlerine erişmelerine izin vermeniz gerekiyorsa, Windows için 3389 numaralı bağlantı noktasındagelen trafiği yapılandırın. Bu ayar havuz düğümlerinin kullanılabilir durumda olması için şart değildir.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- |
Herhangi biri <br /><br />Bunun için "tümüne izin ver" izni gerekli olsa da Batch hizmeti her düğümün düzeyinde Batch harici IP adreslerini filtreleyen bir ACL kuralı uygular. | * | Herhangi biri | 10100, 20100, 30100 | TCP | İzin Ver |
| İsteğe bağlı, RDP'nin bilgi işlem düğümlerine erişmesine izin vermek için. | * | Herhangi biri | 3389 | TCP | İzin Ver |

**Giden güvenlik kuralları**

| Kaynak | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protokol | Eylem |
| --- | --- | --- | --- | --- | --- |
| Herhangi biri | * | Herhangi biri | 443  | Herhangi biri | İzin Ver |
