---
title: Azure Güvenlik Merkezi ve Azure Kapsayıcı Kayıt Defteri
description: Azure Güvenlik Merkezi'nin Azure Kapsayıcı Kayıt Defteri ile tümleştirmesi hakkında bilgi edinin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206002"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Güvenlik Merkezi ile Azure Kapsayıcı Kayıt Defteri entegrasyonu

Azure Kapsayıcı Kayıt Defteri (ACR), azure dağıtımları için konteyner resimlerinizi merkezi bir kayıt defterinde depolayan ve yöneten yönetilen, özel bir Docker kayıt hizmetidir. Açık kaynak Docker Registry 2.0'a dayanıyor.

Azure Güvenlik Merkezi'nin standart katmanındaysanız, Konteyner Kayıt Defterleri paketini ekleyebilirsiniz. Bu isteğe bağlı özellik, ARM tabanlı kayıt defterlerinizdeki görüntülerin güvenlik açıklarına daha fazla görünürlük sağlar. Abonelikteki tüm kayıtları kapsayacak şekilde paketi abonelik düzeyinde etkinleştirin veya devre dışı edin. Bu özellik, [fiyatlandırma sayfasında](security-center-pricing.md)gösterildiği gibi görüntü başına ücretlendirilir. Kapsayıcı Kayıt Defterleri paketini etkinleştirmek, Güvenlik Merkezi'nin kayıt defterine itilen görüntüleri taraya hazır olmasını sağlar. 

Bir görüntü kayıt defterinize itildiğinde, Güvenlik Merkezi bu görüntüyü otomatik olarak tarar. Görüntünün teşbini tetiklemek için deponuza itin.

Taramalar tamamlandığında (genellikle yaklaşık 10 dakika sonra), bulgular Güvenlik Merkezi önerilerinde şu gibi kullanılabilir:

[![Azure Kapsayıcı Kayıt Defteri'nde (ACR) barındırılan görüntüde bulunan güvenlik açıkları hakkında örnek Azure Güvenlik Merkezi önerisi](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Entegrasyonun faydaları

Güvenlik Merkezi, aboneliğinizdeki ARM tabanlı ACR kayıt defterlerini tanımlar ve sorunsuz bir şekilde sağlar:

* Tüm itilen Linux görüntüleri için **Azure'da yerel güvenlik açığı taraması.** Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı Qualys'in tarayıcısını kullanarak görüntüyü tarar. Bu yerel çözüm varsayılan olarak sorunsuz bir şekilde tümleştirilir.

* Bilinen güvenlik açıklarına sahip Linux görüntüleri için **güvenlik önerileri.** Güvenlik Merkezi, bildirilen her güvenlik açığının ayrıntılarını ve önem derecesini sınıflamayı sağlar. Ayrıca, kayıt defterine itilen her görüntüde bulunan belirli güvenlik açıklarının nasıl düzeltileceğine ilişkin rehberlik sağlar.

![Azure Güvenlik Merkezi ve Azure Kapsayıcı Kayıt Defteri (ACR) üst düzey genel bakış](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Kubernetes Service ile tümleştirme](azure-kubernetes-service-integration.md)

* [Sanal Makine koruması](security-center-virtual-machine-protection.md) - Güvenlik Merkezi'nin önerilerini açıklar
