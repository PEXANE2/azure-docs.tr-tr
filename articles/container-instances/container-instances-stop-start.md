---
title: Konteyner grubunu el ile durdurma veya başlatma
description: Azure Kapsayıcı Örnekleri'nde bir kapsayıcı grubunu el ile nasıl durdurup başlatıştığınızda öğrenin.
ms.topic: article
ms.date: 04/15/2019
ms.openlocfilehash: c9f8afea33c65df940d02823ec394697d2786d6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533418"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>Azure Kapsayıcı Örnekleri'nde kapsayıcıları el ile durdurma veya başlatma

Kapsayıcı grubunun [yeniden başlatma ilkesi](container-instances-restart-policy.md) ayarı, kapsayıcı örneklerinin varsayılan olarak nasıl başlatılabildiğini veya durdurduğunu belirler. Bir kapsayıcı grubunu el ile durdurarak veya başlatarak varsayılan ayarı geçersiz kılabilirsiniz.

## <a name="stop"></a>Durdur

Çalışan bir kapsayıcı grubunu el ile durdurun - örneğin, [az kapsayıcı durdurma][az-container-stop] komutunu veya Azure portalını kullanarak. Belirli kapsayıcı iş yükleri için, maliyetlerden tasarruf etmek için belirli bir dönemden sonra uzun süren bir kapsayıcı grubunu durdurmak isteyebilirsiniz. 

*Bir kapsayıcı grubu Durduruldu durumuna girdiğinde, gruptaki tüm kapsayıcıları sonlandırır ve geri dönüştürür. Kapsayıcı durumunu korumaz.*

Kapsayıcılar geri dönüştürüldüğünde, [kaynaklar](container-instances-container-groups.md#resource-allocation) ayrılır ve kapsayıcı grubu için faturalandırma durur.

Kapsayıcı grubu zaten sonlandırılmışsa (Başarılı veya Başarısız durumdaysa) durdurma eyleminin bir etkisi yoktur. Örneğin, bir kez çalışan kapsayıcı görevleri olan bir kapsayıcı grubu Başarılı durumda başarıyla sona erer. Bu durumda grup durdurmak için girişimleri durumu değiştirmez. 

## <a name="start"></a>Başlangıç

Bir kapsayıcı grubu durdurulduğunda - kapsayıcılar kendi başlarına sonlandırıldığı için veya grubu el ile durdurduğunuz için - kapsayıcıları başlatabilirsiniz. Örneğin, gruptaki kapsayıcıları el ile başlatmak için [az kapsayıcı başlat][az-container-start] komutunu veya Azure portalını kullanın. Herhangi bir kapsayıcının kapsayıcı görüntüsü güncelleştirilirse, yeni bir görüntü çekilir. 

Kapsayıcı grubunu başlatmak, aynı kapsayıcı yapılandırmasıyla yeni bir dağıtıma başlar. Bu eylem, beklediğiniz gibi çalışan bilinen bir kapsayıcı grubu yapılandırmasını hızla yeniden kullanmanıza yardımcı olabilir. Aynı iş yükünü çalıştırmak için yeni bir kapsayıcı grubu oluşturmanız gerekmez.

Bir kapsayıcı grubundaki tüm kapsayıcılar bu eylem tarafından başlatılır. Grupta belirli bir kapsayıcı başlatamazsınız.

Bir kapsayıcı grubunu el ile başlattıktan veya yeniden başlattıktan sonra, kapsayıcı grubu yapılandırılan yeniden başlatma ilkesine göre çalışır.
  
## <a name="restart"></a>Yeniden Başlatma

Çalışırken bir kapsayıcı grubunu yeniden başlatabilirsiniz - örneğin, [az kapsayıcı yeniden başlatma][az-container-restart] komutunu kullanarak. Bu eylem, kapsayıcı grubundaki tüm kapsayıcıları yeniden başlatır. Herhangi bir kapsayıcının kapsayıcı görüntüsü güncelleştirilirse, yeni bir görüntü çekilir. 

Bir dağıtım sorununu gidermek istediğinizde kapsayıcı grubunu yeniden başlatmak yararlıdır. Örneğin, geçici bir kaynak sınırlaması kapsayıcılarınızın başarılı bir şekilde çalışmasını engelliyorsa, grubu yeniden başlatmak sorunu çözebilir.

Bir kapsayıcı grubundaki tüm kapsayıcılar bu eylem tarafından yeniden başlatılır. Grupta belirli bir kapsayıcıyı yeniden başlatamazsınız.

Bir kapsayıcı grubunu el ile yeniden başlattıktan sonra, kapsayıcı grubu yapılandırılan yeniden başlatma ilkesine göre çalışır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Örnekleri'nde [ilke ayarlarını yeniden başlatma](container-instances-restart-policy.md) hakkında daha fazla bilgi edinin.

Varolan yapılandırmayla bir kapsayıcı grubunu el ile durdurma ve başlatmaya ek olarak, çalışan bir kapsayıcı grubunun [ayarlarını güncelleştirebilirsiniz.](container-instances-update.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop
