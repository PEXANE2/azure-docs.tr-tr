---
title: Azure Güvenlik Denetimi - Veri Koruma
description: Güvenlik Denetimi Veri Koruması
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 5482495f87e87e5d05d8adca6b053810a62dcb4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934514"
---
# <a name="security-control-data-protection"></a>Güvenlik Denetimi: Veri Koruma

Veri koruma önerileri, şifreleme, erişim denetimi listeleri, kimlik tabanlı erişim denetimi ve veri erişimi için denetim günlüğüyle ilgili sorunları ele almaya odaklanır.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.1 | 13.1 | Müşteri |

Hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için Etiketler'i kullanın.

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.2 | 13.2 | Müşteri |

Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar VNet/Subnet tarafından ayrılmalı, uygun şekilde etiketlenmelidir ve bir NSG veya Azure Güvenlik Duvarı tarafından güvence altına alınmalıdır. Hassas verilerin depolanması veya işlenmesi gereken kaynaklar yeterince izole edilmelidir. Hassas verileri depolamak veya işlemek için, kullanılmadığı zaman kapatmak için ilke ve yordam(lar) uygulayın.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal Ağ nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Config ile bir NSG oluşturmak için nasıl:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Azure Güvenlik Duvarı nasıl dağıtılır:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Azure Güvenlik Duvarı ile uyarı veya uyarı yapılandırma ve reddetme:

https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.3 | 13.3 | Müşteri |

Hassas bilgilerin yetkisiz aktarımı için izleyen ve bilgi güvenliği profesyonellerini uyarırken bu tür aktarımları engelleyen ağ çevrelerine otomatik bir araç dağıtın.

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.4 | 14.4 | Paylaşımlı |

Geçişteki tüm hassas bilgileri şifreleyin. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya daha büyük bir anlaşma sağlayabilmesini sağlayın.

Mümkün olduğu durumlarda, aktarım sırasında istirahat te şifreleme ve şifreleme için Azure Güvenlik Merkezi önerilerine uyun.

Azure ile aktarım sırasında şifrelemeyi anlayın:

https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4,5 | 14.5 | Müşteri |

Azure'daki özel hizmetiniz için hiçbir özellik yoksa, kuruluşun teknoloji sistemleri tarafından depolanan, işlenen veya iletilen tüm hassas bilgileri tanımlamak için bir üçüncü taraf etkin bulma aracını kullanın( ve kuruluşun hassas bilgi envanterini güncelleştirin.

Office 365 belgelerindeki hassas bilgileri tanımlamak için Azure Bilgi Koruması'nı kullanın.

Azure SQL Veritabanlarında depolanan bilgilerin sınıflandırılmasına ve etiketlenmesine yardımcı olmak için Azure SQL Bilgi Koruması'nı kullanın.

Azure SQL Veri Bulma nasıl uygulanır:

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

Azure Bilgi Koruması nasıl uygulanır:

https://docs.microsoft.com/azure/information-protection/deployment-roadmap

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.6 | 14.6 | Müşteri |

Verilere ve kaynaklara erişimi denetlemek için Azure AD RBAC'ı kullanın, aksi takdirde hizmete özel erişim denetim yöntemlerini kullanın.

Azure RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/overview

Azure'da RBAC nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.7 | 14.7 | Müşteri |

Veriler sistemden kopyalandığında bile verilere erişim denetimlerini zorlamak için otomatik ana bilgisayar tabanlı Veri Kaybı Önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

## <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.8 | 14.8 | Müşteri |

Tüm Azure kaynaklarında şifrelemeyi kullanın. Microsoft, Azure'un şifreleme anahtarlarınızı yönetmesine izin vermenizi önerir, ancak bazı durumlarda kendi anahtarlarınızı yönetmeniz seçeneği vardır. 

Azure'da dinlenilerde şifrelemeyi anlayın:

https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest

Müşteri tarafından yönetilen şifreleme anahtarları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 4.9 | 14.9 | Müşteri |

Kritik Azure kaynaklarında ne zaman değişiklikler yapılacağına dair uyarılar oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki güvenlik denetimine bakın: [Güvenlik Açığı Yönetimi](security-control-vulnerability-management.md)
