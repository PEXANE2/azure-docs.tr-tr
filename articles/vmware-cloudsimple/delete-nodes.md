---
title: CloudSimple tarafından VMware Çözümü için düğümleri silme - Azure
description: CloudSimple dağıtımı ile VMWare'inizdeki düğümleri nasıl silebilirsiniz öğrenin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 122e0636f54e066ae86ed2d19cefe5863b026293
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024747"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-cloudsimple"></a>CloudSimple tarafından Azure VMware Çözümünden düğümleri silme

CloudSimple düğümleri oluşturulduktan sonra ölçülür.  Düğümlerin ölçümlerini durdurmak için düğümlerin silinmesi gerekir.  Azure portalında kullanılmayan düğümleri silersiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bir düğüm yalnızca aşağıdaki koşullar altında silinebilir:

* Düğümlerle oluşturulan Özel Bulut silinir.  Özel Bulut'u silmek için [bkz.](delete-private-cloud.md)
* Düğüm, Özel Bulut küçültilerek Özel Bulut'tan kaldırıldı.  Özel Bulut'u küçültmek için [CloudSimple Private Cloud tarafından Azure VMware Çözümlerini Küçült](shrink-private-cloud.md)ün .

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="delete-cloudsimple-node"></a>CloudSimple düğümünü sil

1. **Tüm Hizmetler**’i seçin.

2. **CloudSimple Düğümleri**arayın.

   ![Arama BulutuBasit Düğümler](media/create-cloudsimple-node-search.png)

3. **CloudSimple Düğümlerini**seçin.

4. Silmek için Özel Bulut'a ait olmayan düğümleri seçin.  **ÖZEL BULUT İsİm** sütunu düğümün ait olduğu Özel Bulut adını gösterir.  Bir düğüm Özel Bulut tarafından kullanılmazsa, değer boş olur. 

    ![CloudSimple Düğümleri seçin](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Yalnızca Özel Bulut'un bir parçası olmayan düğümler silinebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Özel [Bulut](cloudsimple-private-cloud.md) hakkında bilgi edinin
