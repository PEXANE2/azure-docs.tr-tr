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
ms.date: 03/01/2020
ms.author: memildin
ms.openlocfilehash: 9d3fa1e0b62ea6f4762c3df6ac7da310d5703807
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79245245"
---
# <a name="feature-coverage-for-machines"></a>Makineler için özellik kapsamı

Aşağıdaki tablolarda, sanal makineler ve sunucular için kullanılabilen Azure Güvenlik Merkezi özellikleri gösterilmektedir.

## Sanal makineler ve sunucular için desteklenen özellikler<a name="vm-server-features"></a>

### <a name="windows-machines"></a>[Windows makineleri](#tab/features-windows)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|**Özellik**|**Azure Sanal Makineler**|**Azure sanal makine ölçek kümeleri**|**Azure dışı makineler**|**Fiyatlandırma**
|[Microsoft Defender ATP tümleştirmesi](security-center-wdatp.md)|✔</br>(desteklenen sürümlerde)|✔</br>(desteklenen sürümlerde)|✔|Standart|
|[Sanal makine davranış analizi (ve güvenlik uyarıları)](threat-protection.md)|✔|✔|✔|Öneriler (ücretsiz) </br></br> Güvenlik uyarıları (Standart)|
|[Dosya daha az güvenlik uyarısı](alerts-reference.md#alerts-windows)|✔|✔|✔|Standart|
|[Ağ tabanlı güvenlik uyarıları](threat-protection.md#network-layer)|✔|✔|-|Standart|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Standart|
|[Yerel güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)|✔|-|-|Standart|
|[Dosya bütünlüğünü Izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standart|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Standart|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Standart|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|-|Standart|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Standart|
|Docker ile barındırılan IaaS kapsayıcıları üzerinde öneriler ve tehdit koruması|-|-|-|Standart|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Ücretsiz|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Ücretsiz|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Ücretsiz|
|Disk şifrelemesi değerlendirmesi|✔|✔|-|Ücretsiz|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Ücretsiz|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Ücretsiz|


### <a name="linux-machines"></a>[Linux makineleri](#tab/features-linux)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|**Özellik**|**Azure Sanal Makineler**|**Azure sanal makine ölçek kümeleri**|**Azure dışı makineler**|**Fiyatlandırma**
|[Microsoft Defender ATP tümleştirmesi](security-center-wdatp.md)|-|-|-|Standart|
|[Sanal makine davranış analizi (ve güvenlik uyarıları)](security-center-alerts-iaas.md)|✔</br>(desteklenen sürümlerde)|✔</br>(desteklenen sürümlerde)|✔|Öneriler (ücretsiz) </br></br> Güvenlik uyarıları (Standart)|
|[Dosya daha az güvenlik uyarısı](alerts-reference.md#alerts-windows)|-|-|-|Standart|
|[Ağ tabanlı güvenlik uyarıları](threat-protection.md#network-layer)|✔|✔|-|Standart|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Standart|
|[Yerel güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)|✔|-|-|Standart|
|[Dosya bütünlüğünü Izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standart|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Standart|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Standart|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|-|Standart|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Standart|
|Docker ile barındırılan IaaS kapsayıcıları üzerinde öneriler ve tehdit koruması|✔|✔|✔|Standart|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Ücretsiz|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Ücretsiz|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Ücretsiz|
|Disk şifrelemesi değerlendirmesi|✔|✔|-|Ücretsiz|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Ücretsiz|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Ücretsiz|

--- 


> [!TIP]
>Yalnızca standart fiyatlandırma katmanında bulunan özelliklerle denemeler yapmak için ücretsiz katman kullanıcıları 30 günlük bir denemeye kaydolabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).


## Desteklenen Endpoint Protection çözümleri<a name="endpoint-supported"></a>

Aşağıdaki tabloda bir matrisi verilmiştir:

 - Her çözümü sizin için yüklemek üzere Azure Güvenlik Merkezi 'ni kullanıp kullanmayacağınızı belirtir.
 - Güvenlik Merkezi 'nin hangi Endpoint Protection çözümlerini keşfedebileceği. Bu listeden bir uç nokta koruma çözümü bulunursa, güvenlik merkezi bir tane yüklemeyi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi için bkz. [Endpoint Protection değerlendirme ve öneriler](security-center-endpoint-protection.md).

| Uç Nokta Koruması| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)| Windows Server 2016| Hayır, işletim sisteminde yerleşik| Yes |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Yes |
| Trend Micro – tüm sürümler * | Windows Server Ailesi  | Hayır | Yes |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Yes |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Yes |
| McAfee v10+ | Linux sunucu ailesi  | Hayır | Evet **\*** |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Evet **\***  |

 **\*** Kapsam durumu ve destekleyici veriler şu anda yalnızca korunan aboneliklerinizle ilişkili olan Log Analytics çalışma alanında kullanılabilir. Azure Güvenlik Merkezi portalında yansıtılmaz.

> [!NOTE]
> - Windows Server 2008 R2 sanal makinesi üzerinde System Center Endpoint Protection (SCEP) algılama, PowerShell 3,0 (veya bir üst sürüm) sonrasında SCEP 'in yüklenmesini gerektirir.
> - Derinlemesine güvenlik aracıları için Trend mikro koruma algılama desteklenir.  OfficeScan aracıları desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin verileri ve Log Analytics aracısını nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- [Güvenlik Merkezi 'ni destekleyen platformları](security-center-os-coverage.md)gözden geçirin.
- [Azure Güvenlik Merkezi 'Nde Windows ve Linux makineleri için tehdit koruması](threat-protection.md#windows-machines)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi hakkında sık sorulan soruları](faq-general.md)bulun.