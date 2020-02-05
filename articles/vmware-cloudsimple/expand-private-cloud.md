---
title: Azure VMware Solutions (AVS) özel bulutunu Genişlet
description: Mevcut bir AVS özel bulutunun var olan veya yeni bir kümede kapasite eklemek için nasıl genişletilmeyeceğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3286b7537056a6c2f282533aa629ebbe47612690
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025308"
---
# <a name="expand-an-avs-private-cloud"></a>AVS özel bulutunu genişletme

AVS, bir AVS özel bulutunu dinamik olarak genişletme esnekliği sağlar. Daha küçük bir yapılandırmayla başlayabilir ve daha yüksek kapasiteye ihtiyaç duyduğunuzda öğesini genişletebilirsiniz. Ya da geçerli ihtiyaçları temel alarak bir AVS özel bulutu oluşturabilir, sonra da tüketim büyüdükçe öğesini genişletebilirsiniz.

Bir AVS özel bulutu bir veya daha fazla vSphere kümelerinden oluşur. Her küme 3 ile 16 arasında düğüme sahip olabilir. Bir AVS özel bulutu genişletirken, var olan kümeye düğüm ekler veya yeni bir küme oluşturun. Var olan bir kümeyi genişletmek için, ek düğümlerin mevcut düğümlerle aynı türde (SKU) olması gerekir. Yeni bir küme oluşturmak için düğümler farklı türde olabilir. AVS özel bulut limitleri hakkında daha fazla bilgi için, [AVS özel buluta genel bakış](cloudsimple-private-cloud.md) makalesindeki sınırlar bölümüne bakın.

Bir AVS özel bulutu, vCenter üzerinde varsayılan bir **veri merkezi** ile oluşturulur. Her veri merkezi en üst düzey yönetim varlığı görevi görür. Yeni bir küme için, AVS, var olan veri merkezine ekleme veya yeni bir veri merkezi oluşturma seçeneği sunar.

Yeni küme yapılandırmasının bir parçası olarak, AVS VMware altyapısını yapılandırır. Bu ayarlar, vSAN disk grupları, VMware yüksek kullanılabilirlik ve dağıtılmış kaynak Zamanlayıcı (DRS) için depolama ayarlarını içerir.

Bir AVS özel bulutu, birden çok kez genişletilebilir. Genişletme, yalnızca genel düğüm sınırları içinde kaldığınızda yapılabilir. Her bir AVS özel bulutu her genişletişinizde var olan kümeye ekler veya yeni bir tane oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

AVS özel bulutunuzu genişletebilmeniz için önce düğümlerin sağlanması gerekir. Düğüm sağlama hakkında daha fazla bilgi için bkz. [AVS tarafından VMware çözümü için düğümleri Sağlama-Azure](create-nodes.md) makalesi. Yeni bir küme oluşturmak için, aynı SKU 'nun en az üç tane kullanılabilir düğümü olmalıdır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="expand-an-avs-private-cloud"></a>AVS özel bulutunu genişletme

1. [AVS portalına erişin](access-cloudsimple-portal.md).

2. **Kaynaklar** sayfasını açın ve GENIŞLETMEK Istediğiniz AVS özel bulutunu seçin.

3. Özet bölümünde **Genişlet**' e tıklayın.

    ![AVS özel bulutunu Genişlet](media/resources-expand-private-cloud.png)

4. Mevcut kümenizi genişletip genişletmeyeceğinizi veya yeni bir vSphere kümesi oluşturmayı seçin. Değişiklik yaparken sayfadaki Özet bilgileri güncelleştirilir.

    * Mevcut kümenizi genişletmek için, **var olan kümeyi Genişlet**' e tıklayın. Genişletmek istediğiniz kümeyi seçin ve eklenecek düğüm sayısını girin. Her küme en fazla 16 düğüm içerebilir.
    * Yeni küme eklemek için **Yeni küme oluştur**' a tıklayın. Küme için bir ad girin. Mevcut bir veri merkezini seçin veya yeni bir veri merkezi oluşturmak için bir ad girin. Düğüm türünü seçin. Yeni bir vSphere kümesi oluştururken farklı bir düğüm türü seçebilirsiniz, ancak var olan bir vSphere kümesini genişlettikten sonra bunu yapabilirsiniz. Düğüm sayısını seçin. Her yeni kümenin en az üç düğümü olmalıdır.

    ![AVS özel bulutu genişletme-düğüm ekleme](media/resources-expand-private-cloud-add-nodes.png)

5. AVS özel bulutunu genişletmek için **Gönder** ' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [AVS özel bulutları](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin