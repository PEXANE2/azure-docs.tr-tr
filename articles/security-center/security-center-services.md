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
ms.openlocfilehash: d756f9dfbd0012f884bb0c4a1e27efc76d613234
ms.sourcegitcommit: 387da88b8262368c1b67fffea58fe881308db1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71982832"
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
||**Sanal makine**|**Sanal makine ölçek kümesi**||**Sanal makine**|**Sanal makine ölçek kümesi**|
|Sanal makine davranış analizi tehdit algılama uyarıları|✔|✔|✔|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|Öneriler (ücretsiz) tehdit algılama (Standart)|
|Ağ tabanlı tehdit algılama uyarıları|✔|✔|X|✔|✔|X|Stand|
|Microsoft Defender ATP tümleştirmesi|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|X|X|X|Stand|
|Eksik düzeltme ekleri|✔|✔|✔|✔|✔|✔|Boş|
|Güvenlik Yapılandırması|✔|✔|✔|✔|✔|✔|Boş|
|Endpoint Protection değerlendirmesi|✔|✔|✔|X|X|X|Boş|
|Tam zamanında VM erişimi|✔|X|X|✔|X|X|Stand|
|Uyarlamalı uygulama denetimleri|✔|X|✔|✔|X|✔|Stand|
|Dosya bütünlüğünü Izleme|✔|✔|✔|✔|✔|✔|Stand|
|Disk şifrelemesi değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Üçüncü taraf dağıtımı|✔|X|X|✔|X|X|Boş|
|NSG değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Dosya daha az tehdit algılama|✔|✔|✔|X|X|X|Stand|
|Ağ eşlemesi|✔|✔|X|✔|✔|X|Stand|
|Uyarlamalı ağ sağlamlaştırma|✔|X|X|✔|X|X|Stand|
|Uyarlamalı ağ denetimleri|✔|✔|X|✔|✔|X|Stand|
|Mevzuat uyumluluk panosu & raporları|✔|✔|✔|✔|✔|✔|Stand|
|Docker tarafından barındırılan IaaS kapsayıcılarında öneriler ve tehdit algılama|X|X|X|✔|✔|✔|Stand|

### Desteklenen Endpoint Protection çözümleri<a name="endpoint-supported"></a>

Aşağıdaki tabloda bir matrisi verilmiştir:

 - Her çözümü sizin için yüklemek üzere Azure Güvenlik Merkezi 'ni kullanıp kullanmayacağınızı belirtir.
 - Güvenlik Merkezi 'nin hangi Endpoint Protection çözümlerini keşfedebileceği. Bu uç nokta koruma çözümlerinden biri bulunursa, güvenlik merkezi bir tane yüklemeyi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi için bkz. [Endpoint Protection değerlendirme ve öneriler](security-center-endpoint-protection.md).

| Endpoint Protection| Platformlar | Güvenlik Merkezi yüklemesi | Güvenlik Merkezi bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft kötü amaçlı yazılımdan koruma)| Windows Server 2016| Hayır, işletim sistemine yerleşik| Evet |
| System Center Endpoint Protection (Microsoft kötü amaçlı yazılımdan koruma) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı aracılığıyla | Evet |
| Trend Micro – tüm sürümler * | Windows Server ailesi  | Hayır | Evet |
| Symantec v 12.1.1100 +| Windows Server ailesi  | Hayır | Evet |
| McAfee ile v10 arasındaki + | Windows Server ailesi  | Hayır | Evet |
| McAfee ile v10 arasındaki + | Linux sunucu ailesi  | Hayır | Evet **\*** |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Evet **\***  |

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
|Depolama hesabı|✔| Yok|
|App Service|✔| ✔|
|İşlev|✔| X|
|Bulut hizmeti|✔| X|
|Adlı|✔| Yok|
|Alt ağ|✔| Yok|
|'I|✔| Yok|
|NSG|✔| Yok|
|Aboneliğiniz|✔ * *| ✔|
|Batch hesabı|✔| X|
|Service Fabric hesabı|✔| X|
|Otomasyon hesabı|✔| X|
|Yük dengeleyici|✔| X|
|Ara|✔| X|
|Service Bus ad alanı|✔| X|
|Stream Analytics|✔| X|
|Olay Hub 'ı ad alanı|✔| X|
|Logic Apps|✔| X|
|Redis|✔| Yok|
|Data Lake Analytics|✔| X|
|Data Lake Store|✔| X|
|Anahtar Kasası|✔| X|

\* Bu özellikler Şu anda genel önizlemede destekleniyor.

\* @ no__t-1 Azure Active Directory (Azure AD) önerileri yalnızca standart abonelikler için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin verileri ve Log Analytics aracısını nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- [Güvenlik Merkezi 'ni destekleyen platformları](security-center-os-coverage.md)gözden geçirin.
- [Azure Güvenlik Merkezi 'Nde VM 'ler & sunucular için tehdit algılama](security-center-alerts-iaas.md)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi 'ni kullanma hakkında sık sorulan soruları](security-center-faq.md)bulun.
- [Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini](https://blogs.msdn.com/b/azuresecurity/)bulun.
