---
title: Azure Güvenlik Merkezi ve Azure Kubernetes Hizmeti
description: Azure Güvenlik Merkezi'nin Azure Kubernetes Hizmetleri yle tümleştirmesi hakkında bilgi edinin
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: d1cd4691586b27282d221a19c5fb7a1af034ed6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125153"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Azure Kubernetes Hizmetleri güvenlik merkezi ile tümleştirme

Azure Kubernetes Service (AKS), Microsoft'un kapsayıcı laştırılmış uygulamaları geliştirmek, dağıtmak ve yönetmek için yönetilen hizmetidir. 

AKS düğümleriniz, bulut trafiğiniz ve güvenlik denetimlerinizde daha fazla görünürlük elde etmek için AKS'yi Azure Güvenlik Merkezi'nin standart katmanıyla birlikte [(fiyatlandırmaya](security-center-pricing.md)bakın) kullanın.

Güvenlik Merkezi, AKS ana düğümü tarafından toplanan verileri kullanarak AKS kümelerinize güvenlik avantajları sağlar. 

![Azure Güvenlik Merkezi ve Azure Kubernetes Hizmeti (AKS) üst düzey genel bakış](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

Bu iki araç birlikte en iyi bulut yerlisi Kubernetes güvenlik teklifini oluşturur. 

## <a name="benefits-of-integration"></a>Entegrasyonun faydaları

İki hizmeti birlikte kullanmak şunları sağlar:

* **Güvenlik önerileri** - Güvenlik Merkezi AKS kaynaklarınızı tanımlar ve kategorilere ayırAr: kümelerden tek tek sanal makinelere kadar. Daha sonra kaynak başına güvenlik önerilerini görüntüleyebilirsiniz. Daha fazla bilgi için, [önerilerin başvuru listesinde](recommendations-reference.md#recs-computeapp)kapsayıcı önerilerine bakın. 

* **Çevre sertleştirme** - Güvenlik Merkezi, Kubernetes kümelerinizin ve Docker yapılandırmalarınızın yapılandırmasını sürekli olarak izler. Daha sonra endüstri standartlarını yansıtan güvenlik önerileri oluşturur.

* **Çalışma süresi koruması** - Aşağıdaki AKS kaynaklarının sürekli analizi yle, Güvenlik Merkezi sizi ana bilgisayar ve AKS küme düzeyinde algılanan *tehditlere* ve kötü amaçlı faaliyetlere karşı uyarır:
    * Ağ verileri ve işlem oluşturma gibi ham güvenlik olayları
    * Kubernetes denetim günlüğü

    Daha fazla bilgi için [Azure kapsayıcıları için tehdit koruması](threat-protection.md#azure-containers)

    Olası uyarıların listesi için, uyarılar başvuru tablosunda şu bölümlere bakın: [AKS küme düzeyi uyarıları](alerts-reference.md#alerts-akscluster) ve [Kapsayıcı ana bilgisayar düzeyi uyarıları.](alerts-reference.md#alerts-containerhost)  

![Azure Güvenlik Merkezi ve Azure Kubernetes Hizmeti (AKS) daha ayrıntılı olarak](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)

> [!NOTE]
> Azure Güvenlik Merkezi tarafından Kubernetes ortamınızdan taranan bazı veriler hassas bilgiler içerebilir.


## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Container Registry ile tümleştirme](azure-container-registry-integration.md)

* [Microsoft'ta veri yönetimi](https://www.microsoft.com/trust-center/privacy/data-management) - Microsoft hizmetlerinin veri ilkelerini (Azure, Intune ve Office 365 dahil) açıklar, Microsoft'un veri yönetiminin ayrıntılarını ve verilerinizi etkileyen bekletme ilkelerini açıklar