---
title: Azure Güvenlik Merkezi 'nin işletim sistemi, makine türü ve buluta göre özellikleri
description: Hangi Azure Güvenlik Merkezi özelliklerinin işletim sistemine, türüne ve bulut dağıtımına göre kullanılabilir olduğunu öğrenin.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: overview
ms.date: 02/16/2021
ms.author: memildin
ms.openlocfilehash: 070f35dc3efea4a3f05ec36cc3860fa589aa87e3
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102437919"
---
# <a name="feature-coverage-for-machines"></a>Makineler için özellik kapsamı

Aşağıdaki iki sekmede, Azure Güvenlik Merkezi 'nin Windows ve Linux sanal makineleri ve sunucuları için kullanılabilen özellikleri gösterilmektedir.

## <a name="supported-features-for-virtual-machines-and-servers"></a>Sanal makineler ve sunucular için desteklenen özellikler <a name="vm-server-features"></a>

### <a name="windows-machines"></a>[**Windows makineleri**](#tab/features-windows)

|**Özellik**|**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure yay etkin makineler**|**Azure Defender gerekli**
|----|:----:|:----:|:----:|:----:|
|[Uç nokta tümleştirmesi için Microsoft Defender](security-center-wdatp.md)|✔</br>(desteklenen sürümlerde)|✔</br>(desteklenen sürümlerde)|✔|Yes|
|[Sanal makine davranış analizi (ve güvenlik uyarıları)](alerts-reference.md)|✔|✔|✔|Yes|
|[Dosya daha az güvenlik uyarısı](alerts-reference.md#alerts-windows)|✔|✔|✔|Yes|
|[Ağ tabanlı güvenlik uyarıları](other-threat-protections.md#network-layer)|✔|✔|-|Yes|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Yes|
|[Yerel güvenlik açığı değerlendirmesi](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)|✔|-|✔|Yes|
|[Dosya bütünlüğünü izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Yes|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Yes|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Yes|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Yes|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Yes|
|Docker ile barındırılan IaaS kapsayıcıları üzerinde öneriler ve tehdit koruması|-|-|-|Yes|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Yay etkin: Evet|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Yay etkin: Evet|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|✔|✔|✔|Azure: Hayır<br><br>Yay etkin: Evet|
|Disk şifrelemesi değerlendirmesi|✔</br>( [desteklenen senaryolar](../virtual-machines/windows/disk-encryption-windows.md#unsupported-scenarios)için)|✔|-|Hayır|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|✔|Hayır|
|[Ağ güvenlik değerlendirmesi](security-center-network-recommendations.md)|✔|✔|-|Hayır|


### <a name="linux-machines"></a>[**Linux makineleri**](#tab/features-linux)

|**Özellik**|**Azure Sanal Makineler**|**Azure Sanal Makine Ölçek Kümeleri**|**Azure yay etkin makineler**|**Azure Defender gerekli**
|----|:----:|:----:|:----:|:----:|
|[Uç nokta tümleştirmesi için Microsoft Defender](security-center-wdatp.md)|-|-|-|-|
|[Sanal makine davranış analizi (ve güvenlik uyarıları)](./azure-defender.md)|✔</br>(desteklenen sürümlerde)|✔</br>(desteklenen sürümlerde)|✔|Yes|
|[Dosya daha az güvenlik uyarısı](alerts-reference.md#alerts-windows)|-|-|-|Yes|
|[Ağ tabanlı güvenlik uyarıları](other-threat-protections.md#network-layer)|✔|✔|-|Yes|
|[Tam zamanında VM erişimi](security-center-just-in-time.md)|✔|-|-|Yes|
|[Yerel güvenlik açığı değerlendirmesi](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)|✔|-|✔|Yes|
|[Dosya bütünlüğünü izleme](security-center-file-integrity-monitoring.md)|✔|✔|✔|Yes|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)|✔|-|✔|Yes|
|[Ağ eşlemesi](security-center-network-recommendations.md#network-map)|✔|✔|-|Yes|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md)|✔|-|-|Yes|
|[Mevzuat uyumluluk panosu & raporları](security-center-compliance-dashboard.md)|✔|✔|✔|Yes|
|Docker ile barındırılan IaaS kapsayıcıları üzerinde öneriler ve tehdit koruması|✔|✔|✔|Yes|
|Eksik işletim sistemi düzeltme ekleri değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Yay etkin: Evet|
|Güvenlik yapılandırması hataları değerlendirmesi|✔|✔|✔|Azure: Hayır<br><br>Yay etkin: Evet|
|[Endpoint Protection değerlendirmesi](security-center-services.md#supported-endpoint-protection-solutions-)|-|-|-|Hayır|
|Disk şifrelemesi değerlendirmesi|✔</br>( [desteklenen senaryolar](../virtual-machines/windows/disk-encryption-windows.md#unsupported-scenarios)için)|✔|-|Hayır|
|Üçüncü taraf güvenlik açığı değerlendirmesi|✔|-|✔|Hayır|
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
| Microsoft Defender virüsten koruma| Windows Server 2016 veya üzeri| Hayır, işletim sisteminde yerleşik| Yes |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Yes |
| Trend Micro – derin güvenlik | Windows Server Ailesi  | Hayır | Yes |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Yes |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Yes |
| McAfee v10+ | Linux sunucu ailesi  | Hayır | Yes |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Yes |

> [!NOTE]
> Windows Server 2008 R2 sanal makinesi üzerinde System Center Endpoint Protection (SCEP) algılama, PowerShell (v 3.0 veya üzeri) sonrasında SCEP yüklenmesini gerektirir.



## <a name="feature-support-in-government-clouds"></a>Kamu bulutlarında özellik desteği

| Hizmet/Özellik | US Gov | Çin gov |
|------|:----:|:----:|
|[Tam ZAMANıNDA VM erişimi](security-center-just-in-time.md) (1)|✔|✔|
|[Dosya bütünlüğü izleme](security-center-file-integrity-monitoring.md) (1)|✔|✔|
|[Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md) (1)|✔|✔|
|[Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md) (1)|-|-|
|[Docker ana bilgisayar sağlamlaştırma](harden-docker-hosts.md) (1)|✔|✔|
|[Makineler Için tümleşik güvenlik açığı değerlendirmesi](deploy-vulnerability-assessment-vm.md) (1)|-|-|
|[Uç nokta Için Microsoft Defender](harden-docker-hosts.md) (1)|✔|-|
|[AWS hesabına Bağlan](quickstart-onboard-aws.md) (1)|-|-|
|[GCP hesabını bağlama](quickstart-onboard-gcp.md) (1)|-|-|
|[Sürekli dışarı aktarma](continuous-export.md)|✔|✔|
|[İş akışı Otomasyonu](workflow-automation.md)|✔|✔|
|[Öneri muafiyet kuralları](exempt-resource.md)|-|-|
|[Uyarı engelleme kuralları](alerts-suppression-rules.md)|✔|✔|
|[Güvenlik uyarıları için e-posta bildirimleri](security-center-provide-security-contact-details.md)|✔|✔|
|[Varlık envanteri](asset-inventory.md)|✔|✔|
|[App Service için Azure Defender](defender-for-app-service-introduction.md)|-|-|
|[Depolama için Azure Defender](defender-for-storage-introduction.md)|✔|-|
|[SQL için Azure Defender](defender-for-sql-introduction.md)|✔|✔ (2)|
|[Key Vault için Azure Defender](defender-for-key-vault-introduction.md)|-|-|
|[Resource Manager için Azure Defender](defender-for-resource-manager-introduction.md)|-|-|
|[DNS için Azure Defender](defender-for-dns-introduction.md)|-|-|
|[Kapsayıcı kayıt defterleri için Azure Defender](defender-for-container-registries-introduction.md)|✔ (2)|✔ (2)|
|[Kubernetes için Azure Defender](defender-for-kubernetes-introduction.md)|✔|✔|
|[Kubernetes iş yükü koruması](kubernetes-workload-protections.md)|✔|✔|
|||

(1) **sunucular Için Azure Defender** gerektirir

(2) kısmi


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Merkezi 'nin Log Analytics aracısını kullanarak verileri nasıl topladığı](security-center-enable-data-collection.md)hakkında bilgi edinin.
- [Güvenlik Merkezi 'nin verileri nasıl yönettiğini ve korumalarını](security-center-data-security.md)öğrenin.
- [Güvenlik Merkezi 'ni destekleyen platformları](security-center-os-coverage.md)gözden geçirin.