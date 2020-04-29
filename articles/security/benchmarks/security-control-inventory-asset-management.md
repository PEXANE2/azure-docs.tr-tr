---
title: Azure Güvenlik denetimi-envanter ve varlık yönetimi
description: Azure Güvenlik denetimi envanteri ve varlık yönetimi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: aece7278d134145e00b027c184f3817d5566e1e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81408375"
---
# <a name="security-control-inventory-and-asset-management"></a>Güvenlik denetimi: envanter ve varlık yönetimi

Envanter ve varlık yönetimi önerileri, yalnızca yetkili kaynaklara erişim izni verilmesi ve yetkisiz ve yönetilmeyen kaynakların tanımlanması ve kaldırılması için tüm Azure kaynaklarını etkin bir şekilde yönetme (envanter, izleme ve düzeltme) ile ilgili sorunları gidermeye odaklanmaktadır.

## <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.1 | 1,1, 1,2, 1,3, 1,4, 9,1, 12,1 | Müşteri |

Abonelikleriniz içindeki tüm kaynakları (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/öğrenmek için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

- [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

- [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

## <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.2 | 1,5 | Müşteri |

Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.3 | 1.6 | Müşteri |

Varlıkları organize etmek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.4 | 2.1 | Müşteri |

Kurumsal gereksinimleriniz uyarınca, işlem kaynakları için onaylanan Azure kaynakları ve onaylanan yazılım envanterini oluşturun.

## <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.5 | 2,3, 2,4 | Müşteri |

Abonelikleriniz içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

## <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.6 | 2,3, 2,4 | Müşteri |

Sanal makinelerdeki tüm yazılımlarla ilgili bilgi toplamayı otomatik hale getirmek için Azure sanal makine envanterini kullanın. Yazılım adı, sürüm, yayımcı ve yenileme süresi Azure portal kullanılabilir. Yüklemeyi tarih ve diğer bilgilere erişim sağlamak için, Konuk düzeyinde tanılamayı etkinleştirin ve Windows olay günlüklerini Log Analytics çalışma alanına getirin.

- [Azure sanal makine envanterini etkinleştirme](https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software)

## <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.7 | 2,5 | Müşteri |

Sanal makinelerde yüklü olan tüm yazılımları belirlemek için Azure Güvenlik Merkezi 'nin dosya bütünlüğü Izleme (Değişiklik İzleme) ve sanal makine envanterini kullanın. Yetkisiz yazılımı kaldırmak için kendi işleminizi uygulayabilirsiniz. Onaylanmamış yazılımları belirlemek için bir üçüncü taraf çözümü de kullanabilirsiniz.

- [Dosya bütünlüğü Izlemeyi kullanma](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring#using-file-integrity-monitoring)

- [Azure Değişiklik İzleme anlama](https://docs.microsoft.com/azure/automation/change-tracking)

- [Azure sanal makine envanterini etkinleştirme](https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software)

## <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6.8 | 2,6 | Müşteri |

Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanarak yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellediği emin olun.

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

## <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6,9 | 2,6 | Müşteri |

Ortamınızda sağlayabileceğiniz hizmetleri kısıtlamak için Azure Ilkesini kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

## <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6,10 | 2.7 | Müşteri |

Bir kuralın hangi dosya türlerini uygulayabileceği veya uygulanmayabilir belirtmek için Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanın.

Bu gereksinimi karşılamazsa üçüncü taraf çözümünü uygulayın.

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

## <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6,11 | 2.9 | Müşteri |

"Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisi ile etkileşime geçmesini sınırlamak için Azure koşullu erişimi kullanın.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

## <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6,12 | 2.9 | Müşteri |

Betiklerin türüne bağlı olarak, kullanıcıların Azure işlem kaynakları içinde betikleri yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanabilirsiniz.  Ayrıca, Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerinden yararlanarak yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellemiş olmasını sağlayabilirsiniz.

- [Windows ortamlarında PowerShell betiği yürütmeyi denetleme](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

## <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 6,13 | 2.9 | Müşteri |

İşletme işlemleri için gerekli olan, ancak kuruluşa daha fazla risk doğurabilecek yazılımlar, kendi sanal makinesi ve/veya sanal ağı içinde yalıtılmalı ve bir Azure Güvenlik Duvarı veya ağ güvenlik grubu ile yeterince güvenmelidir.

- [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

- [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [güvenli yapılandırma](security-control-secure-configuration.md)