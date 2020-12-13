---
title: Azure Güvenlik kıyaslaması v2-veri koruma
description: Azure Güvenlik kıyaslaması v2 veri koruma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 687c344aefc70729c85fb37d615ec0a272ff4fde
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97368877"
---
# <a name="security-control-v2-data-protection"></a>Güvenlik denetimi v2: veri koruma

Veri koruma, bekleyen veri koruma denetimini, geçiş sırasında ve yetkili erişim mekanizmaları aracılığıyla ele alır. Bu, erişim denetimi, şifreleme ve Azure 'da oturum açma kullanarak hassas veri varlıklarını bulma, sınıflandırma, koruma ve izleme içerir.

## <a name="dp-1-discovery-classify-and-label-sensitive-data"></a>DP-1: Hassas verileri bulun, sınıflandırın ve etiketleyin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| DP-1 | 13,1, 14,5, 14,7 | SC-28 |

Hassas bilgilerin depolanmasını, işlenmesini ve kuruluşun teknoloji sistemleri tarafından güvenli bir şekilde aktarılmasını sağlamak üzere uygun denetimleri tasarlayabilmeniz için hassas verilerinizi bulun, sınıflandırın ve etiketleyin. 

Azure’daki, şirket içindeki, Office 365’teki ve diğer konumlardaki hassas bilgiler için Azure Information Protection’ı (ve ilişkili tarama aracını) kullanın. 

Azure SQL Information Protection’ı kullanarak Azure SQL Veritabanlarında depolanan bilgilerin sınıflandırılmasına ve etiketlenmesine yardımcı olabilirsiniz.

- [Azure Information Protection kullanarak hassas bilgileri etiketleme](/azure/information-protection/what-is-information-protection) 

- [Azure SQL Veri Bulma’yı uygulama](../../azure-sql/database/data-discovery-and-classification-overview.md)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)  

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-2-protect-sensitive-data"></a>DP-2: Hassas verileri koruyun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| DP-2 | 13,2, 2,10 | SC-7, AC-4 |

Azure rol tabanlı erişim denetimi (Azure RBAC), ağ tabanlı erişim denetimleri ve Azure hizmetlerinde belirli denetimler (SQL ve diğer veritabanlarında şifreleme gibi) kullanarak erişimi kısıtlayarak hassas verileri koruyun. 

Tutarlı erişim denetimi sağlamak için tüm erişim denetimi türlerinin kurumsal segmentasyon stratejinizle uyumlu olması gerekir. Ayrıca kurumsal segmentasyon stratejisi, hassas ve iş açısından kritik verilerle sistemlerin konumu hakkında da bilgilendirilmelidir.

Microsoft tarafından yönetilen temel platform açısından, Microsoft tüm müşteri içeriğini hassas olarak değerlendirir ve müşteri verilerinin kaybına ve açığa çıkmasına karşı koruma sağlar. Azure içindeki müşteri verilerinin güvenlik altında kalmasını sağlamak için Microsoft bazı varsayılan veri koruma denetimleri ve özellikleri uygulamıştır.

- [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md)

- [Azure’da müşteri verilerinin korunmasını anlama](../fundamentals/protection-customer-data.md)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>DP-3: Yetkisiz hassas veri aktarımını izleyin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| DP-3 | 13,3 | AC-4, SI-4 |

Kurumsal görünürlük ve denetim dışındaki konumlara verilerin yetkisiz aktarılmasını izleyin. Bu normalde yetkisiz veri sızdırmaya işaret ediyor olabilecek anormal etkinliklerin (büyük veya alışılmamış aktarımlar) izlenmesini içerir. 

Azure Depolama Gelişmiş Tehdit Koruması (ATP) ve Azure SQL ATP, hassas bilgilerin yetkisiz aktarımlarına işaret ediyor olabilecek anormal bilgi aktarımında uyarı verebilir. 

Azure Information Protection (AIP) sınıflandırılmış ve etiketlenmiş bilgileri izleme özellikleri sağlar. 

Veri kaybı önleme (DLP) uyumluluğu için gerekirse, veri sızdırmayı önlemeye yönelik algılama ve/veya koruma denetimlerini zorunlu tutmak üzere konak tabanlı bir DLP çözümü kullanabilirsiniz.

- [Azure SQL ATP’yi Etkinleştirme](../../azure-sql/database/threat-detection-overview.md)

- [Azure Depolama ATP’yi Etkinleştirme](../../storage/common/azure-defender-storage-configure.md?tabs=azure-security-center)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security) 

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Hassas bilgileri aktarım sırasında şifreleyin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| DP-4 | 14,4 | SC-8 |

Erişim denetimlerini tamamlamak için, yoldaki verilerin verileri kolayca okuyamadığından veya değiştiremeyeceği için şifreleme kullanılarak ' bant dışı ' saldırılarına (örn. trafik yakalama) karşı korunması gerekir. 

Bu, özel ağlardaki trafik için isteğe bağlı olsa da, bu, dış ve genel ağlardaki trafik için önemlidir. HTTP trafiği için, Azure kaynaklarınıza bağlanan istemcilerin TLS v 1.2 veya üzerini anlaştığınızdan emin olun. Uzaktan Yönetim için şifrelenmemiş bir protokol yerine SSH (Linux için) veya RDP/TLS (Windows için) kullanın. Kullanımdan çıkarıldı SSL, TLS ve SSH sürümleri ve protokolleri ve zayıf şifrelemeler devre dışı bırakılmalıdır.  

Azure, varsayılan olarak Azure veri merkezleri arasında aktarım halindeki veriler için şifreleme sağlar. 

- [Azure ile iletim sırasında şifrelemeyi anlama](../fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [TLS güvenliği hakkında bilgi](/security/engineering/solving-tls1-problem)

- [Yoldaki Azure verileri için çift şifreleme](../fundamentals/double-encryption.md#data-in-transit)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

## <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: Bekleyen hassas verileri şifreleyin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| DP-5 | 14,8 | SC-28, SC-12 |

Erişim denetimlerini tamamlamak için, Rest 'teki verilerin şifreleme kullanılarak ' bant dışı ' saldırılarına (temeldeki depolamaya erişme gibi) karşı korunması gerekir. Bu, saldırganların verileri kolayca okuyabilmesini veya değiştirememesini sağlamaya yardımcı olur. 

Azure, bekleyen veriler için varsayılan olarak şifreleme sağlar. Son derece hassas veriler için, kullanılabilir olduğunda tüm Azure kaynaklarında bekleyen ek şifreleme uygulama seçenekleriniz vardır. Azure, şifreleme anahtarlarınızı varsayılan olarak yönetir, ancak Azure belirli Azure hizmetleri için kendi anahtarlarınızı (müşteri tarafından yönetilen anahtarlar) yönetmek için seçenekler sağlar.

- [Azure’da bekleyen veri şifrelemesini anlama](../fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services)

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](../../storage/common/customer-managed-keys-configure-key-vault.md)

- [Şifreleme modeli ve anahtar yönetimi tablosu](../fundamentals/encryption-models.md)

- [Azure 'da Rest çift Şifrelemeli veriler](../fundamentals/double-encryption.md#data-at-rest)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)