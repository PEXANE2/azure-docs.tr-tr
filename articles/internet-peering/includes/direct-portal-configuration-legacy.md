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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775075"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    > [!div class="mx-imgBorder"]
    > ![eşleme yapılandırması-doğrudan](../media/setup-direct-conf-tab.png)

    * **Eşleme türü**için *doğrudan*' yi seçin.
    * **Microsoft ağı**için *AS8075*öğesini seçin. ASN 8069 ' i seçmeyin. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft eşlemesi](mailto:peering@microsoft.com)tarafından kullanılır.
    * **SKU 'Yu** *temel ücretsiz*olarak seçin. Özel uygulamalar için ayrıldığından *Premium ücretsiz* 'yi seçmeyin.
    * Eşlemeyi Azure kaynağına dönüştürmek istediğiniz **Metro** konumunu seçin. Azure kaynağına dönüştürülmemiş seçili **Metro** konumunda Microsoft ile eşleme bağlantılarınız varsa, bu bağlantılar aşağıda gösterildiği gibi **eşleme bağlantıları** bölümünde listelenecektir. Artık bu eşleme bağlantılarını Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![eşleme yapılandırması-doğrudan eski bağlantılar](../media/setup-directlegacy-conf-tab.png)

1. Bant genişliğini güncelleştirmeniz gerekiyorsa, bağlantı ayarlarını değiştirmek için aşağıda vurgulanan şekilde bir çizginin Düzenle düğmesine tıklayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme yapılandırması-doğrudan düzenleme](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek Istiyorsanız **Yeni oluştur** düğmesine tıklayarak bunu yapabilirsiniz. Daha fazla bilgi için bkz. [portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](../howto-direct-portal.md) .
    >

1. **Gözden geçir + oluştur**seçeneğine tıklayın. Portalın, girdiğiniz bilgilerin temel doğrulamasını çalıştırmasını gözlemleyin. Bu, *son doğrulama çalıştırılırken*en üstteki şeritte görüntülenir....

    > [!div class="mx-imgBorder"]
    > ![eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, **Oluştur**' a tıklayarak bilgilerinizi doğrulayın ve isteği iletin. İsteğinizi değiştirmeniz gerekiyorsa, **önceki** seçeneğine tıklayın ve yukarıdaki adımları tekrarlayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme gönderme](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım aşağıda gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![eşleme başarısı](../media/setup-direct-success.png)
