---
title: Azure Güvenlik Denetimi - Veri Koruma
description: Azure Güvenlik Denetimi Veri Koruması
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 035894c80e619851264aae91daa2d7852d156964
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81408553"
---
# <a name="security-control-data-protection"></a>Güvenlik Denetimi: Veri Koruma

Veri koruma önerileri, şifreleme, erişim denetimi listeleri, kimlik tabanlı erişim denetimi ve veri erişimi için denetim günlüğüyle ilgili sorunları ele almaya odaklanır.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.1 | 13.1 | Müşteri |

Hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için Etiketler'i kullanın.

- [Etiketler nasıl oluşturulur ve kullanılır](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.2 | 13.2, 2.10 | Müşteri |

Ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamlarınızın talep ettiği Azure kaynaklarına erişim düzeyini kısıtlayabilirsiniz. Azure Etkin Dizin rolü tabanlı erişim denetimi aracılığıyla Azure kaynaklarına erişimi denetleyebilirsiniz. 

- [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Yönetim Grupları nasıl oluşturulur?](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.3 | 13.3 | Paylaşımlı |

Azure Marketi'nin, hassas bilgilerin yetkisiz aktarımı için izleyen ve bilgi güvenliği profesyonellerini uyarırken bu tür aktarımları engelleyen ağ çevresinde üçüncü taraf çözümünden yararlanın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas ve müşteri veri kaybına ve açığa karşı koruma olarak ele adatır. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

- [Azure'da müşteri verilerinin korunmasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.4 | 14.4 | Paylaşımlı |

Geçişteki tüm hassas bilgileri şifreleyin. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya daha büyük bir anlaşma sağlayabilmesini sağlayın.

Mümkün olduğu durumlarda, aktarım sırasında istirahat te şifreleme ve şifreleme için Azure Güvenlik Merkezi önerilerine uyun.

- [Azure ile aktarım sırasında şifrelemeyi anlama](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4,5 | 14.5 | Paylaşımlı |

Azure'daki özel hizmetiniz için hiçbir özellik yoksa, kurumda veya uzak hizmet sağlayıcısında bulunanlar da dahil olmak üzere kuruluşun teknoloji sistemleri tarafından depolanan, işlenen veya iletilen tüm hassas bilgileri tanımlamak ve kuruluşun hassas bilgi envanterini güncelleştirmek için bir üçüncü taraf etkin bulma aracı kullanın.

Office 365 belgelerindeki hassas bilgileri tanımlamak için Azure Bilgi Koruması'nı kullanın.

Azure SQL Veritabanlarında depolanan bilgilerin sınıflandırılmasına ve etiketlenmesine yardımcı olmak için Azure SQL Bilgi Koruması'nı kullanın.

- [Azure SQL Veri Bulma nasıl uygulanır?](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

- [Azure Bilgi Koruması nasıl uygulanır?](https://docs.microsoft.com/azure/information-protection/deployment-roadmap)

- [Azure'da müşteri verilerinin korunmasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Rol tabanlı erişim denetimini kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.6 | 14.6 | Müşteri |

Verilere ve kaynaklara erişimi denetlemek için Azure AD RBAC'ı kullanın, aksi takdirde hizmete özel erişim denetim yöntemlerini kullanın.

- [Azure'da RBAC nasıl yapılandırılmaz?](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.7 | 14.7 | Paylaşımlı |

Bilgi işlem kaynaklarına uyumluluk için gerekliyse, veriler sistemden kopyalandığında bile verilere erişim denetimlerini zorlamak için otomatik ana bilgisayar tabanlı Veri Kaybı Önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

- [Azure'da müşteri verilerinin korunmasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.8 | 14.8 | Müşteri |

Tüm Azure kaynaklarında şifrelemeyi kullanın. Microsoft, Azure'un şifreleme anahtarlarınızı yönetmesine izin vermenizi önerir, ancak bazı durumlarda kendi anahtarlarınızı yönetmeniz seçeneği vardır. 

- [Azure'da dinlenilerde şifrelemeyi anlama](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

- [Müşteri tarafından yönetilen şifreleme anahtarları nasıl yapılandırılabilen](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal)

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.9 | 14.9 | Müşteri |

Kritik Azure kaynaklarında ne zaman değişiklikler yapılacağına dair uyarılar oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.

- [Azure Etkinlik Günlüğü etkinlikleri için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki Güvenlik Denetimine bakın: [Güvenlik Açığı Yönetimi](security-control-vulnerability-management.md)