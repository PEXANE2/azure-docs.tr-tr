---
title: Azure Güvenlik kıyaslama v2-uç nokta güvenliği
description: Azure Güvenlik kıyaslaması v2 uç nokta güvenliği
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 48b22ba913370b27cd01319a14a2a627d7589ce4
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051523"
---
# <a name="security-control-v2-endpoint-security"></a>Güvenlik denetimi v2: uç nokta güvenliği

Uç nokta güvenliği, uç nokta algılama ve yanıttaki denetimleri ele alır. Bu, Azure ortamlarındaki uç noktalar için uç nokta algılama ve yanıt (EDR) ve kötü amaçlı yazılımdan koruma hizmeti kullanımını içerir.

Geçerli yerleşik Azure Ilkesini görmek için bkz. [Azure Güvenlik kıyaslaması uyumluluğu yerleşik girişiminin ayrıntıları: uç nokta güvenliği](../../governance/policy/samples/azure-security-benchmark.md#endpoint-security)

## <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uç nokta algılamayı ve yanıtını kullanın (EDR)

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ES-1 | 8.1 | SI-2, SI-3, SC-3 |

Sunucular ve istemciler için uç nokta algılamayı ve yanıtı (EDR) yeteneklerini etkinleştirin ve SıEM ve güvenlik Işlemleri işlemleriyle tümleştirin.

Uç nokta için Microsoft Defender, gelişmiş tehditleri önlemeye, algılamaya, araştırmaya ve yanıt vermeye yönelik bir kurumsal uç nokta güvenlik platformunun parçası olarak EDR özelliği sağlar.

- [Endpoint için Microsoft Defender genel bakış](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Windows Server için uç nokta için Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Windows dışı sunucular için uç nokta için Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-2-use-centrally-managed-modern-anti-malware-software"></a>ES-2: merkezi olarak yönetilen modern kötü amaçlı yazılımdan koruma yazılımı kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ES-2 | 8.1 | SI-2, SI-3, SC-3 |

Gerçek zamanlı ve düzenli tarama özellikli merkezi olarak yönetilen bir uç nokta kötü amaçlı yazılımdan koruma çözümü kullanın

Azure Güvenlik Merkezi, sanal makineleriniz için çeşitli popüler kötü amaçlı yazılımdan koruma çözümlerinin kullanımını otomatik olarak tanımlayabilir ve uç nokta koruma çalıştırma durumunu raporlar ve öneriler yapar. 

Azure için Microsoft kötü amaçlı yazılımdan koruma Cloud Services, Windows sanal makineleri (VM 'Ler) için varsayılan kötü amaçlı yazılımdan koruma. Linux VM 'Ler için üçüncü taraf kötü amaçlı yazılımdan koruma çözümünü kullanın. Ayrıca, Azure depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanabilirsiniz. 

- [Cloud Services ve sanal makineler için Microsoft Antimalware 'i yapılandırma](../fundamentals/antimalware.md)

- [Desteklenen Endpoint Protection çözümleri](../../security-center/security-center-services.md?tabs=features-windows#supported-endpoint-protection-solutions-)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-3-ensure-anti-malware-software-and-signatures-are-updated"></a>ES-3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ES-3 | 8.2 | SI-2, SI-3 |

Kötü amaçlı yazılımdan koruma imzalarının hızlı ve tutarlı bir şekilde güncelleştirildiğinden emin olun.

Tüm uç noktaların en son imzalarla güncel olduğundan emin olmak için Azure Güvenlik Merkezi 'ndeki önerileri izleyin: "Işlem & uygulamalar". Microsoft kötü amaçlı yazılımdan koruma, varsayılan olarak en son imzaları ve altyapı güncelleştirmelerini otomatik olarak yükler. Linux için, imzaların üçüncü taraf kötü amaçlı yazılımdan koruma çözümünde güncelleştirildiğinden emin olun.

- [Azure Cloud Services ve sanal makineler için Microsoft Antimalware nasıl dağıtılır](../fundamentals/antimalware.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Azure Güvenlik Merkezi 'nde Endpoint Protection değerlendirmesi ve önerileri](../../security-center/security-center-endpoint-protection.md)