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
ms.openlocfilehash: b8869e93a7156b24d61ac555c95b9ca7f850ae34
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678532"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinde, burada gösterildiği gibi kutuları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Eşleme sayfası Exchange eşleme türü oluşturma](../media/setup-exchange-conf-tab.png)

    * **Peering türü için** **Exchange'i**seçin.
    * **Temel Ücretsiz**olarak **SKU'yı** seçin.
    * Bir Azure kaynağına eşlemi dönüştürmek istediğiniz **Metro** konumunu seçin. Azure kaynağına dönüştürülmeyen seçili **Metro** konumunda Microsoft ile karşılaş bağlantılarınız varsa, bu bağlantılar gösterildiği gibi **Eşler bağlantıları** bölümünde listelenir. Artık bu bakan bağlantıları bir Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![Eşleme bağlantıları listesi](../media/setup-exchange-legacy-conf-tab.png)

        > [!NOTE]
        > Eski bakan bağlantılarıiçin ayarları değiştiremezsiniz. Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek istiyorsanız, **yeni oluştur'u**seçin. Daha fazla bilgi için [bkz.](../howto-exchange-portal.md)
        >

1. **İncele ve oluştur**’u seçin. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığına dikkat edin. Üstteki *şerit, son doğrulamayı çalıştıran*iletiyi görüntüler... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. İleti nin *Doğrulama'ya geçirilmesinden*sonra bilgilerinizi doğrulayın. **Oluştur'u**seçerek isteği gönderin. İsteğinizi değiştirmeniz gerekiyorsa, **Önceki'yi** seçin ve adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Akran gönderimi](../media/setup-exchange-review-tab-submit.png)

1. İsteğe göndermeniz inden sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Başarılı bir dağıtım gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Akran başarısı](../media/setup-direct-success.png)
