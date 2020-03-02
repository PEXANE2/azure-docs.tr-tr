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
ms.openlocfilehash: 069ce6ca1e76a9bd954031708702c973387abbaa
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78206002"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Güvenlik Merkezi ile Azure Container Registry tümleştirme

Azure Container Registry (ACR), Azure dağıtımları için kapsayıcı görüntülerinizi merkezi bir kayıt defterinde depolayan ve yöneten, yönetilen ve özel bir Docker kayıt defteri hizmetidir. Bu, açık kaynaklı Docker kayıt defteri 2,0 ' i temel alır.

Azure Güvenlik Merkezi 'nin standart katmanı kullanıyorsanız, kapsayıcı kayıt defterleri paketini ekleyebilirsiniz. Bu isteğe bağlı özellik, ARM tabanlı kayıt defterlerinden görüntülerin güvenlik açıklarına daha derin görünürlük getirir. Abonelik düzeyindeki paketi bir abonelikteki tüm kayıt defterlerini kapsayacak şekilde etkinleştirin veya devre dışı bırakın. Bu özellik, [fiyatlandırma sayfasında](security-center-pricing.md)gösterildiği gibi görüntü başına ücretlendirilir. Kapsayıcı kayıt defterleri paketini etkinleştirme, güvenlik merkezi 'nin kayıt defterine gönderilen görüntüleri taramaya hazırlanmasını sağlar. 

Kayıt defterinize her görüntü gönderildiğinde, güvenlik merkezi bu görüntüyü otomatik olarak tarar. Bir görüntünün taramasını tetiklemek için onu deponuza gönderin.

Tarama tamamlandığında (genellikle yaklaşık 10 dakika sonra), bulgular aşağıdaki gibi güvenlik merkezi önerilerinde mevcuttur:

[Azure Container Registry (ACR) barındırılan görüntüde bulunan güvenlik açıklarına ilişkin ![örnek Azure Güvenlik Merkezi önerisi](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Tümleştirmenin avantajları

Güvenlik Merkezi, aboneliğinizdeki ARM tabanlı ACR kayıt defterlerini tanımlar ve şunları sorunsuz şekilde sağlar:

* Tüm gönderilen Linux görüntüleri için **Azure yerel güvenlik açığı taraması** . Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı, Qualys 'den bir tarayıcı kullanarak görüntüyü tarar. Bu yerel çözüm, varsayılan olarak sorunsuzca tümleşiktir.

* Bilinen güvenlik açıklarına sahip Linux görüntüleri için **güvenlik önerileri** . Güvenlik Merkezi, bildirilen her güvenlik açığının ayrıntılarını ve önem derecesini sağlar. Ayrıca, kayıt defterine gönderilen her görüntüde bulunan belirli güvenlik açıklarının nasıl düzeltileceğine ilişkin yönergeler sağlar.

![Azure Güvenlik Merkezi ve Azure Container Registry (ACR) üst düzey genel bakış](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Kubernetes hizmeti ile tümleştirme](azure-kubernetes-service-integration.md)

* [Sanal makine koruması](security-center-virtual-machine-protection.md) -Güvenlik Merkezi 'nin önerilerini açıklar
