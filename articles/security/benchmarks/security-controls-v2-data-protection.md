---
title: Azure Güvenlik kıyaslaması v2-veri koruma
description: Azure Güvenlik kıyaslaması v2 veri koruma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: e377c6669b1db96f549958173aded4f29bae6713
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059382"
---
# <a name="security-control-data-protection"></a>Güvenlik denetimi: veri koruma

Veri koruma, bekleyen veri koruma denetimini, geçiş sırasında ve yetkili erişim mekanizmaları aracılığıyla ele alır. Bu, erişim denetimi, şifreleme ve Azure 'da oturum açma kullanarak hassas veri varlıklarını bulma, sınıflandırma, koruma ve izleme içerir.

## <a name="dp-1-discovery-classify-and-label-sensitive-data"></a>DP-1: hassas verileri bulma, sınıflandırma ve etiketleme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| DP-1 | 14,5, 14,7 | SI-4, SC-28 |

Hassas bilgilerin depolanmasını, işlenmesini ve kuruluşun teknoloji sistemleri tarafından güvenli bir şekilde aktarılmasını sağlamak üzere uygun denetimleri tasarlayabilmeniz için hassas verilerinizi bulun, sınıflandırın ve etiketleyin. 

Azure 'da, şirket içinde, Office 365 ' de ve diğer konumlarda bulunan Office belgelerindeki önemli bilgiler için Azure Information Protection (ve ilişkili Tarama Aracı) kullanın. 

Azure SQL veritabanlarında depolanan bilgilerin sınıflandırmasına ve etiketlemesine yardımcı olması için Azure SQL Information Protection kullanabilirsiniz.

- [Azure Information Protection kullanarak gizli bilgileri etiketleme](/azure/information-protection/what-is-information-protection) 

- [Azure SQL veri bulmayı uygulama](/azure/sql-database/sql-database-data-discovery-and-classification)

**Sorumluluk**: paylaşılan

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)  

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-2-protect-sensitive-data"></a>DP-2: hassas verileri koruma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| DP-2 | 13,2, 2,10 | SC-7, AC-4 |

Azure rol tabanlı Access Control (Azure RBAC), ağ tabanlı erişim denetimleri ve Azure hizmetlerinde belirli denetimleri (SQL ve diğer veritabanlarında şifreleme gibi) kullanarak erişimi kısıtlayarak hassas verileri koruyun. 

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır. Kurumsal segmentasyon stratejisi Ayrıca hassas veya iş açısından kritik veri ve sistemlerinin konumu ile de bilgilendirilir.

Microsoft tarafından yönetilen temel platform için, Microsoft tüm müşteri içeriklerini müşteri veri kaybına ve pozlamaya karşı hassas ve koruyucuları olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için, Microsoft bazı varsayılan veri koruma denetimleri ve yeteneklerini uygulamıştır.

- [Azure rol tabanlı Access Control (RBAC)](../../role-based-access-control/overview.md)

- [Azure 'da müşteri veri korumasını anlama](../fundamentals/protection-customer-data.md)

**Sorumluluk**: paylaşılan

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>DP-3: hassas verilerin yetkisiz aktarımını Izleme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| DP-3 | 13,3 | AC-4, SI-4 |

Kurumsal görünürlük ve denetim dışındaki konumlara verilerin yetkisiz aktarılmasını izleyin. Bu, genellikle yetkisiz veri taşmasını gösterebilen anormal etkinliklerin (büyük veya olağan dışı aktarımların) izlenmesini içerir. 

Azure depolama Gelişmiş tehdit koruması (ATP) ve Azure SQL ATP, hassas bilgilerin yetkisiz aktarımlarını gösterebilen anormal bir bilgi aktarımına uyarı verebilir. 

Azure Information Protection (AıP), sınıflandırılan ve etiketlendiği bilgiler için izleme özellikleri sağlar. 

Veri kaybı önleme (DLP) uyumluluğu için gerekliyse, bir ana bilgisayar tabanlı DLP çözümünü kullanarak, veri kaybını önlemek için algılama ve/veya koruyucu denetimleri zorunlu kılabilirsiniz.

- [Azure SQL ATP 'yi etkinleştirme](../../azure-sql/database/threat-detection-overview.md)

- [Azure depolama ATP 'yi etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center)

**Sorumluluk**: paylaşılan

**Müşteri güvenlik katılımcıları**:

- [Güvenlik işlemleri merkezi (SOC)](/azure/cloud-adoption-framework/organize/cloud-security) 

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: geçiş sırasında hassas bilgileri şifreleyin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| DP-4 | 14,4 | SC-8 |

Erişim denetimlerini tamamlamak için, yoldaki verilerin verileri kolayca okuyamadığından veya değiştiremeyeceği için şifreleme kullanılarak ' bant dışı ' saldırılarına (örn. trafik yakalama) karşı korunması gerekir. 

Bu, özel ağlardaki trafik için isteğe bağlı olsa da, bu, dış ve genel ağlardaki trafik için önemlidir. HTTP trafiği için, Azure kaynaklarınıza bağlanan istemcilerin TLS v 1.2 veya üzerini anlaştığınızdan emin olun. Uzaktan Yönetim için şifrelenmemiş bir protokol yerine SSH (Linux için) veya RDP/TLS (Windows için) kullanın. Kullanımdan çıkarıldı SSL, TLS ve SSH sürümleri ve protokolleri ve zayıf şifrelemeler devre dışı bırakılmalıdır.  

Azure, varsayılan olarak Azure veri merkezleri arasında aktarım halindeki veriler için şifreleme sağlar. 

- [Azure ile iletim sırasında şifrelemeyi anlama](../fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [TLS güvenliği hakkında bilgi](/security/engineering/solving-tls1-problem)

- [Yoldaki Azure verileri için çift şifreleme](../fundamentals/double-encryption.md#data-in-transit)

**Sorumluluk**: paylaşılan

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

## <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: hassas verileri bekleyen şifreleme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| DP-5 | 14,8 | SC-28, SC-12 |

Erişim denetimlerini tamamlamak için, Rest 'teki verilerin şifreleme kullanılarak ' bant dışı ' saldırılarına (temeldeki depolamaya erişme gibi) karşı korunması gerekir. Bu, saldırganların verileri kolayca okuyabilmesini veya değiştirememesini sağlamaya yardımcı olur. 

Azure, bekleyen veriler için varsayılan olarak şifreleme sağlar. Son derece hassas veriler için, kullanılabilir olduğunda tüm Azure kaynaklarında bekleyen ek şifreleme uygulama seçenekleriniz vardır. Azure, şifreleme anahtarlarınızı varsayılan olarak yönetir, ancak Azure belirli Azure hizmetleri için kendi anahtarlarınızı (müşteri tarafından yönetilen anahtarlar) yönetmek için seçenekler sağlar.

- [Azure 'da bekleyen şifrelemeyi anlama](../fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services)

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](../../storage/common/storage-encryption-keys-portal.md)

- [Şifreleme modeli ve anahtar yönetimi tablosu](../fundamentals/encryption-models.md)

- [Azure 'da Rest çift Şifrelemeli veriler](../fundamentals/double-encryption.md#data-at-rest)

**Sorumluluk**: paylaşılan

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

