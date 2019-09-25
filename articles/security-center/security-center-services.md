---
title: Azure Güvenlik Merkezi 'nde sunulan desteklenen özellikler | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi tarafından desteklenen hizmetlerin bir listesini sağlar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 870ebc8d-1fad-435b-9bf9-c477f472ab17
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: memildin
ms.openlocfilehash: ad662cdeb0a87e57eb9e3e7480d120be1c6c729e
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71218268"
---
# <a name="supported-features-available-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde sunulan desteklenen özellikler

> [!NOTE]
>Bazı özellikler yalnızca Standart katmanda kullanılabilir. Güvenlik Merkezi 'nin standart katmanına kaydolmadıysanız ücretsiz bir deneme süresi vardır. Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/) bakın.

Aşağıdaki bölümlerde, [desteklenen platformları](security-center-os-coverage.md)Için kullanılabilen güvenlik merkezi özellikleri gösterilmektedir.

* [Sanal makineler/sunucular](#vm-server-features)
* [PaaS hizmetleri](#paas-services)


## Sanal makine/sunucu tarafından desteklenen özellikler<a name="vm-server-features"></a>

> [!div class="mx-tableFixed"]

|Sunucusu|Windows|||Linux|||Fiyatlandırma|
|----|----|----|----|----|----|----|----|
|**Ortam**|**Azure**||**Azure dışı**|**Azure**||**Azure dışı**||
||**Sanal Makine**|**Sanal makine ölçek kümesi**||**Sanal Makine**|**Sanal makine ölçek kümesi**|
|VMBA tehdit algılama uyarıları|✔|✔|✔|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|Öneriler (ücretsiz) tehdit algılama (Standart)|
|Ağ tabanlı tehdit algılama uyarıları|✔|✔|X|✔|✔|X|Standart|
|Microsoft Defender ATP tümleştirmesi|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|X|X|X|Standart|
|Eksik düzeltme ekleri|✔|✔|✔|✔|✔|✔|Boş|
|Güvenlik yapılandırmaları|✔|✔|✔|✔|✔|✔|Boş|
|Endpoint Protection değerlendirmesi|✔|✔|✔|X|X|X|Boş|
|JIT VM erişimi|✔|X|X|✔|X|X|Standart|
|Uyarlamalı uygulama denetimleri|✔|X|✔|✔|X|✔|Standart|
|FIM|✔|✔|✔|✔|✔|✔|Standart|
|Disk şifrelemesi değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Üçüncü taraf dağıtımı|✔|X|X|✔|X|X|Boş|
|NSG değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Dosya daha az tehdit algılama|✔|✔|✔|X|X|X|Standart|
|Ağ haritası|✔|✔|X|✔|✔|X|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|X|✔|✔|X|Standart|
|Mevzuat uyumluluk panosu & raporları|✔|✔|✔|✔|✔|✔|Standart|
|Docker tarafından barındırılan IaaS kapsayıcılarında öneriler ve tehdit algılama|X|X|X|✔|✔|✔|Standart|

### Desteklenen Endpoint Protection çözümleri<a name="endpoint-supported"></a>

Aşağıdaki tabloda bir matrisi verilmiştir:

 - Her çözümü sizin için yüklemek üzere Azure Güvenlik Merkezi 'ni kullanıp kullanmayacağınızı belirtir.
 - Güvenlik Merkezi 'nin hangi Endpoint Protection çözümlerini keşfedebileceği. Bu uç nokta koruma çözümlerinden biri bulunursa, güvenlik merkezi bir tane yüklemeyi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi için bkz. [Endpoint Protection değerlendirme ve öneriler](security-center-endpoint-protection.md).

| Endpoint Protection| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)| Windows Server 2016| Hayır, işletim sisteminde yerleşik| Evet |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Evet |
| Trend Micro – tüm sürümler * | Windows Server Ailesi  | Hayır | Evet |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Linux sunucu ailesi  | Hayır | Yes **\*** |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Yes **\***  |

 **\*** Kapsam durumu ve destekleyici veriler şu anda yalnızca korunan aboneliklerinizle ilişkili Log Analytics çalışma alanında kullanılabilir ve Azure Güvenlik Merkezi portalında yansıtılmamaktadır.

> [!NOTE]
>
> - Windows Server 2008 R2 sanal makinesi üzerinde System Center Endpoint Protection (SCEP) algılama, PowerShell 3,0 (veya bir üst sürüm) sonrasında SCEP 'in yüklenmesini gerektirir.
> - Derinlemesine güvenlik aracıları için Trend mikro koruma algılama desteklenir.  OfficeScan aracıları desteklenmez.


## PaaS hizmetleri desteklenen özellikler <a name="paas-services"></a>

Aşağıdaki PaaS kaynakları Azure Güvenlik Merkezi tarafından desteklenir:

|Hizmet|Öneriler (ücretsiz)|Tehdit algılama (Standart)|
|----|----|----|
|SQL|✔| ✔|
|PostgreSQL|✔| ✔|
|MySQL|✔| ✔|
|CosmosDB|X| ✔|
|Blob depolama|✔| ✔|
|Depolama hesabı|✔| NA|
|App Service|✔| ✔|
|İşlev|✔| X|
|Bulut Hizmeti|✔| X|
|Sanal ağ|✔| NA|
|Subnet|✔| NA|
|NIC|✔| NA|
|NSG|✔| NA|
|Subscription|✔ * *| ✔|
|Batch hesabı|✔| X|
|Service Fabric hesabı|✔| X|
|Otomasyon hesabı|✔| X|
|Yük dengeleyici|✔| X|
|Ara|✔| X|
|Service bus ad alanı|✔| X|
|Stream Analytics|✔| X|
|Olay hub’ı ad alanı|✔| X|
|Logic Apps|✔| X|
|Redis|✔| NA|
|Data Lake Analytics|✔| X|
|Data Lake Store|✔| X|
|Anahtar kasası|✔| X|

\*Bu özellikler Şu anda genel önizlemede desteklenmektedir.

\*\*Azure Active Directory (Azure AD) önerileri yalnızca standart abonelikler için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin verileri ve Log Analytics aracısını nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- [Güvenlik Merkezi 'ni destekleyen platformları](security-center-os-coverage.md)gözden geçirin.
- [Azure Güvenlik Merkezi 'Nde VM 'ler & sunucular için tehdit algılama](security-center-alerts-iaas.md)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi 'ni kullanma hakkında sık sorulan soruları](security-center-faq.md)bulun.
- [Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini](https://blogs.msdn.com/b/azuresecurity/)bulun.
