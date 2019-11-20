---
title: Azure Güvenlik Merkezi ve Azure Container Registry
description: Azure Güvenlik Merkezi 'nin Azure Container Registry ile tümleştirmesi hakkında bilgi edinin
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/19/2019
ms.author: memildin
ms.openlocfilehash: f2a07774fa23173738bc33907dd00017ca260fd9
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196374"
---
# <a name="azure-container-registry-integration-with-security-center-preview"></a>Güvenlik Merkezi ile tümleştirme Azure Container Registry (Önizleme)

Azure Container Registry (ACR), Azure dağıtımları için kapsayıcı görüntülerinizi merkezi bir kayıt defterinde depolayan ve yöneten, yönetilen ve özel bir Docker kayıt defteri hizmetidir. Bu, açık kaynaklı Docker kayıt defteri 2,0 ' i temel alır.

Kayıt defteriniz ve görüntülerinizin güvenlik açıklarına daha derin görünürlük sağlamak için Azure Güvenlik Merkezi 'nin standart katmanının kullanıcıları isteğe bağlı kapsayıcı kayıt defterleri paketini etkinleştirebilir. Daha fazla bilgi için bkz. [Fiyatlandırma](security-center-pricing.md). Paket etkinken, Güvenlik Merkezi, kayıt defterine her bir görüntü gönderildiğinde ACR hizmetinizdeki görüntüleri otomatik olarak tarar. Tarama tamamlandığında (genellikle yaklaşık 10 dakika sonra), bulgular Güvenlik Merkezi 'nde aşağıdakine benzer önerilere sunulmaktadır:

[Azure Container Registry (ACR) barındırılan görüntüde bulunan güvenlik açıklarına ilişkin ![örnek Azure Güvenlik Merkezi önerisi](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Tümleştirmenin avantajları

Güvenlik Merkezi aboneliğinizdeki ACR kayıt defterlerini tanımlar ve şunları sorunsuzca sağlar:

* Tüm gönderilen Linux görüntüleri için **Azure yerel güvenlik açığı taraması** . Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı, Qualys 'den bir tarayıcı kullanarak görüntüyü tarar. Bu yerel çözüm, varsayılan olarak sorunsuzca tümleşiktir.

* Bilinen güvenlik açıklarına sahip Linux görüntüleri için **güvenlik önerileri** . Güvenlik Merkezi, bildirilen her güvenlik açığının ayrıntılarını ve önem derecesini sağlar. Ayrıca, kayıt defterine gönderilen her görüntüde bulunan belirli güvenlik açıklarının nasıl düzeltileceğine ilişkin yönergeler sağlar.

![Azure Güvenlik Merkezi ve Azure Container Registry (ACR) üst düzey genel bakış](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Kubernetes hizmeti ile tümleştirme](azure-kubernetes-service-integration.md)

* [Sanal makine koruması](security-center-virtual-machine-protection.md) -Güvenlik Merkezi 'nin önerilerini açıklar