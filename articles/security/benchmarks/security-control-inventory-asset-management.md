---
title: Azure Güvenlik Kontrolü - Envanter ve Varlık Yönetimi
description: Güvenlik Kontrol Envanteri ve Varlık Yönetimi
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 2f24012a69a7673426644a8ded6162cd1021938e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75930055"
---
# <a name="security-control-inventory-and-asset-management"></a>Güvenlik Kontrolü: Envanter ve Varlık Yönetimi

Envanter ve Varlık Yönetimi önerileri, yalnızca yetkili kaynaklara erişim verilmesi ve yetkisiz ve yönetilmeyen kaynakların etkin olarak yönetilmesi (envanter, izleme ve doğru) ile ilgili sorunları ele almaya odaklanır tanımlanır ve kaldırılır.

## <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.1 | 1.1, 1.2, 1.3, 1.4, 9.1, 12.1 | Müşteri |

Aboneliğinizdeki tüm kaynakları (bilgi işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Kiracınızda uygun (okundu) izinler sağlayın ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sırala.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Kaynak Grafiği ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/overview

## <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.2 | 1,5 | Müşteri |

Etiketleri, mantıksal olarak taksonomi olarak düzenlemek için meta veriler sağlayan Azure kaynaklarına uygulayın.

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.3 | 1.6 | Müşteri |

Varlıkları düzenlemek ve izlemek için etiketlemeyi, yönetim gruplarını ve uygun olduğunda ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

## <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.4 | 2.1 | Müşteri |

İşlem kaynakları için onaylanmış Azure kaynaklarını ve onaylı yazılımları tanımlayın.

## <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.5 | 2.3, 2.4 | Müşteri |

Aboneliğinizde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure İlkesi'ni kullanın.

Abonelik(ler) içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. &nbsp;Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

## <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.6 | 2.3/2.4 | Müşteri |

Sanal Makinelerdeki tüm yazılımlar hakkındaki bilgilerin toplanmasını otomatikleştirmek için Azure sanal makine Envanteri'ni kullanın. Yazılım Adı, Sürüm, Yayımlama ve Yenileme süresi Azure portalından edinilebilir. Yükleme tarihi ve diğer bilgilere erişmek için konuk düzeyinde tanılamayı etkinleştirin ve Windows Olay Günlükleri'ni bir Log Analytics Çalışma alanına getirin.

Azure sanal makine Envanteri nasıl etkinleştirilir:

https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software

## <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.7 | 2,5 | Müşteri |

Sanal Makinelerde yüklü tüm yazılımları tanımlamak için Azure Güvenlik Merkezi'nin Dosya Bütünlüğü İzleme (Değişiklik İzleme) ve sanal makine Envanterini kullanın. Yetkisiz yazılımı kaldırmak için kendi işleminizi uygulayabilirsiniz. Onaylanmamış yazılımı tanımlamak için üçüncü taraf çözümlerini de kullanabilirsiniz.

Dosya Bütünlüğü İzleme nasıl kullanılır:

https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring#using-file-integrity-monitoring

Azure Değişiklik İzlemeyi Anlayın:

https://docs.microsoft.com/azure/automation/change-tracking

Azure sanal makine envanteri nasıl etkinleştirilir:

https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software

## <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.8 | 2,6 | Müşteri |

Azure Güvenlik Merkezi Uyarlamalı Uygulama Denetimleri'ni kullanarak yalnızca yetkili yazılımın yürütülmesini ve tüm yetkisiz yazılımların Azure Sanal Makinelerde yürütülmesinin engellenmesini sağlayın.

Azure Güvenlik Merkezi Uyarlamalı Uygulama Denetimleri nasıl kullanılır:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-application

## <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.9 | 2,6 | Müşteri |

Ortamınızda hangi hizmetleri sağlayabileceğinizi kısıtlamak için Azure İlkesi'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:

https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

## <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.1 | 2.7 | Müşteri |

Bir kuralın hangi dosya türlerine uygulanabileceğini veya uygulanmayabileceğini belirtmek için Azure Güvenlik Merkezi Uyarlamalı Uygulama Denetimleri'ni kullanın.

Gereksinimi karşılamazsa üçüncü taraf çözümlerini uygulayın.

Azure Güvenlik Merkezi Uyarlamalı Uygulama Denetimleri nasıl kullanılır:

https://docs.microsoft.com/azure/security-center/security-center-adaptive-application

## <a name="611-limit-users-ability-to-interact-with-azure-resource-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla Azure Kaynak Yöneticisi ile etkileşim deşmelerini sınırlandırın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.11 | 2.8 | Müşteri |

&quot;Microsoft Azure Yönetim&quot; &quot;&quot; Uygulaması için Blok erişimini yapılandırarak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişim'i kullanın.

Koşullu Erişim'i Azure Kaynak Yöneticisi'ne erişimi engellemek için yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

## <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.12 | 2.8 | Müşteri |

Kullanıcıların Azure bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırmaları veya üçüncü taraf kaynaklarını kullanın.

Örneğin, Windows Ortamlarında PowerShell komut dosyası yürütmenasıl denetilir:

https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6

## <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 6.13 | 2.9 | Müşteri |

İş işlemleri için gerekli olan ancak kuruluş için daha yüksek risk lere neden olabilecek yazılımlar, kendi sanal makinesi ve/veya sanal ağı içinde izole edilmeli ve bir Azure Güvenlik Duvarı veya Ağ Güvenlik Grubu ile yeterince güvenli olmalıdır.

Sanal ağ nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Nasıl bir güvenlik config ile bir NSG oluşturmak için:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki güvenlik denetimine bakın: [Yapılandırmayı Güvenli](security-control-secure-configuration.md)
