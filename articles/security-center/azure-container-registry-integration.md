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
ms.openlocfilehash: 2d588d2707c267097e25176997e58f9573017582
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780054"
---
# <a name="azure-container-registry-integration-with-security-center-preview"></a>Güvenlik Merkezi ile tümleştirme Azure Container Registry (Önizleme)

Azure Container Registry (ACR), Azure dağıtımları için kapsayıcı görüntülerinizi merkezi bir kayıt defterinde depolayan ve yöneten, yönetilen ve özel bir Docker kayıt defteri hizmetidir. Bu, açık kaynaklı Docker kayıt defteri 2,0 ' i temel alır.

Azure Güvenlik Merkezi 'nin standart katmanı kullanıyorsanız, kapsayıcı kayıt defterleri paketini ekleyebilirsiniz. Bu isteğe bağlı özellik, kayıt defterlerinden görüntülerin güvenlik açıklarına daha derin görünürlük getirir. Abonelik düzeyindeki paketi bir abonelikteki tüm kayıt defterlerini kapsayacak şekilde etkinleştirin veya devre dışı bırakın. Bu özellik, [fiyatlandırma sayfasında](security-center-pricing.md)gösterildiği gibi tarama başına değil, görüntü başına ücretlendirilir. 

Kapsayıcı kayıt defterleri paketini etkinleştirme, güvenlik merkezi 'nin kayıt defterine gönderilen görüntüleri taramaya hazırlanmasını sağlar. Taramalar görüntü düzeyindedir: Güvenlik Merkezi kayıt defterinizi taramıyor, kayıt defterinde depolanan görüntüleri tarıyor. 

Kayıt defterinize her görüntü gönderildiğinde, güvenlik merkezi bu görüntüyü otomatik olarak tarar. Bir görüntünün taramasını tetiklemek için onu deponuza gönderin.


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
