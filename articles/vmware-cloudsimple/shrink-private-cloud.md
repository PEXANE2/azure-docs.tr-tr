---
title: CloudSimple Private Cloud tarafından Azure VMware Çözümlerini Küçültün
description: CloudSimple Private Cloud'un nasıl küçültürülü
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 602dca105e91c55c591388a833a36e71f951da8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77014275"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>Bir CloudSimple Özel Bulutu Küçültün

CloudSimple, Özel Bulut'u dinamik olarak küçültme esnekliği sağlar.  Özel Bulut, bir veya daha fazla vSphere kümesinden oluşur. Her kümede 3 ila 16 düğüm olabilir. Özel Bulut'u küçültterken, varolan kümeden bir düğüm kaldırır veya tüm kümeyi silersiniz. 

## <a name="before-you-begin"></a>Başlamadan önce

Özel Bulut'un küçültülmesi için aşağıdaki koşullar yerine getirilmelidir.  Özel Bulut oluşturulduğunda oluşturulan yönetim kümesi (ilk küme) silinemez.

* Bir vSphere kümesinin üç düğümü olmalıdır.  Yalnızca üç düğümü olan bir küme küçültülemez.
* Tüketilen toplam depolama, kümenin küçüldükten sonraki toplam kapasitesini aşmamalıdır.
* Dağıtılmış Kaynak Zamanlayıcısı (DRS) kurallarının sanal bir makinenin vMotion'ını engelleyip engellemedığını kontrol edin.  Kurallar varsa, kuralları devre dışı edin veya silin.  DRS kuralları, yakınlık kurallarını barındıracak sanal makineyi içerir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="shrink-a-private-cloud"></a>Özel Bulutu küçültme

1. [CloudSimple portalına erişin.](access-cloudsimple-portal.md)

2. **Kaynaklar** sayfasını açın.

3. Küçültmek istediğiniz Özel Bulut'a tıklayın

4. Özet sayfasında **Shrink'i**tıklatın.

    ![Özel bulutu küçültme](media/shrink-private-cloud.png)

5. Küçültmek veya silmek istediğiniz kümeyi seçin. 

    ![Özel bulutu küçültme - küme yi seçin](media/shrink-private-cloud-select-cluster.png)

6. **Bir düğümü kaldır'ı** veya **tüm kümeyi sil'i**seçin. 

7. Küme kapasitesini doğrulama

8. Özel Bulut'u küçültmek için **Gönder'i** tıklatın.

Özel Bulut'un Küçültüşu başlıyor.  Görevlerdeki ilerlemeyi izleyebilirsiniz.  Küçültme işlemi, vSAN üzerinde yeniden senkronize edilmesi gereken verilere bağlı olarak birkaç saat sürebilir.

> [!NOTE]
> 1. Veri merkezindeki son veya tek kümeyi silerek özel bir bulutu küçülterseniz, veri merkezi silinmez.
> 2. Herhangi bir DRS kural ihlali oluşursa, düğüm kümeden kaldırılmaz ve görev açıklaması bir düğümü kaldırmanın kümedeki DRS kurallarını ihlal edeceğini gösterir.    


## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Özel Bulutlar](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin
