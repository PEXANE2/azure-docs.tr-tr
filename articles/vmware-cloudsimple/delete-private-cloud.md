---
title: CloudSimple Private Cloud tarafından bir Azure VMware Çözümünü silme
description: CloudSimple Private Cloud'un nasıl silinir olduğunu açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6bc3e7030c500ea2d6072a1cce0f0b3d9fc62801
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024764"
---
# <a name="delete-a-cloudsimple-private-cloud"></a>Bir CloudBasit Özel Bulutu Silme

CloudSimple, Özel Bulut'u silme esnekliği sağlar.  Özel Bulut, bir veya daha fazla vSphere kümesinden oluşur. Her kümede 3 ila 16 düğüm olabilir. Bir Özel Bulut'u sildiğinizde, tüm kümeler silinir.

## <a name="before-you-begin"></a>Başlamadan önce

Özel Bulut'un silinmesi, Tüm Özel Bulut'u siler.  Özel Bulut'un tüm bileşenleri silinir.  Verilerden herhangi birini saklamak istiyorsanız, verileri şirket içi depolama veya Azure depolama sına yedeklediğinizden emin olun.

Özel Bulut'un bileşenleri şunlardır:

* CloudSimple Düğümleri
* Sanal makineler
* VLAN’lar/Alt Ağlar
* Özel Bulut'ta depolanan tüm kullanıcı verileri
* VLAN/Subnet'e tüm güvenlik duvarı kuralı ekleri

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="delete-a-private-cloud"></a>Özel Bulutu silme

1. [CloudSimple portalına erişin.](access-cloudsimple-portal.md)

2. **Kaynaklar** sayfasını açın.

3. Silmek istediğiniz Özel Bulut'a tıklayın

4. Özet sayfasında **Sil'i**tıklatın.

    ![Özel bulutu silme](media/delete-private-cloud.png)

5. Onay sayfasında, Özel Bulut'un adını girin ve **Sil'i**tıklatın. 

    ![Özel bulutu silme - onaylayın](media/delete-private-cloud-confirm.png)

Özel Bulut silme için işaretlenir.  Silme işlemi üç saat sonra başlar ve Özel Bulut'u siler.

> [!CAUTION]
> Özel Bulut silindikten sonra düğümler silinmelidir.  Düğümlerin ölçümü, düğümler aboneliğinizden silinene kadar devam eder.

## <a name="next-steps"></a>Sonraki adımlar

* [Düğümleri silme](delete-nodes.md)
