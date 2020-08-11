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
ms.date: 08/02/2020
ms.author: memildin
ms.openlocfilehash: 718f9a29b70dab34269c959ccd62452e56a32d72
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056610"
---
# <a name="azure-container-registry-image-scanning-by-security-center"></a>Azure Container Registry resim taramasını Güvenlik Merkezi 'Ne göre

Azure Container Registry (ACR), Azure dağıtımları için kapsayıcı görüntülerinizi merkezi bir kayıt defterinde depolayan ve yöneten, yönetilen ve özel bir Docker kayıt defteri hizmetidir. Bu, açık kaynaklı Docker kayıt defteri 2,0 ' i temel alır.

Azure Güvenlik Merkezi 'nin standart katmanı kullanıyorsanız, kapsayıcı kayıt defterleri paketini ekleyebilirsiniz. Bu isteğe bağlı özellik Azure Resource Manager tabanlı kayıt defterlerinden görüntülerin güvenlik açıklarına daha derin görünürlük getirir. Abonelik düzeyindeki paketi bir abonelikteki tüm kayıt defterlerini kapsayacak şekilde etkinleştirin veya devre dışı bırakın. Bu özellik, [fiyatlandırma sayfasında](security-center-pricing.md)gösterildiği gibi görüntü başına ücretlendirilir. Kapsayıcı kayıt defterleri paketini etkinleştirme, güvenlik merkezi 'nin kayıt defterine gönderilen görüntüleri taramaya hazırlanmasını sağlar. 

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel kullanılabilirlik|
|Fiyat|Standart katmanı|
|Desteklenen kayıt defterleri ve görüntüler:|![](./media/icons/yes-icon.png)Genel İnternet 'ten erişilebilen Linux ile barındırılan ACR kayıt defterleri ve kabuk erişimi sağlar.<br>![](./media/icons/no-icon.png)Windows tarafından barındırılan BIR ACR kayıt defterleri yoktur.<br>![](./media/icons/no-icon.png)' Özel ' kayıt defterleri yok-Güvenlik Merkezi, kayıt defterlerinden genel İnternet 'ten erişilebilmesini gerektirir. Güvenlik Merkezi şu anda bir güvenlik duvarı, hizmet uç noktası veya Azure özel bağlantı gibi özel uç noktalar ile sınırlı erişimli kayıt defterlerine bağlanamaz veya bunları tarayamaz.<br>![](./media/icons/no-icon.png) [Docker karalama](https://hub.docker.com/_/scratch/) görüntüleri gibi süper minimuz görüntüleri veya yalnızca bir uygulama ve çalışma zamanı bağımlılıklarını bir paket yöneticisi, kabuk veya işletim sistemi olmadan Içeren "distrodaha az" görüntüler.|
|Gerekli roller ve izinler:|**Güvenlik okuyucu** ve [Azure Container Registry okuyucu rolü](https://docs.microsoft.com/azure/container-registry/container-registry-roles)|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||




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
[Kullanılabilirlik bölümü](#availability) , kapsayıcı kayıt defterleri paketi tarafından desteklenen kapsayıcı kayıt defterlerinin türlerini listeler. 

Desteklenmeyen kayıt defterleri Azure aboneliğinize bağlı ise, bunlar taranmaz ve bu kişiler için faturalandırılmaz.


## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Kubernetes Service ile tümleştirme](azure-kubernetes-service-integration.md)

* [Sanal makine koruması](security-center-virtual-machine-protection.md) -Güvenlik Merkezi 'nin önerilerini açıklar
