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
ms.openlocfilehash: 4cc88e7c04d10907a9a6386b1266eb8031d60926
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552687"
---
# <a name="azure-container-registry-integration-with-security-center-preview"></a>Güvenlik Merkezi ile tümleştirme Azure Container Registry (Önizleme)

Azure Container Registry (ACR), Azure dağıtımları için kapsayıcı görüntülerinizi merkezi bir kayıt defterinde depolayan ve yöneten, yönetilen ve özel bir Docker kayıt defteri hizmetidir. Bu, açık kaynaklı Docker kayıt defteri 2,0 ' i temel alır.

Kayıt defteriniz ve görüntülerinizin güvenlik açıklarına daha derin görünürlük sağlamak için Azure Güvenlik Merkezi 'nin standart katmanının kullanıcıları isteğe bağlı kapsayıcı kayıt defterleri paketini etkinleştirebilir. Bu özelliği kullanma maliyeti, tarama başına değil, görüntü başına ücretlendirilir. Daha fazla bilgi için bkz. [Fiyatlandırma](security-center-pricing.md). Paket etkin olduğunda, kayıt defterine bir görüntü gönderildiğinde Güvenlik Merkezi, kayıt defterinizde bulunan görüntüleri otomatik olarak tarar.

> [!NOTE]
> Güvenlik Merkezi 'nin bir kayıt defterinin ilk taraması, yalnızca kapsayıcı kayıt defterleri paketi etkinleştirildikten ve bir görüntü kayıt defterine gönderildikten sonra gerçekleşir.

Tarama tamamlandığında (genellikle yaklaşık 10 dakika sonra), bulgular aşağıdaki gibi güvenlik merkezi önerilerinde mevcuttur:

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
