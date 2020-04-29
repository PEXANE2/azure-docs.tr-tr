---
title: Azure Güvenlik Merkezi ve Azure Kubernetes hizmeti
description: Azure Güvenlik Merkezi 'nin Azure Kubernetes hizmetleriyle tümleştirmesi hakkında bilgi edinin
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
ms.openlocfilehash: d5c74ac760b5746d37468d692c2a46d5aedbee72
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80436171"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Güvenlik Merkezi ile Azure Kubernetes Hizmetleri Tümleştirmesi

Azure Kubernetes Service (AKS), Kapsayıcılı uygulamalar geliştirmeye, dağıtmaya ve yönetmeye yönelik Microsoft tarafından yönetilen bir hizmettir. 

AKS düğümleriniz, bulut trafiğiniz ve güvenlik denetimlerine daha derin bir görünürlük elde etmek için Azure Güvenlik Merkezi 'nin standart katmanı (bkz. [fiyatlandırma](security-center-pricing.md)) ile birlikte aks kullanın.

Güvenlik Merkezi, aks ana düğümü tarafından zaten toplanan verileri kullanarak AKS kümelerinize yönelik güvenlik avantajları sunar. 

![Azure Güvenlik Merkezi ve Azure Kubernetes hizmeti (AKS) üst düzey genel bakış](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

Bu iki araç birlikte, en iyi bulut Yerel Kubernetes güvenlik sunumunu oluşturur. 

## <a name="benefits-of-integration"></a>Tümleştirmenin avantajları

İki hizmetin birlikte kullanılması aşağıdakileri sağlar:

* **Güvenlik önerileri** : Güvenlik Merkezi, aks kaynaklarınızı tanımlar ve bunları kümeler halinde tek tek sanal makinelere kategorilere ayırır. Ardından, kaynak başına güvenlik önerilerini görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [önerilerin başvuru listesindeki](recommendations-reference.md#recs-containers)kapsayıcı önerileri. 

* **Ortam sağlamlaştırma** -Güvenlik Merkezi, Kubernetes kümelerinizin ve Docker yapılandırmalarının yapılandırmasını sürekli izler. Daha sonra sektör standartlarını yansıtan güvenlik önerileri oluşturur.

* **Çalışma zamanı koruması** -aşağıdaki aks kaynaklarının sürekli analizine sahip Güvenlik Merkezi, ana bilgisayar *ve* aks kümesi düzeyinde algılanan tehditler ve kötü amaçlı etkinlikler hakkında sizi uyarır:
    * Ağ verileri ve işlem oluşturma gibi ham güvenlik olayları
    * Kubernetes denetim günlüğü

    Daha fazla bilgi için bkz. [Azure kapsayıcıları için tehdit koruması](threat-protection.md#azure-containers)

    Olası uyarıların listesi için, uyarılar başvuru tablosundaki şu bölümlere bakın: [aks küme düzeyi uyarıları](alerts-reference.md#alerts-akscluster) ve [kapsayıcı ana bilgisayar düzeyi uyarıları](alerts-reference.md#alerts-containerhost).  

![Azure Güvenlik Merkezi ve Azure Kubernetes hizmeti (AKS) daha ayrıntılı](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)

> [!NOTE]
> Azure Güvenlik Merkezi tarafından Kubernetes ortamınızdan taranan bazı veriler hassas bilgiler içerebilir.


## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Container Registry ile tümleştirme](azure-container-registry-integration.md)

* [Microsoft 'Ta veri yönetimi](https://www.microsoft.com/trust-center/privacy/data-management) -Microsoft hizmetlerinin (Azure, Intune ve Office 365 dahil) veri Ilkelerini, Microsoft 'un veri yönetiminin ayrıntılarını ve verilerinizi etkileyen bekletme ilkelerini açıklar
