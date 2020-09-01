---
title: Azure Güvenlik denetimi-veri koruma
description: Azure Güvenlik denetimi veri koruması
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 2622d7a2e54dac710ac09d61dee639ab1d104f02
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89079005"
---
# <a name="security-control-data-protection"></a>Güvenlik denetimi: veri koruma

Veri koruma önerileri şifreleme, erişim denetim listeleri, kimlik tabanlı erişim denetimi ve veri erişimi için denetim günlüğü ile ilgili sorunları gidermeye odaklanmaktadır.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.1 | 13,1 | Müşteri |

Gizli bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.2 | 13,2, 2,10 | Müşteri |

Ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Azure kaynaklarınıza erişim düzeyini kısıtlayabilirsiniz. Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla Azure kaynaklarına erişimi denetleyebilirsiniz. 

- [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.3 | 13,3 | Paylaşılan |

Hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir üçüncü taraf çözümünü Azure Marketi 'nden yararlanın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini, müşteri veri kaybına ve pozlamaya karşı hassas ve koruma olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.4 | 14,4 | Paylaşılan |

Yoldaki tüm hassas bilgileri şifreleyin. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın.

- [Azure ile iletim sırasında şifrelemeyi anlama](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,5 | 14,5 | Paylaşılan |

Azure 'daki belirli bir hizmet için kullanılabilir bir özellik yoksa, şirket içinde veya uzak bir hizmet sağlayıcısında bulunan ve kuruluşun önemli bilgi envanterini güncelleştiren tüm hassas bilgileri, kuruluşunuzun teknoloji sistemleri tarafından saklanan, işlenen veya aktarılan tüm hassas bilgileri tanımlamak için üçüncü taraf bir etkin bulma aracı kullanın.

Office 365 belgelerindeki önemli bilgileri tanımlamak için Azure Information Protection kullanın.

Azure SQL veritabanı 'nda depolanan bilgilerin sınıflandırmasına ve etiketlemesine yardımcı olması için Azure SQL Information Protection kullanın.

- [Azure SQL veri bulmayı uygulama](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

- [Azure Information Protection uygulama](https://docs.microsoft.com/azure/information-protection/deployment-roadmap)

- [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4.6 | 14,6 | Müşteri |

Veri ve kaynaklara erişimi denetlemek için Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) kullanın, aksi takdirde hizmete özel erişim denetimi yöntemlerini kullanın.

- [Azure RBAC 'yi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,7 | 14,7 | Paylaşılan |

İşlem kaynaklarında uyumluluk için gerekliyse, verileri bir sistemden kopyalandıklarında bile verilere erişim denetimlerine zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,8 | 14,8 | Müşteri |

Tüm Azure kaynaklarında bekleyen şifreleme kullanın. Microsoft, Azure 'un şifreleme anahtarlarınızı yönetmesine izin vermesini önerir, ancak bazı örneklerde kendi anahtarlarınızı yönetmeniz için seçenek vardır. 

- [Azure 'da bekleyen şifrelemeyi anlama](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal)

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 4,9 | 14,9 | Müşteri |

Azure Izleyici 'yi Azure etkinlik günlüğü ile birlikte kullanarak, önemli Azure kaynaklarına yapılan değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın:  [güvenlik açığı yönetimi](security-control-vulnerability-management.md)