---
title: Azure Güvenlik Merkezi'nde desteklenen özellikler | Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245245"
---
# <a name="feature-coverage-for-machines"></a>Makineler için özellik kapsamı

Aşağıdaki tablolarda sanal makineler ve sunucular için kullanılabilen Azure Güvenlik Merkezi özellikleri gösterilebilir.

## <a name="supported-features-for-virtual-machines-and-servers"></a>Sanal makineler ve sunucular için desteklenen özellikler<a name="vm-server-features"></a>

### <a name="windows-machines"></a>[Windows makineleri](#tab/features-windows)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|**Özellik**|**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure Olmayan Makineler**|**Fiyatlandırma**
|[Microsoft Defender ATP entegrasyonu](security-center-wdatp.md)|✔</br>(desteklenen sürümler)|✔</br>(desteklenen sürümler)|✔|Standart|
|[Sanal Makine Davranış Analizi (ve güvenlik uyarıları)](threat-protection.md)|✔|✔|✔|Öneriler (Ücretsiz) </br></br> Güvenlik uyarıları (Standart)|
|[Dosyasız güvenlik uyarıları](alerts-reference.md#alerts-windows)|✔|✔|✔|Standart|
|[Ağ tabanlı güvenlik uyarıları](threat-protection.md#network-layer)|✔|✔|-|Standart|
|[Tam Zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Standart|
|[Yerel güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)|✔|-|-|Standart|
|[Dosya Bütünlüğü İzleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standart|
|[Uyarlanabilir uygulama kontrolleri](security-center-adaptive-application.md)|✔|-|✔|Standart|
|[Ağ haritası](security-center-network-recommendations.md#network-map)|✔|✔|-|Standart|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Standart|
|Uyarlanabilir ağ denetimleri|✔|✔|-|Standart|
|[Mevzuata Uygunluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Standart|
|Docker barındırılan IaaS konteynerlerinde öneriler ve tehdit koruması|-|-|-|Standart|
|Eksik işletim sistemi yamaları değerlendirmesi|✔|✔|✔|Ücretsiz|
|Güvenlik yanlış yapılandırmaları değerlendirmesi|✔|✔|✔|Ücretsiz|
|[Uç nokta koruma değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Ücretsiz|
|Disk şifreleme değerlendirmesi|✔|✔|-|Ücretsiz|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Ücretsiz|
|[Ağ güvenliği değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Ücretsiz|


### <a name="linux-machines"></a>[Linux makineleri](#tab/features-linux)

|||||||||
|----|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|**Özellik**|**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure Olmayan Makineler**|**Fiyatlandırma**
|[Microsoft Defender ATP entegrasyonu](security-center-wdatp.md)|-|-|-|Standart|
|[Sanal Makine Davranış Analizi (ve güvenlik uyarıları)](security-center-alerts-iaas.md)|✔</br>(desteklenen sürümler)|✔</br>(desteklenen sürümler)|✔|Öneriler (Ücretsiz) </br></br> Güvenlik uyarıları (Standart)|
|[Dosyasız güvenlik uyarıları](alerts-reference.md#alerts-windows)|-|-|-|Standart|
|[Ağ tabanlı güvenlik uyarıları](threat-protection.md#network-layer)|✔|✔|-|Standart|
|[Tam Zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Standart|
|[Yerel güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)|✔|-|-|Standart|
|[Dosya Bütünlüğü İzleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Standart|
|[Uyarlanabilir uygulama kontrolleri](security-center-adaptive-application.md)|✔|-|✔|Standart|
|[Ağ haritası](security-center-network-recommendations.md#network-map)|✔|✔|-|Standart|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Standart|
|Uyarlanabilir ağ denetimleri|✔|✔|-|Standart|
|[Mevzuata Uygunluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Standart|
|Docker barındırılan IaaS konteynerlerinde öneriler ve tehdit koruması|✔|✔|✔|Standart|
|Eksik işletim sistemi yamaları değerlendirmesi|✔|✔|✔|Ücretsiz|
|Güvenlik yanlış yapılandırmaları değerlendirmesi|✔|✔|✔|Ücretsiz|
|[Uç nokta koruma değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Ücretsiz|
|Disk şifreleme değerlendirmesi|✔|✔|-|Ücretsiz|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Ücretsiz|
|[Ağ güvenliği değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Ücretsiz|

--- 


> [!TIP]
>Yalnızca standart fiyatlandırma katmanında kullanılabilen özellikleri denemek için, ücretsiz katman kullanıcıları 30 günlük deneme sürümüne kaydolabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="supported-endpoint-protection-solutions"></a>Desteklenen uç nokta koruma çözümleri<a name="endpoint-supported"></a>

Aşağıdaki tablo da bir matris sağlar:

 - Her çözümü sizin için yüklemek için Azure Güvenlik Merkezi'ni kullanıp kullanamayacağınızı.
 - Güvenlik Merkezi hangi uç nokta koruma çözümlerini keşfedebilir. Bu listeden bir uç nokta koruma çözümü keşfedilirse, Güvenlik Merkezi bir çözüm yüklemenizi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi [için](security-center-endpoint-protection.md)Bkz.

| Uç Nokta Koruması| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)| Windows Server 2016| Hayır, işletim sisteminde yerleşik| Evet |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Evet |
| Trend Micro – Tüm sürümler* | Windows Server Ailesi  | Hayır | Evet |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Linux Server Ailesi  | Hayır | Evet**\*** |
| Sophos V9+| Linux Server Ailesi  | Hayır | Evet**\***  |

 **\*** Kapsama durumu ve destekleyici veriler şu anda yalnızca korumalı aboneliklerinizle ilişkili Log Analytics çalışma alanında kullanılabilir. Azure Güvenlik Merkezi portalına yansıtılmamaktadır.

> [!NOTE]
> - Windows Server 2008 R2 sanal makinesinde System Center Endpoint Protection'ın (SCEP) algılanması, PowerShell 3.0'dan (veya üst sürümden) sonra SCEP'in yüklenmesini gerektirir.
> - Derin Güvenlik ajanları için Trend Micro korumasının algılanması desteklenir.  OfficeScan aracıları desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik [Merkezi'nin verileri ve Log Analytics Aracısını](security-center-enable-data-collection.md)nasıl topladığını öğrenin.
- Güvenlik [Merkezi'nin verileri nasıl yönettiğini ve koruduğuhakkında](security-center-data-security.md)bilgi edinin.
- [Azure Güvenlik Merkezi'ni benimsemek için tasarım hususlarını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- Güvenlik [merkezini destekleyen platformları](security-center-os-coverage.md)gözden geçirin.
- [Azure Güvenlik Merkezi'nde Windows ve Linux makineleri için tehdit koruması](threat-protection.md#windows-machines)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi hakkında sık sorulan soruları](faq-general.md)bulun.