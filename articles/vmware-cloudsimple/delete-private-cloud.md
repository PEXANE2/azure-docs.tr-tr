---
title: Azure VMware Solutions (AVS) özel bulutunu silme
description: Bir AVS özel bulutunun nasıl silineceğini açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 77cbfb19c3861bac517142f7491e6b1a5fb4ca27
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024764"
---
# <a name="delete-an-avs-private-cloud"></a>AVS özel bulutunu silme

AVS, bir AVS özel bulutunu silme esnekliği sağlar. Bir AVS özel bulutu bir veya daha fazla vSphere kümelerinden oluşur. Her küme 3 ile 16 arasında düğüme sahip olabilir. Bir AVS özel bulutu sildiğinizde, tüm kümeler silinir.

## <a name="before-you-begin"></a>Başlamadan önce

Bir AVS özel bulutu silindiğinde, tüm AVS özel bulutu silinir. AVS özel bulutunun tüm bileşenleri silinecek. Verilerin herhangi birini tutmak istiyorsanız, verileri şirket içi depolamaya veya Azure Storage 'a yedeklemiş olduğunuzdan emin olun.

Bir AVS özel bulutunun bileşenleri şunlardır:

* AVS düğümleri
* Sanal makineler
* VLAN 'Lar/alt ağlar
* AVS özel bulutu 'nda depolanan tüm Kullanıcı verileri
* Bir VLAN/subnet 'e yönelik tüm güvenlik duvarı kuralı ekleri

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="delete-an-avs-private-cloud"></a>AVS özel bulutunu silme

1. [AVS portalına erişin](access-cloudsimple-portal.md).

2. **Kaynaklar** sayfasını açın.

3. Silmek istediğiniz AVS özel bulutuna tıklayın

4. Özet sayfasında **Sil**' e tıklayın.

    ![AVS özel bulutunu silme](media/delete-private-cloud.png)

5. Onay sayfasında, AVS özel bulutunun adını girin ve **Sil**' e tıklayın. 

    ![AVS özel bulutunu silme-Onayla](media/delete-private-cloud-confirm.png)

AVS özel bulutu silinmek üzere işaretlendi. Silme işlemi üç saat sonra başlar ve AVS özel bulutunu siler.

> [!CAUTION]
> AVS özel bulutunun silinmesinden sonra düğümlerin silinmesi gerekir. Düğümler aboneliğinizden silinene kadar düğümlerin ölçümü devam edecektir.

## <a name="next-steps"></a>Sonraki adımlar

* [Düğümleri Sil](delete-nodes.md)
