---
title: Azure Güvenlik denetimi-veri koruma
description: Güvenlik denetimi veri koruması
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 5482495f87e87e5d05d8adca6b053810a62dcb4e
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934514"
---
# <a name="security-control-data-protection"></a>Güvenlik denetimi: veri koruma

Veri koruma önerileri şifreleme, erişim denetim listeleri, kimlik tabanlı erişim denetimi ve veri erişimi için denetim günlüğü ile ilgili sorunları gidermeye odaklanmaktadır.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.1 | 13,1 | Müşteri |

Gizli bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.2 | 13.2 | Müşteri |

Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar VNet/subnet ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir NSG veya Azure güvenlik duvarıyla korunmuş olmalıdır. Hassas verileri depolayan veya işleyen kaynaklar yeterince yalıtılmalıdır. Hassas verileri depolayan veya işleyen sanal makineler için, kullanımda olmadığında devre dışı bırakmak üzere ilke ve yordam uygulayın.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal ağ oluşturma:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Yapılandırması ile NSG oluşturma:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Azure Güvenlik duvarını dağıtma:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal

Uyarı veya uyarı yapılandırma ve Azure Güvenlik Duvarı ile reddetme:

https://docs.microsoft.com/azure/firewall/threat-intel

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.3 | 13,3 | Müşteri |

Hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir otomatik araç dağıtın.

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.4 | 14,4 | Paylaşılan |

Yoldaki tüm hassas bilgileri şifreleyin. Azure kaynaklarınıza bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun.

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın.

Azure ile iletim sırasında şifrelemeyi anlayın:

https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4,5 | 14,5 | Müşteri |

Azure 'daki belirli bir hizmet için kullanılabilir özellik olmadığında, şirket içinde bulunan, işlenen veya Şirket içindeki teknoloji sistemleri tarafından aktarılan tüm hassas bilgileri (yerinde veya bir uzak hizmet sağlayıcısı ve kuruluşun hassas bilgi envanterini güncelleştirin.

Office 365 belgelerindeki önemli bilgileri tanımlamak için Azure Information Protection kullanın.

Azure SQL veritabanlarında depolanan bilgilerin sınıflandırmasına ve etiketlemesine yardımcı olması için Azure SQL Information Protection kullanın.

Azure SQL veri bulmayı uygulama:

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

Azure Information Protection nasıl uygulanır:

https://docs.microsoft.com/azure/information-protection/deployment-roadmap

## <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.6 | 14,6 | Müşteri |

Veri ve kaynaklara erişimi denetlemek için Azure AD RBAC kullanın, aksi takdirde hizmete özel erişim denetimi yöntemlerini kullanın.

Azure RBAC 'yi anlama:

https://docs.microsoft.com/azure/role-based-access-control/overview

Azure 'da RBAC 'yi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.7 | 14,7 | Müşteri |

Verileri bir sistemden kopyalandıklarında bile verilere erişim denetimlerine zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

## <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4.8 | 14,8 | Müşteri |

Tüm Azure kaynaklarında bekleyen şifreleme kullanın. Microsoft, Azure 'un şifreleme anahtarlarınızı yönetmesine izin vermesini önerir, ancak bazı örneklerde kendi anahtarlarınızı yönetmeniz için seçenek vardır. 

Azure 'da bekleyen şifrelemeyi anlayın:

https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest

Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 4,9 | 14,9 | Müşteri |

Azure Izleyici 'yi Azure etkinlik günlüğü ile birlikte kullanarak, önemli Azure kaynaklarına yapılan değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

Azure etkinlik günlüğü olayları için uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

## <a name="next-steps"></a>Sonraki adımlar

Sonraki güvenlik denetimine bakın: [güvenlik açığı yönetimi](security-control-vulnerability-management.md)
