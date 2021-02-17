---
title: Azure Güvenlik denetimi-veri koruma
description: Azure Güvenlik denetimi veri koruması
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: d36ed6a795c5fa2241ee71751053a4cb5986aaf0
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100576835"
---
# <a name="security-control-data-protection"></a>Güvenlik denetimi: veri koruma

Veri koruma önerileri şifreleme, erişim denetim listeleri, kimlik tabanlı erişim denetimi ve veri erişimi için denetim günlüğü ile ilgili sorunları gidermeye odaklanmaktadır.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.1 | 13.1 | Müşteri |

Gizli bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.2 | 13,2, 2,10 | Müşteri |

Ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Azure kaynaklarınıza erişim düzeyini kısıtlayabilirsiniz. Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla Azure kaynaklarına erişimi denetleyebilirsiniz. 

- [Ek Azure abonelikleri oluşturma](../../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../../azure-resource-manager/management/tag-resources.md)

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.3 | 13,3 | Paylaşılan |

Hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir üçüncü taraf çözümünü Azure Marketi 'nden yararlanın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini, müşteri veri kaybına ve pozlamaya karşı hassas ve koruma olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure’da müşteri verilerinin korunmasını anlama](../fundamentals/protection-customer-data.md)

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.4 | 14.4 | Paylaşılan |

Yoldaki tüm hassas bilgileri şifreleyin. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın.

- [Azure ile iletim sırasında şifrelemeyi anlama](../fundamentals/encryption-overview.md#encryption-of-data-in-transit)

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,5 | 14.5 | Paylaşılan |

Azure 'daki belirli bir hizmet için kullanılabilir bir özellik yoksa, şirket içinde veya uzak bir hizmet sağlayıcısında bulunan ve kuruluşun önemli bilgi envanterini güncelleştiren tüm hassas bilgileri, kuruluşunuzun teknoloji sistemleri tarafından saklanan, işlenen veya aktarılan tüm hassas bilgileri tanımlamak için üçüncü taraf bir etkin bulma aracı kullanın.

Microsoft 365 belgeler içindeki hassas bilgileri tanımlamak için Azure Information Protection kullanın.

Azure SQL veritabanı 'nda depolanan bilgilerin sınıflandırmasına ve etiketlemesine yardımcı olması için Azure SQL Information Protection kullanın.

- [Azure SQL Veri Bulma’yı uygulama](../../azure-sql/database/data-discovery-and-classification-overview.md)

- [Azure Information Protection uygulama](/azure/information-protection/deployment-roadmap)

- [Azure’da müşteri verilerinin korunmasını anlama](../fundamentals/protection-customer-data.md)

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,6 | 14,6 | Müşteri |

Veri ve kaynaklara erişimi denetlemek için Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) kullanın, aksi takdirde hizmete özel erişim denetimi yöntemlerini kullanın.

- [Azure RBAC 'yi yapılandırma](../../role-based-access-control/role-assignments-portal.md)

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.7 | 14.7 | Paylaşılan |

İşlem kaynaklarında uyumluluk için gerekliyse, verileri bir sistemden kopyalandıklarında bile verilere erişim denetimlerine zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure’da müşteri verilerinin korunmasını anlama](../fundamentals/protection-customer-data.md)

## <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.8 | 14.8 | Müşteri |

Tüm Azure kaynaklarında bekleyen şifreleme kullanın. Microsoft, Azure 'un şifreleme anahtarlarınızı yönetmesine izin vermesini önerir, ancak bazı örneklerde kendi anahtarlarınızı yönetmeniz için seçenek vardır. 

- [Azure’da bekleyen veri şifrelemesini anlama](../fundamentals/encryption-atrest.md)

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](../../storage/common/customer-managed-keys-configure-key-vault.md)

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,9 | 14.9 | Müşteri |

Azure Izleyici 'yi Azure etkinlik günlüğü ile birlikte kullanarak, önemli Azure kaynaklarına yapılan değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../../azure-monitor/alerts/alerts-activity-log.md)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın:  [güvenlik açığı yönetimi](security-control-vulnerability-management.md)