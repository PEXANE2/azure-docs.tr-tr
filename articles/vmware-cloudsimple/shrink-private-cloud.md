---
title: Azure VMware çözümlerini (AVS) özel bulutunu küçültme
description: Bir AVS özel bulutunun nasıl küçültüleceği açıklanır.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0ea764081cd0b4d5c6d44cd7364d1e9a89a3cec3
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014275"
---
# <a name="shrink-an-avs-private-cloud"></a>AVS özel bulutunu daraltma

AVS, bir AVS özel bulutunu dinamik olarak daraltma esnekliği sağlar. Bir AVS özel bulutu bir veya daha fazla vSphere kümelerinden oluşur. Her küme 3 ile 16 arasında düğüme sahip olabilir. Bir AVS özel bulutu daraldığınızda, var olan kümeden bir düğümü kaldırır veya tüm kümeyi silersiniz. 

## <a name="before-you-begin"></a>Başlamadan önce

Bir AVS özel bulutu daralmadan önce aşağıdaki koşulların karşılanması gerekir. Yönetim kümesi (ilk küme), AVS özel bulutu oluşturulduğunda oluşturulur. Silinemez.

* VSphere kümesi üç düğüme sahip olmalıdır. Yalnızca üç düğümü olan bir küme daraltılamaz.
* Tüketilen toplam depolama alanı, kümeyi daraldıktan sonra toplam kapasiteyi aşmamalıdır.
* Dağıtılmış kaynak Zamanlayıcı (DRS) kurallarının sanal bir makinenin vMotion 'ı önlediği olup olmadığını denetleyin. Kurallar varsa, kuralları devre dışı bırakın veya silin. DRS kuralları konak benzeşim kurallarını barındırmak için sanal makineyi içerir.


## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="shrinking-an-avs-private-cloud"></a>AVS özel bulutunu daraltma

1. [AVS portalına erişin](access-cloudsimple-portal.md).

2. **Kaynaklar** sayfasını açın.

3. Küçültmek istediğiniz AVS özel bulutuna tıklayın

4. Özet sayfasında **Küçült**' e tıklayın.

    ![AVS özel bulutunu daraltın](media/shrink-private-cloud.png)

5. Küçültmek veya silmek istediğiniz kümeyi seçin. 

    ![AVS özel bulutunu daraltma-küme Seç](media/shrink-private-cloud-select-cluster.png)

6. **Bir düğüm kaldır** ' ı veya **tüm kümeyi Sil**' i seçin. 

7. Küme kapasitesini doğrulama

8. AVS özel bulutunu daraltmak için **Gönder** ' e tıklayın.

AVS özel bulutun daraltılması başlar. Görevlerin ilerlemesini izleyebilirsiniz. Daraltma işlemi, sanal San 'da yeniden eşitlenmesi gereken verilere bağlı olarak birkaç saat sürebilir.

> [!NOTE]
> 1. Veri merkezinde son veya tek kümeyi silerek özel bir bulutu daraldıysanız, veri merkezi silinmez.
> 2. Herhangi bir DRS kuralı ihlali oluşursa, düğüm kümeden kaldırılmaz ve görev açıklaması, düğüm kaldırma işleminin kümedeki DRS kurallarını ihlal edeceğini gösterir.    


## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [AVS özel bulutları](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin
