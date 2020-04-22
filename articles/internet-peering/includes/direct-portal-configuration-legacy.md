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
ms.openlocfilehash: 632490498b8dd13414657edb9518cd543ac07af6
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678850"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinde, burada gösterildiği gibi kutuları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Eşleme sayfası Yapılandırma sekmesi oluşturma](../media/setup-direct-conf-tab.png)

    * **Peering türü için** **Doğrudan'ı**seçin.
    * **Microsoft ağı için** **AS8075'i**seçin. ASN 8069'u seçmeyin. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft peering](mailto:peering@microsoft.com)tarafından kullanılır.
    * **Temel Ücretsiz**olarak **SKU'yı** seçin. Özel uygulamalar için ayrılmış olduğundan Premium Free'i seçmeyin.
    * Bir Azure kaynağına eşlemi dönüştürmek istediğiniz **Metro** konumunu seçin. Azure kaynağına dönüştürülmeyen seçili **Metro** konumunda Microsoft ile karşılaş bağlantılarınız varsa, bu bağlantılar gösterildiği gibi **Eşler bağlantıları** bölümünde listelenir. Artık bu bakan bağlantıları bir Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![Eşleme bağlantıları listesi](../media/setup-directlegacy-conf-tab.png)

1. Bant genişliğini güncelleştirmeniz gerekiyorsa, bağlantı ayarlarını değiştirmek için bir satırIçin düzenle düğmesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Düzenle düğmesi](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek istiyorsanız, **yeni oluştur'u**seçin. Daha fazla bilgi için [bkz.](../howto-direct-portal.md)
    >

1. **İncele ve oluştur**’u seçin. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığına dikkat edin. Üstteki *şerit, son doğrulamayı çalıştıran*iletiyi görüntüler... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. İleti nin *Doğrulama'ya geçirilmesinden*sonra bilgilerinizi doğrulayın. **Oluştur'u**seçerek isteği gönderin. İsteğinizi değiştirmek için **Önceki'yi** seçin ve adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Akran gönderimi](../media/setup-direct-review-tab-submit.png)

1. İsteğe göndermeniz inden sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Burada gösterildiği gibi başarılı bir dağıtım görüntülenir.

    > [!div class="mx-imgBorder"]
    > ![Akran başarısı](../media/setup-direct-success.png)
