---
title: Hata giderme hataları
description: Kaynakları yeni bir kaynak grubuna veya aboneye taşımak için Azure Kaynak Yöneticisi'ni kullanın.
ms.topic: conceptual
ms.date: 08/27/2019
ms.openlocfilehash: 5a65f7daa0f5e3b1c8c6ddfdbecc0ff7d53e5afd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75891270"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Azure kaynaklarını yeni kaynak grubuna veya aboneliğe taşıma sorunlarını giderme

Bu makalede, kaynakları taşınırken sorunları çözmeye yardımcı olmak için öneriler sağlar.

## <a name="upgrade-a-subscription"></a>Aboneliği yükseltme

Azure aboneliğinizi gerçekten yükseltmek istiyorsanız (örneğin, ücretsizden istediğiniz kadar ödemeye geçiş yapmak gibi), aboneliğinizi dönüştürmeniz gerekir.

* Ücretsiz deneme sürümünü yükseltmek [için, Ücretsiz Deneme Sürümü'nüze veya Microsoft Imagine Azure aboneliğinizi Olabildiğince Öde'ye yükseltin'e](../../billing/billing-upgrade-azure-subscription.md)bakın.
* Bir kaldıkça öde hesabını değiştirmek için, [Azure'un You-As-Go aboneliğini farklı bir teklifle değiştir'e](../../billing/billing-how-to-switch-azure-offer.md)bakın.

Aboneliği dönüştüremezseniz, bir [Azure destek isteği oluşturun.](../../azure-portal/supportability/how-to-create-azure-support-request.md) Sorun türü için **Abonelik Yönetimi'ni** seçin.

## <a name="service-limitations"></a>Hizmet sınırlamaları

Bazı hizmetler, kaynakları taşınırken ek hususlar gerektirir. Aşağıdaki hizmetleri taşıyorsanız, kılavuzu ve sınırlamaları kontrol ettiğinizden emin olun.

* [Uygulama Hizmetleri](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Klasik dağıtım modeli](./move-limitations/classic-model-move-limitations.md)
* [Ağ Oluşturma](./move-limitations/networking-move-limitations.md)
* [Kurtarma Hizmetleri](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Virtual Machines](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>Büyük istekler

Mümkün olduğunda, büyük hareketleri ayrı hareket işlemlerine ayırın. Kaynak Yöneticisi, tek bir işlemde 800'den fazla kaynak olduğunda hemen bir hata döndürür. Ancak, 800'den az kaynağın taşınması da zamanlama yla başarısız olabilir.

## <a name="resource-not-in-succeeded-state"></a>Kaynak başarılı durumda değil

Başarılı bir durumda olmadığı için kaynağın taşınamadığını belirten bir hata iletisi aldığınızda, aslında taşımayı engelleyen bağımlı bir kaynak olabilir. Genellikle, hata kodu **MoveCannotProceedWithResourcesNotInSucceededState olduğunu.**

Kaynak veya hedef kaynak grubu sanal bir ağ içeriyorsa, taşıma sırasında sanal ağ için tüm bağımlı kaynakların durumları denetlenir. Denetim, sanal ağa doğrudan ve dolaylı olarak bağımlı olan kaynakları içerir. Bu kaynaklardan herhangi biri başarısız durumdaysa, taşıma engellenir. Örneğin, sanal ağı kullanan sanal bir makine başarısız olduysa, taşıma engellenir. Sanal makine taşınan kaynaklardan biri olmasa ve taşıma için kaynak gruplarından birinde olmasa bile hareket engellenir.

Bu hatayı aldığınızda, iki seçeneğiniz var. Kaynaklarınızı sanal ağı olmayan bir kaynak grubuna taşıyın veya [desteğe başvurun.](../../azure-portal/supportability/how-to-create-azure-support-request.md)

## <a name="next-steps"></a>Sonraki adımlar

Komutların kaynakları taşıması [için](move-resource-group-and-subscription.md)bkz.
