---
title: Azure Güvenlik kıyaslama v2-uç nokta güvenliği
description: Azure Güvenlik kıyaslaması v2 uç nokta güvenliği
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: c04e4233ded34ceaeec9cd9afb240d3d1ac864e0
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059383"
---
# <a name="security-control-endpoint-security"></a>Güvenlik denetimi: uç nokta güvenliği

Uç nokta güvenliği, uç nokta algılama ve yanıttaki denetimleri ele alır. Bu, Azure ortamlarındaki uç noktalar için uç nokta algılama ve yanıt (EDR) ve kötü amaçlı yazılımdan koruma hizmeti kullanımını içerir.

## <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uç nokta algılamayı ve yanıtını kullanın (EDR)

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ES-1 | 8.1 | SI-2, SI-3, SC-3 |

Sunucular ve istemciler için uç nokta algılamayı ve yanıtı (EDR) yeteneklerini etkinleştirin ve SıEM ve güvenlik Işlemleri işlemleriyle tümleştirin.

Microsoft Defender Gelişmiş tehdit koruması, gelişmiş tehditleri önlemeye, algılamaya, araştırmaya ve yanıt vermeye yönelik bir kurumsal uç nokta güvenlik platformunun parçası olarak EDR özelliği sağlar. 

- [Microsoft Defender Gelişmiş tehdit korumasına genel bakış](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Windows Server 'lar için Microsoft Defender ATP hizmeti](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Windows dışı sunucular için Microsoft Defender ATP hizmeti](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-2-use-centrally-managed-modern-anti-malware-software"></a>ES-2: merkezi olarak yönetilen modern kötü amaçlı yazılımdan koruma yazılımı kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ES-2 | 8.1 | SI-2, SI-3, SC-3 |

Gerçek zamanlı ve düzenli tarama özellikli merkezi olarak yönetilen bir uç nokta kötü amaçlı yazılımdan koruma çözümü kullanın

Azure Güvenlik Merkezi, sanal makineleriniz için çeşitli popüler kötü amaçlı yazılımdan koruma çözümlerinin kullanımını otomatik olarak tanımlayabilir ve uç nokta koruma çalıştırma durumunu raporlar ve öneriler yapar. 

Azure için Microsoft kötü amaçlı yazılımdan koruma Cloud Services, Windows sanal makineleri (VM 'Ler) için varsayılan kötü amaçlı yazılımdan koruma. Linux VM 'Ler için üçüncü taraf kötü amaçlı yazılımdan koruma çözümünü kullanın.  Ayrıca, Azure depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanabilirsiniz. 

- [Cloud Services ve sanal makineler için Microsoft Antimalware 'i yapılandırma](../fundamentals/antimalware.md)

- [Desteklenen Endpoint Protection çözümleri](https://docs.microsoft.com/azure/security-center/security-center-services?tabs=features-windows#supported-endpoint-protection-solutions-)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-3-ensure-anti-malware-software-and-signatures-are-updated"></a>ES-3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ES-3 | 8.2 | SI-2, SI-3 |

Kötü amaçlı yazılımdan koruma imzalarının hızlı ve tutarlı bir şekilde güncelleştirildiğinden emin olun. 

Azure Güvenlik Merkezi 'ndeki önerileri takip edin: "Işlem &amp; Uygulamaları", tüm uç noktaların en son imzalarla güncel olduğundan emin olmak için. Microsoft kötü amaçlı yazılımdan koruma, varsayılan olarak en son imzaları ve altyapı güncelleştirmelerini otomatik olarak yükler. Linux için üçüncü taraf kötü amaçlı yazılımdan koruma çözümünü kullanın.

- [Azure Cloud Services ve sanal makineler için Microsoft Antimalware nasıl dağıtılır](../fundamentals/antimalware.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

