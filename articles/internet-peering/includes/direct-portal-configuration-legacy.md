---
title: dosya dahil etme
titleSuffix: Azure
description: dosya dahil etme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 632490498b8dd13414657edb9518cd543ac07af6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678850"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme sayfası yapılandırma sekmesi oluşturma](../media/setup-direct-conf-tab.png)

    * **Eşleme türü**için **doğrudan**' yi seçin.
    * **Microsoft ağı**için **AS8075**öğesini seçin. ASN 8069 ' i seçmeyin. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft eşlemesi](mailto:peering@microsoft.com)tarafından kullanılır.
    * **SKU 'Yu** **temel ücretsiz**olarak seçin. Premium ücretsiz seçmeyin çünkü özel uygulamalar için ayrılmıştır.
    * Eşlemeyi bir Azure kaynağına dönüştürmek istediğiniz **Metro** konumunu seçin. Azure kaynağına dönüştürülmemiş seçili **Metro** konumunda Microsoft ile eşleme bağlantılarınız varsa, bu bağlantılar gösterilen şekilde **eşleme bağlantıları** bölümünde listelenecektir. Artık bu eşleme bağlantılarını bir Azure kaynağına dönüştürebilirsiniz.

        > [!div class="mx-imgBorder"]
        > ![Eşleme bağlantıları listesi](../media/setup-directlegacy-conf-tab.png)

1. Bant genişliğini güncelleştirmeniz gerekiyorsa, bağlantı ayarlarını değiştirmek için bir çizginin Düzenle düğmesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Düzenle düğmesi](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Seçili **Metro** konumunda Microsoft ile ek eşleme bağlantıları eklemek Istiyorsanız **Yeni oluştur**' u seçin. Daha fazla bilgi için bkz. [portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](../howto-direct-portal.md).
    >

1. **İncele ve oluştur**’u seçin. Portalın girdiğiniz bilgilerin temel doğrulamasını çalıştırdığına dikkat edin. Üstteki bir şerit *son doğrulama çalıştıran iletiyi görüntüler...*.

    > [!div class="mx-imgBorder"]
    > ![Eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, bilgilerinizi doğrulayın. **Oluştur**seçeneğini belirleyerek isteği gönderebilirsiniz. İsteğinizi değiştirmek için, **önceki** seçeneğini belirleyip adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme gönderimi](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın bitmesini bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım burada gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Eşleme başarısı](../media/setup-direct-success.png)
