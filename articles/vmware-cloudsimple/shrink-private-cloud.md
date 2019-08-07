---
title: Azure VMware çözümünü CloudSimple özel bulutu ile küçültme
description: CloudSimple özel bulutunun nasıl küçültüleceği açıklanır.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 67bb7e0cd8a800c2c2ba87c768739fd573ef6888
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812197"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>CloudSimple özel bulutunu daraltma

CloudSimple, özel bir bulutu dinamik olarak küçültme esnekliği sağlar.  Bir özel bulut, bir veya daha fazla vSphere kümesinden oluşur. Her küme 3 ile 16 arasında düğüme sahip olabilir. Özel bir bulutu daraldığınızda, var olan kümeden bir düğümü kaldırır veya tüm kümeyi silersiniz. 

## <a name="before-you-begin"></a>Başlamadan önce

Özel bulutun küçültülmesi için aşağıdaki koşulların karşılanması gerekir.  Özel bir bulut oluşturulduğunda oluşturulan yönetim kümesi (ilk küme) silinemez.

* VSphere kümesi üç düğüme sahip olmalıdır.  Yalnızca üç düğümü olan bir küme daraltılamaz.
* Tüketilen toplam depolama alanı, kümenin küçültülmeden sonra toplam kapasiteyi aşmamalıdır. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="shrink-a-private-cloud"></a>Özel bulutu daraltma 

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md).

2. **Kaynaklar** sayfasını açın.

3. Küçültmek istediğiniz özel buluta tıklayın

4. Özet sayfasında **Küçült**' e tıklayın.

    ![Özel bulutu Küçült](media/shrink-private-cloud.png)

5. Küçültmek veya silmek istediğiniz kümeyi seçin. 

    ![Özel bulutu Küçült-küme Seç](media/shrink-private-cloud-select-cluster.png)

6. **Bir düğüm kaldır** ' ı veya **tüm kümeyi Sil**' i seçin. 

7. Küme kapasitesini doğrulama

8. Özel bulutu daraltmak için **Gönder** ' e tıklayın.

Özel bulutun küçültülmesi başlar.  Görevlerin ilerlemesini izleyebilirsiniz.  Daraltma işlemi, sanal San 'da yeniden eşitlenmesi gereken verilere bağlı olarak birkaç saat sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Özel bulutlar](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin