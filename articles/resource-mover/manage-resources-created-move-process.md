---
title: Azure Kaynak taşıyıcısı 'nde VM taşıma işlemi sırasında oluşturulan kaynakları yönetme
description: Azure Kaynak taşıyıcısı 'nde VM taşıma işlemi sırasında oluşturulan kaynakları yönetmeyi öğrenin
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 4ff1905eb164d004af69ce5b0df3278bf3a46884
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89670772"
---
# <a name="manage-resources-created-for-the-vm-move"></a>VM taşıma için oluşturulan kaynakları yönetme

Bu makalede, VM taşıma işlemini kolaylaştırmak için [Azure Kaynak taşıyıcısı](overview.md) tarafından açıkça oluşturulan kaynakların nasıl yönetileceği açıklanır. 

VM 'Leri bölgeler arasında taşıdıktan sonra, kaynak taşıyıcısı tarafından el ile temizlenmesi gereken birkaç kaynak vardır.

## <a name="delete-resources-created-for-vm-move"></a>VM taşıma için oluşturulan kaynakları silme

Taşıma koleksiyonunu el ile silin ve VM taşıma için oluşturulan kaynakları Site Recovery.

1. VM 'Leri taşıdığınız kaynak bölgede, kaynak grubundaki kaynakları gözden geçirin ```RegionMoveRG-<sourceregion>-<target-region>``` .
2. Taşıma koleksiyonundaki VM 'nin ve diğer tüm kaynak kaynaklarının taşındığını/silindiğini kontrol edin. Bu, bunları kullanan beklemedeki kaynaklar olmamasını sağlar.
2. Bu kaynakları silin.

    - Taşıma koleksiyonu adı ```movecollection-<sourceregion>-<target-region>``` .
    - Önbellek depolama hesabı adı ```resmovecache<guid>```
    - Kasa adı ```ResourceMove-<sourceregion>-<target-region>-GUID``` .

## <a name="next-steps"></a>Sonraki adımlar

Kaynak taşıyıcısı ile [BIR VM](tutorial-move-region-virtual-machines.md) 'yi başka bir bölgeye taşımayı deneyin.