---
title: Taşıma hatalarını giderme-Azure Resource Manager
description: Kaynakları yeni bir kaynak grubuna veya aboneliğe taşımak için Azure Resource Manager kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/27/2019
ms.author: tomfitz
ms.openlocfilehash: 383098ab227013119bca668b42035c93334c8464
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72533346"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Azure kaynaklarını yeni kaynak grubuna veya aboneliğe taşıma sorunlarını giderme

Bu makalede, kaynakları taşırken oluşan sorunları çözmeye yardımcı olacak öneriler sunulmaktadır.

## <a name="upgrade-a-subscription"></a>Aboneliği yükseltme

Azure aboneliğinizi gerçekten yükseltmek istiyorsanız (örneğin, ücretsiz 'den Kullandıkça Öde 'ye geçiş), aboneliğinizi dönüştürmeniz gerekir.

* Ücretsiz denemeyi yükseltmek için bkz. [ücretsiz deneme sürümünüzü yükseltme veya Azure aboneliğinizi Microsoft Imagine Kullandıkça Öde](../billing/billing-upgrade-azure-subscription.md).
* Kullandıkça Öde hesabını değiştirmek için bkz. [Azure Kullandıkça Öde aboneliğinizi farklı bir teklifine değiştirme](../billing/billing-how-to-switch-azure-offer.md).

Aboneliği dönüştüremiyoruz, [bir Azure destek isteği oluşturun](../azure-supportability/how-to-create-azure-support-request.md). Sorun türü için **Abonelik yönetimi** ' ni seçin.

## <a name="service-limitations"></a>Hizmet sınırlamaları

Bazı hizmetler, kaynakları taşırken ek hususlar gerektirir. Aşağıdaki hizmetleri taşıyorsanız, kılavuz ve sınırlamaları kontrol ettiğinizden emin olun.

* [Uygulama Hizmetleri](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Klasik dağıtım modeli](./move-limitations/classic-model-move-limitations.md)
* [Ağ](./move-limitations/networking-move-limitations.md)
* [Kurtarma Hizmetleri](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Sanal Makineler](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>Büyük istekler

Mümkün olduğunda, büyük taşıma işlemlerini ayrı olarak bölün. Tek bir işlemde 800 'den fazla kaynak olduğunda hemen Kaynak Yöneticisi bir hata döndürür. Ancak, 800 'den az kaynak taşıma zaman aşımına uğramasından de başarısız olabilir.

## <a name="resource-not-in-succeeded-state"></a>Kaynak başarılı değil durumunda

Kaynak başarılı bir durumda olmadığından, bir kaynağın taşınamayacağını belirten bir hata iletisi aldığınızda, gerçekten taşımayı engelleyen bir bağımlı kaynak olabilir. Genellikle, hata kodu **MoveCannotProceedWithResourcesNotInSucceededState**' dir.

Kaynak veya hedef kaynak grubu bir sanal ağ içeriyorsa, sanal ağ için tüm bağımlı kaynakların durumları taşıma sırasında denetlenir. Denetim, bu kaynakları doğrudan ve sanal ağa dolaylı olarak bağlı olarak içerir. Bu kaynaklardan herhangi biri başarısız durumdaysa taşıma engellenir. Örneğin, sanal ağ kullanan bir sanal makine başarısız olduysa, taşıma engellenir. Sanal makine, taşınmakta olan kaynaklardan biri olmadığında ve taşıma için kaynak gruplarından birinde yer alsa bile taşıma engellenir.

Bu hatayı aldığınızda, iki seçeneğiniz vardır. Kaynaklarınızı sanal ağı olmayan bir kaynak grubuna taşıyın veya [desteğe başvurun](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutları için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
