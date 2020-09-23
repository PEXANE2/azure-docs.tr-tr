---
title: Azure Güvenlik Merkezi ve Azure Container Registry
description: Azure Güvenlik Merkezi ile kapsayıcı kayıt defterlerinden tarama hakkında bilgi edinin
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 1335b1034304b7efe2b113f7ff2d2927fea41638
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90977356"
---
# <a name="azure-container-registry-image-scanning-by-security-center"></a>Azure Container Registry resim taramasını Güvenlik Merkezi 'Ne göre

Azure Container Registry (ACR), Azure dağıtımları için kapsayıcı görüntülerinizi merkezi bir kayıt defterinde depolayan ve yöneten, yönetilen ve özel bir Docker kayıt defteri hizmetidir. Bu, açık kaynaklı Docker kayıt defteri 2,0 ' i temel alır.

Azure Resource Manager tabanlı kayıt defterlerinden görüntülerin güvenlik açıklarına daha derin görünürlük sağlamak için **Azure Defender 'ı kapsayıcı kayıt defterleri için** etkinleştirin. Abonelik düzeyindeki planı, bir abonelikteki tüm kayıt defterlerini kapsayacak şekilde etkinleştirin veya devre dışı bırakın. Bu özellik, [fiyatlandırma sayfasında](security-center-pricing.md)gösterildiği gibi görüntü başına ücretlendirilir. Azure Defender 'ı etkinleştirme, güvenlik merkezi 'nin kayıt defterine gönderilen görüntüleri taramaya hazırlanmasını sağlar. 


## <a name="when-are-images-scanned"></a>Görüntüler ne zaman taranır?

Kayıt defterinize her görüntü gönderildiğinde, güvenlik merkezi bu görüntüyü otomatik olarak tarar. Bir görüntünün taramasını tetiklemek için onu deponuza gönderin.

Tarama tamamlandığında (genellikle yaklaşık 2 dakika sonra, ancak en fazla 15 dakika), bulgular aşağıdaki gibi güvenlik merkezi önerileri olarak kullanılabilir:

[![Azure Container Registry (ACR) barındırılan görüntüde bulunan güvenlik açıkları hakkında örnek Azure Güvenlik Merkezi önerisi](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Tümleştirmenin avantajları

Güvenlik Merkezi, aboneliğinizdeki Azure Resource Manager tabanlı ACR kayıt defterlerini tanımlar ve şunları sorunsuz şekilde sağlar:

* Tüm gönderilen Linux görüntüleri için **Azure yerel güvenlik açığı taraması** . Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı, Qualys 'den bir tarayıcı kullanarak görüntüyü tarar. Bu yerel çözüm, varsayılan olarak sorunsuzca tümleşiktir.

* Bilinen güvenlik açıklarına sahip Linux görüntüleri için **güvenlik önerileri** . Güvenlik Merkezi, bildirilen her güvenlik açığının ayrıntılarını ve önem derecesini sağlar. Ayrıca, kayıt defterine gönderilen her görüntüde bulunan belirli güvenlik açıklarının nasıl düzeltileceğine ilişkin yönergeler sağlar.

![Azure Güvenlik Merkezi ve Azure Container Registry (ACR) üst düzey genel bakış](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="faq-for-azure-container-registry-image-scanning"></a>Azure Container Registry resim taraması hakkında SSS

### <a name="how-does-security-center-scan-an-image"></a>Güvenlik Merkezi bir görüntüyü nasıl tarar?
Görüntü kayıt defterinden çekilir. Daha sonra, bilinen güvenlik açıklarının listesini ayıklayan Qualys tarayıcısı ile yalıtılmış bir korumalı alanda çalıştırılır.

Güvenlik Merkezi, tarayıcıdan bulguları filtreler ve sınıflandırır. Bir görüntü sağlıklı olduğunda, güvenlik merkezi bunu bu şekilde işaretler. Güvenlik Merkezi yalnızca çözümleme sorunları olan görüntüler için güvenlik önerileri oluşturur. Güvenlik Merkezi, yalnızca sorun olduğunda bilgilendirerek istenmeyen bilgi uyarıları için potansiyelini azaltır.

### <a name="how-often-does-security-center-scan-my-images"></a>Güvenlik Merkezi görüntülerimi ne sıklıkta taraysın?
Yansıma taramaları her gönderim üzerinde tetiklenir.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Tarama sonuçlarını REST API aracılığıyla alabilir miyim?
Evet. Sonuçlar [alt değerlendirmeler REST API 'si](/rest/api/securitycenter/subassessments/list/)altındadır. Ayrıca, tüm kaynaklarınız için kusto benzeri API olan Azure Kaynak Grafiği 'ni (ARG) kullanabilirsiniz: bir sorgu belirli bir taramayı getirebilir.
 
### <a name="what-registry-types-are-scanned-what-types-are-billed"></a>Hangi kayıt defteri türleri taranır? Hangi türler faturalandırılır?
Kullanılabilirlik bölümü, kapsayıcı kayıt defterleri için Azure Defender tarafından desteklenen kapsayıcı kayıt defterlerinin türlerini listeler. 

Desteklenmeyen kayıt defterleri Azure aboneliğinize bağlı ise, bunlar taranmaz ve bu kişiler için faturalandırılmaz.


## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Kubernetes Service ile tümleştirme](azure-kubernetes-service-integration.md)


