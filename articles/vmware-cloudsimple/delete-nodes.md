---
title: VMware çözümleri (AVS) için düğümleri silme-Azure
description: AVS dağıtımıyla, VMWare 'nizden düğümleri silmeyi öğrenin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 30d128d6bd2f2e1e2705a7b742f02d11fd947a03
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024747"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-avs"></a>AVS tarafından Azure VMware çözümünden düğümleri silme

AVS düğümleri oluşturulduktan sonra ölçümlerdir. Düğümlerin ölçümünü durdurmak için düğümlerin silinmesi gerekir. Azure portal kullanılmayan düğümleri silersiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Düğüm, yalnızca aşağıdaki koşullarda silinebilir:

* Düğümlerle oluşturulmuş bir AVS özel bulutu silinir. Bir AVS özel bulutunu silmek için bkz. [AVS özel bulutu tarafından Azure VMware çözümünü silme](delete-private-cloud.md).
* Düğüm, AVS özel bulutunu daraltarak AVS özel bulutlarından kaldırılmıştır. Bir AVS özel bulutunu daraltmak için bkz. [AVS özel bulutuna göre Azure VMware çözümünü küçültme](shrink-private-cloud.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="delete-avs-node"></a>AVS düğümünü Sil

1. **Tüm Hizmetler**’i seçin.

2. **AVS düğümlerini**arayın.

   ![AVS düğümlerinde ara](media/create-cloudsimple-node-search.png)

3. **AVS düğümlerini**seçin.

4. Silmek için bir AVS özel bulutuna ait olmayan düğümleri seçin. **AVS özel bulut adı** sütunu, bir düğümün AIT olduğu AVS özel bulut adını gösterir. Bir düğüm bir AVS özel bulutu tarafından kullanılmıyorsa, değer boş olur. 

    ![AVS düğümlerini seçin](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Yalnızca AVS özel bulutunun bir parçası olmayan düğümler silinebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [AVS özel bulutu](cloudsimple-private-cloud.md) hakkında bilgi edinin
