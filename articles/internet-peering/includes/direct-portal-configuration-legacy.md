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
ms.openlocfilehash: 16909e0a5d59b0ae019d91aad00e0168b0c5f433
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775075"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinin altında, aşağıda gösterildiği gibi alanları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Peering Yapılandırması - Doğrudan](../media/setup-direct-conf-tab.png)

    * **Peering türü için** *Doğrudan'ı*seçin.
    * **Microsoft ağı için** *AS8075'i*seçin. ASN 8069'u seçmeyin. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft peering](mailto:peering@microsoft.com)tarafından kullanılır.
    * *Temel Ücretsiz*olarak **SKU'yı** seçin. Özel uygulamalar için ayrılmış olduğundan *Premium Free'yi* seçmeyin.
    * Eşlemeyi Azure kaynağına dönüştürmek istediğiniz yer için **Metro** konumunu seçin. Azure kaynağına dönüştürülmeyen seçili **Metro** konumunda Microsoft ile karşılaşbağlantılarınız varsa, bu bağlantılar aşağıda gösterildiği gibi **Eşler bağlantıları** bölümünde listelenir. Artık bu bakan bağlantıları Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![Peering Configuration - Doğrudan - Eski Bağlantılar](../media/setup-directlegacy-conf-tab.png)

1. Bağlantı ayarlarını değiştirmek için bant genişliğini güncelleştirmeniz gerekiyorsa, aşağıda vurgulandığı gibi bir çizginin düzenle düğmesini tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Peering Configuration - Doğrudan Düzenleme](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek istiyorsanız, **bunu yeni oluştur** düğmesine tıklayarak yapabilirsiniz. Bkz. Daha fazla bilgi için [portalı kullanarak Doğrudan bir bakış oluşturma veya değiştirme.](../howto-direct-portal.md)
    >

1. Gözden **Geçir + oluştur'a**tıklayın. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığını gözlemleyin. Bu, *son doğrulamayı çalıştırırken*üstteki şeritte görüntülenir... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama Sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Validation Geçti*döndükten sonra, bilgilerinizi doğrulayın ve **Oluştur'a**tıklayarak isteğinizi gönderin. İsteğinizi değiştirmeniz gerekiyorsa, **Önceki'yi** tıklatın ve yukarıdaki adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Gönder](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımı tamamlamasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Başarılı bir dağıtım aşağıdaki gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Akran Başarısı](../media/setup-direct-success.png)
