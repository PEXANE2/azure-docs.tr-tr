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
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: 7f43f50fc35b5ecbc9720224036ea13e2ba753f5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90894929"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Güvenlik Merkezi ile Azure Kubernetes Hizmetleri Tümleştirmesi

Azure Kubernetes Service (AKS), Kapsayıcılı uygulamalar geliştirmeye, dağıtmaya ve yönetmeye yönelik Microsoft tarafından yönetilen bir hizmettir. 

**Kubernetes Için Azure Defender** 'ı etkinleştirerek aks düğümleriniz, bulut trafiğiniz ve güvenlik denetimleriniz hakkında daha ayrıntılı bir görünürlük elde edin.

Bir arada, güvenlik merkezi ve AKS, en iyi bulut Yerel Kubernetes güvenlik teklifini oluşturur.

## <a name="what-are-the-components-of-security-centers-kubernetes-protection"></a>Güvenlik Merkezi 'nin Kubernetes korumasının bileşenleri nelerdir?

Kubernetes için Güvenlik Merkezi 'nin korumaları iki öğe birleşimi tarafından sağlanır:

- **Azure Güvenlik Merkezi 'nin sanal makineler için tehdit koruması** -Güvenlik Merkezi 'Nin diğer VM 'lerde kullandığı aynı Log Analytics aracısını kullanarak, güvenlik merkezi aks düğümleriniz üzerinde oluşan güvenlik sorunlarını gösterebilir. Aracı Ayrıca kapsayıcıya özgü analiz için de izler.

- **Azure Güvenlik Merkezi 'Nin Kubernetes planı için isteğe bağlı Azure Defender** -Kubernetes planı, aks hizmeti aracılığıyla Kubernetes alt sisteminden Günlükler ve bilgiler alır. Bu Günlükler Azure 'da AKS hizmeti aracılığıyla zaten kullanılabilir. Kubernetes için Azure Defender 'ı etkinleştirdiğinizde, günlüklere güvenlik merkezi erişimi verirsiniz. Bu nedenle, güvenlik merkezi aks ana düğümü tarafından zaten toplanan verileri kullanarak AKS kümelerinize yönelik güvenlik avantajları sunar. Azure Güvenlik Merkezi tarafından Kubernetes ortamınızdan taranan bazı veriler hassas bilgiler içerebilir.


## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Container Registry ile tümleştirme](azure-container-registry-integration.md)

* [Microsoft 'Ta veri yönetimi](https://www.microsoft.com/trust-center/privacy/data-management) -Microsoft hizmetlerinin (Azure, ıntune ve Microsoft 365 dahil) veri Ilkelerini, Microsoft 'un veri yönetimiyle ilgili ayrıntıları ve verilerinizi etkileyen bekletme ilkelerini açıklar
