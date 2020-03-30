---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 7d7b9f847cdcc4ab4b1ff065425eebe07fb4d888
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775218"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinin altında, aşağıda gösterildiği gibi alanları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Peering Yapılandırma - Exchange](../media/setup-exchange-conf-tab.png)

    * **Peering türü için** *Exchange'i*seçin.
    * *Temel Ücretsiz*olarak **SKU'yı** seçin.
    * Eşlemeyi Azure kaynağına dönüştürmek istediğiniz yer için **Metro** konumunu seçin. Azure kaynağına dönüştürülmeyen seçili **Metro** konumunda Microsoft ile karşılaşbağlantılarınız varsa, bu bağlantılar aşağıda gösterildiği gibi **Eşler bağlantıları** bölümünde listelenir. Artık bu bakan bağlantıları Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![Peering Configuration - Exchange - Eski Bağlantılar](../media/setup-exchange-legacy-conf-tab.png)

        > [!NOTE]
        > Eski bakan bağlantılarıiçin ayarları değiştiremezsiniz. Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek istiyorsanız, **bunu yeni oluştur** düğmesine tıklayarak yapabilirsiniz. Bkz. Daha fazla bilgi için [portalı kullanarak Exchange eşlemi oluştur veya değiştirin.](../howto-exchange-portal.md)
        >

1. Gözden **Geçir + oluştur'a**tıklayın. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığını gözlemleyin. Bu, *son doğrulamayı çalıştırırken*üstteki şeritte görüntülenir... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama Sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Validation Geçti*döndükten sonra, bilgilerinizi doğrulayın ve **Oluştur'a**tıklayarak isteğinizi gönderin. İsteğinizi değiştirmeniz gerekiyorsa, **Önceki'yi** tıklatın ve yukarıdaki adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Gönder](../media/setup-exchange-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımı tamamlamasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Başarılı bir dağıtım aşağıdaki gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Akran Başarısı](../media/setup-direct-success.png)
