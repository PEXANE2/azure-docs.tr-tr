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
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2020
ms.author: memildin
ms.openlocfilehash: a311439e8efc5481fbfd7431c1514ba6be576e39
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91858022"
---
# <a name="feature-coverage-for-machines"></a>Makineler için özellik kapsamı

Aşağıdaki iki sekmede, Azure Güvenlik Merkezi 'nin Windows ve Linux sanal makineleri ve sunucuları için kullanılabilen özellikleri gösterilmektedir.

## <a name="supported-features-for-virtual-machines-and-servers"></a>Sanal makineler ve sunucular için desteklenen özellikler <a name="vm-server-features"></a>

### <a name="windows-machines"></a>[**Windows makineleri**](#tab/features-windows)

|**Özellik**|**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure dışı makineler**|**Azure Defender gerekli**
|----|:----:|:----:|:----:|:----:|
|[Microsoft Defender ATP tümleştirmesi](security-center-wdatp.md)|✔</br>(desteklenen sürümlerde)|✔</br>(desteklenen sürümlerde)|✔|Evet|
|[Sanal makine davranış analizi (ve güvenlik uyarıları)](alerts-reference.md)|✔|✔|✔|Evet|
|[Dosya daha az güvenlik uyarısı](alerts-reference.md#alerts-windows)|✔|✔|✔|Evet|
|[Ağ tabanlı güvenlik uyarıları](other-threat-protections.md#network-layer)|✔|✔|-|Evet|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Evet|
|[Yerel güvenlik açığı değerlendirmesi](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)|✔|-|-|Evet|
|[Dosya bütünlüğünü izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Evet|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Evet|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Evet|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Evet|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Evet|
|Docker ile barındırılan IaaS kapsayıcıları üzerinde öneriler ve tehdit koruması|-|-|-|Evet|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Azure dışı: Evet|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Azure dışı: Evet|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Azure: Hayır<br><br>Azure dışı: Evet|
|Disk şifrelemesi değerlendirmesi|✔</br>( [desteklenen senaryolar](../virtual-machines/windows/disk-encryption-windows.md#unsupported-scenarios)için)|✔|-|Hayır|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Hayır|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Hayır|


### <a name="linux-machines"></a>[**Linux makineleri**](#tab/features-linux)

|**Özellik**|**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure dışı makineler**|**Azure Defender gerekli**
|----|:----:|:----:|:----:|:----:|
|[Microsoft Defender ATP tümleştirmesi](security-center-wdatp.md)|-|-|-|Evet|
|[Sanal makine davranış analizi (ve güvenlik uyarıları)](security-center-alerts-iaas.md)|✔</br>(desteklenen sürümlerde)|✔</br>(desteklenen sürümlerde)|✔|Evet|
|[Dosya daha az güvenlik uyarısı](alerts-reference.md#alerts-windows)|-|-|-|Evet|
|[Ağ tabanlı güvenlik uyarıları](other-threat-protections.md#network-layer)|✔|✔|-|Evet|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Evet|
|[Yerel güvenlik açığı değerlendirmesi](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)|✔|-|-|Evet|
|[Dosya bütünlüğünü izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Evet|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Evet|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Evet|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Evet|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Evet|
|Docker ile barındırılan IaaS kapsayıcıları üzerinde öneriler ve tehdit koruması|✔|✔|✔|Evet|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Azure dışı: Evet|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Azure dışı: Evet|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Hayır|
|Disk şifrelemesi değerlendirmesi|✔</br>( [desteklenen senaryolar](../virtual-machines/windows/disk-encryption-windows.md#unsupported-scenarios)için)|✔|-|Hayır|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|-|Hayır|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Hayır|

--- 


> [!TIP]
>Yalnızca Azure Defender ile kullanılabilen özelliklerle denemeler yapmak için 30 günlük bir denemeye kayıt yapabilirsiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="supported-endpoint-protection-solutions"></a>Desteklenen Endpoint Protection çözümleri <a name="endpoint-supported"></a>

Aşağıdaki tabloda bir matrisi verilmiştir:

 - Her çözümü sizin için yüklemek üzere Azure Güvenlik Merkezi 'ni kullanıp kullanmayacağınızı belirtir.
 - Güvenlik Merkezi 'nin hangi Endpoint Protection çözümlerini keşfedebileceği. Bu listeden bir uç nokta koruma çözümü bulunursa, güvenlik merkezi bir tane yüklemeyi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi için bkz. [Endpoint Protection değerlendirme ve öneriler](security-center-endpoint-protection.md).

| Uç Nokta Koruma| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Microsoft Defender virüsten koruma| Windows Server 2016 veya üzeri| Hayır, işletim sisteminde yerleşik| Evet |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Evet |
| Trend Micro – derin güvenlik | Windows Server Ailesi  | Hayır | Evet |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Linux sunucu ailesi  | Hayır | Yes **\*** |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Yes  **\***  |

 **\*** Kapsam durumu ve destekleyici veriler şu anda yalnızca korunan aboneliklerinizle ilişkili olan Log Analytics çalışma alanında kullanılabilir. Azure Güvenlik Merkezi portalında yansıtılmaz.

> [!NOTE]
> Windows Server 2008 R2 sanal makinesi üzerinde System Center Endpoint Protection (SCEP) algılama, PowerShell (v 3.0 veya üzeri) sonrasında SCEP yüklenmesini gerektirir.


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin Log Analytics aracısını kullanarak verileri nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Güvenlik Merkezi 'ni destekleyen platformları](security-center-os-coverage.md)gözden geçirin.
