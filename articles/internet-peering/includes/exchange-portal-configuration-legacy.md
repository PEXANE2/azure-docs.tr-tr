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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775218"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    > [!div class="mx-imgBorder"]
    > ![eşleme yapılandırması-Exchange](../media/setup-exchange-conf-tab.png)

    * **Eşleme türü**için *Exchange*' i seçin.
    * **SKU 'Yu** *temel ücretsiz*olarak seçin.
    * Eşlemeyi Azure kaynağına dönüştürmek istediğiniz **Metro** konumunu seçin. Azure kaynağına dönüştürülmemiş seçili **Metro** konumunda Microsoft ile eşleme bağlantılarınız varsa, bu bağlantılar aşağıda gösterildiği gibi **eşleme bağlantıları** bölümünde listelenecektir. Artık bu eşleme bağlantılarını Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![eşleme yapılandırması-Exchange-eski bağlantıları](../media/setup-exchange-legacy-conf-tab.png)

        > [!NOTE]
        > Eski eşleme bağlantılarının ayarlarını değiştiremezsiniz. Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek Istiyorsanız **Yeni oluştur** düğmesine tıklayarak bunu yapabilirsiniz. Daha fazla bilgi için bkz. [portalı kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](../howto-exchange-portal.md) .
        >

1. **Gözden geçir + oluştur**seçeneğine tıklayın. Portalın, girdiğiniz bilgilerin temel doğrulamasını çalıştırmasını gözlemleyin. Bu, *son doğrulama çalıştırılırken*en üstteki şeritte görüntülenir....

    > [!div class="mx-imgBorder"]
    > ![eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, **Oluştur**' a tıklayarak bilgilerinizi doğrulayın ve isteği iletin. İsteğinizi değiştirmeniz gerekiyorsa, **önceki** seçeneğine tıklayın ve yukarıdaki adımları tekrarlayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme gönderme](../media/setup-exchange-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım aşağıda gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![eşleme başarısı](../media/setup-direct-success.png)
