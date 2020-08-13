---
title: CloudSimple özel bulutuna sahip bir Azure VMware çözümünü silme
description: CloudSimple özel bulutunu silmeyi öğrenin. Özel bir bulutu sildiğinizde, tüm kümeler silinir.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 11fda35f5b236a4930b3d90eb7e3a62ea60207cf
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88142239"
---
# <a name="delete-a-cloudsimple-private-cloud"></a>CloudSimple özel bulutunu silme

CloudSimple, özel bir bulutu silme esnekliği sağlar.  Bir özel bulut, bir veya daha fazla vSphere kümesinden oluşur. Her küme 3 ile 16 arasında düğüme sahip olabilir. Özel bir bulutu sildiğinizde, tüm kümeler silinir.

## <a name="before-you-begin"></a>Başlamadan önce

Özel bulutun silinmesi, tüm özel bulutun silinmesine neden olacak.  Özel bulutun tüm bileşenleri silinecek.  Verilerin herhangi birini tutmak istiyorsanız, verileri şirket içi depolamaya veya Azure Storage 'a yedeklemiş olduğunuzdan emin olun.

Özel bulutun bileşenleri şunlardır:

* CloudSimple düğümleri
* Sanal makineler
* VLAN’lar/Alt Ağlar
* Özel bulutta depolanan tüm Kullanıcı verileri
* Bir VLAN/subnet 'e yönelik tüm güvenlik duvarı kuralı ekleri

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="delete-a-private-cloud"></a>Özel Bulutu silme

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md).

2. **Kaynaklar** sayfasını açın.

3. Silmek istediğiniz özel buluta tıklayın

4. Özet sayfasında **Sil**' e tıklayın.

    ![Özel bulutu Sil](media/delete-private-cloud.png)

5. Onay sayfasında, özel bulutun adını girin ve **Sil**' e tıklayın. 

    ![Özel bulutu Sil-Onayla](media/delete-private-cloud-confirm.png)

Özel bulut silinmek üzere işaretlendi.  Silme işlemi üç saat sonra başlar ve özel bulutu siler.

> [!CAUTION]
> Özel bulutun silinmesinden sonra düğümlerin silinmesi gerekir.  Düğüm ölçümü, düğümlerin aboneliğinizden silindiği zamana kadar devam edecektir.

## <a name="next-steps"></a>Sonraki adımlar

* [Düğümleri silme](delete-nodes.md)
