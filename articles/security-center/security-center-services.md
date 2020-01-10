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
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 7ffab200b50a7c4d1af60a230f237b18e8afc621
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754217"
---
# <a name="supported-features-available-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde sunulan desteklenen özellikler

> [!NOTE]
>Bazı özellikler yalnızca Standart katmanda kullanılabilir. Güvenlik Merkezi 'nin standart katmanına kaydolmadıysanız ücretsiz bir deneme süresi vardır. Daha fazla bilgi için [Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.

Aşağıdaki bölümlerde, [desteklenen platformları](security-center-os-coverage.md)Için kullanılabilen güvenlik merkezi özellikleri gösterilmektedir.

* [Sanal makineler/sunucular](#vm-server-features)
* [PaaS hizmetleri](#paas-services)


## Sanal makine/sunucu tarafından desteklenen özellikler<a name="vm-server-features"></a>

### <a name="windowstabfeatures-windows"></a>[Windows](#tab/features-windows)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
||**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure dışı makineler**|**Fiyatlandırma**
|[Microsoft Defender ATP tümleştirmesi](security-center-wdatp.md)|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|Standart|
|[Sanal makine davranış analizi tehdit algılama uyarıları](security-center-alerts-iaas.md)|✔|✔|✔|Öneriler (ücretsiz) tehdit algılama (Standart)|
|[Dosya daha az tehdit algılama uyarıları](alerts-reference.md#alerts-windows)|✔|✔|✔|Standart|
|[Ağ tabanlı tehdit algılama uyarıları](security-center-alerts-service-layer.md#azure-network-layer)|✔|✔|-|Standart|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Standart|
|[Yerel güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)|✔|-|-|Standart|
|[Dosya bütünlüğünü Izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standart|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Standart|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Standart|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|-|Standart|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Standart|
|Docker tarafından barındırılan IaaS kapsayıcılarında öneriler ve tehdit algılama|-|-|-|Standart|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Ücretsiz|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Ücretsiz|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Ücretsiz|
|Disk şifrelemesi değerlendirmesi|✔|✔|-|Ücretsiz|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Ücretsiz|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Ücretsiz|


### <a name="linuxtabfeatures-linux"></a>[Linux](#tab/features-linux)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
||**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure dışı makineler**|**Fiyatlandırma**
|[Microsoft Defender ATP tümleştirmesi](security-center-wdatp.md)|-|-|-|Standart|
|[Sanal makine davranış analizi tehdit algılama uyarıları](security-center-alerts-iaas.md)|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|Öneriler (ücretsiz) tehdit algılama (Standart)|
|[Dosya daha az tehdit algılama uyarıları](alerts-reference.md#alerts-windows)|-|-|-|Standart|
|[Ağ tabanlı tehdit algılama uyarıları](security-center-alerts-service-layer.md#azure-network-layer)|✔|✔|-|Standart|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Standart|
|[Yerel güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)|✔|-|-|Standart|
|[Dosya bütünlüğünü Izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standart|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Standart|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Standart|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|-|Standart|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Standart|
|Docker tarafından barındırılan IaaS kapsayıcılarında öneriler ve tehdit algılama|✔|✔|✔|Standart|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Ücretsiz|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Ücretsiz|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Ücretsiz|
|Disk şifrelemesi değerlendirmesi|✔|✔|-|Ücretsiz|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Ücretsiz|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Ücretsiz|

--- 

## Desteklenen Endpoint Protection çözümleri<a name="endpoint-supported"></a>

Aşağıdaki tabloda bir matrisi verilmiştir:

 - Her çözümü sizin için yüklemek üzere Azure Güvenlik Merkezi 'ni kullanıp kullanmayacağınızı belirtir.
 - Güvenlik Merkezi 'nin hangi Endpoint Protection çözümlerini keşfedebileceği. Bu listeden bir uç nokta koruma çözümü bulunursa, güvenlik merkezi bir tane yüklemeyi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi için bkz. [Endpoint Protection değerlendirme ve öneriler](security-center-endpoint-protection.md).

| Endpoint Protection| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)| Windows Server 2016| Hayır, işletim sisteminde yerleşik| Evet |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Evet |
| Trend Micro – tüm sürümler * | Windows Server Ailesi  | Hayır | Evet |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Linux sunucu ailesi  | Hayır | Evet **\*** |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Evet **\***  |

 **\*** Kapsam durumu ve destekleyici veriler şu anda yalnızca korunan aboneliklerinizle ilişkili olan Log Analytics çalışma alanında kullanılabilir. Azure Güvenlik Merkezi portalında yansıtılmaz.

> [!NOTE]
> - Windows Server 2008 R2 sanal makinesi üzerinde System Center Endpoint Protection (SCEP) algılama, PowerShell 3,0 (veya bir üst sürüm) sonrasında SCEP 'in yüklenmesini gerektirir.
> - Derinlemesine güvenlik aracıları için Trend mikro koruma algılama desteklenir.  OfficeScan aracıları desteklenmez.


## PaaS hizmetleri desteklenen özellikler <a name="paas-services"></a>

Aşağıdaki PaaS kaynakları Azure Güvenlik Merkezi tarafından desteklenir:

|Hizmet|Öneriler (ücretsiz)|Tehdit algılama uyarıları (Standart)|Güvenlik açığı değerlendirmesi (Standart)|
|----|:----:|:----:|:----:|
|SQL Veritabanları|✔|✔|✔|
|Azure Container Registry|-|-|✔|
|Azure Kubernetes Service|✔|✔|-|
|PostgreSQL için Azure veritabanı *|✔|✔|-|
|MySQL için Azure veritabanı *|✔|✔|-|
|Azure CosmosDB *|-|✔|-|
|Depolama Hesapları|✔|-|-|
|Blob Depolama|✔|✔|-|
|App Service|✔|✔|-|
|İşlev uygulaması|✔|-|-|
|Bulut Hizmetleri|✔|-|-|
|Sanal Ağ|✔|-|-|
|Alt ağ|✔|-|-|
|NIC|✔|-|-|
|Ağ Güvenlik Grupları|✔|-|-|
|Abonelik|✔ * *|✔|-|
|Batch hesabı|✔|-|-|
|Service Fabric hesabı|✔|-|-|
|Otomasyon hesabı|✔|-|-|
|Yük Dengeleyici|✔|-|-|
|Bilişsel Arama|✔|-|-|
|Service Bus ad alanı|✔|-|-|
|Stream Analytics|✔|-|-|
|Olay hub’ı ad alanı|✔|-|-|
|Mantıksal uygulamalar|✔|-|-|
|Redsıs için önbellek|✔|-|-|
|Data Lake Analytics|✔|-|-|
|Azure Data Lake Storage|✔|-|-|
|Key Vault|✔|✔ *|-|

\* bu özellikler Şu anda önizlemede destekleniyor.

\*\* Azure Active Directory (Azure AD) önerileri yalnızca standart abonelikler için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin verileri ve Log Analytics aracısını nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- [Güvenlik Merkezi 'ni destekleyen platformları](security-center-os-coverage.md)gözden geçirin.
- [Azure Güvenlik Merkezi 'Nde VM 'ler & sunucular için tehdit algılama](security-center-alerts-iaas.md)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi 'ni kullanma hakkında sık sorulan soruları](security-center-faq.md)bulun.
- [Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini](https://blogs.msdn.com/b/azuresecurity/)bulun.