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
ms.openlocfilehash: 268703081a2a40e8bcc665889eaeaf8edd673bfd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680928"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinde, gösterildiği gibi kutuları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Eşleme sayfası Exchange eşleme türü oluşturma](../media/setup-exchange-conf-tab.png)

    * **Peering türü için** **Exchange'i**seçin.
    * **Temel Ücretsiz**olarak **SKU'yı** seçin.
    * Bakmayı ayarlamak istediğiniz **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten karşılaş bağlantılarınız varsa ve portalı bu konumda eşleme ayarlamak için ilk kez kullanıyorsanız, varolan görünen bağlantılarınız gösterildiği gibi **Eşleme bağlantıları** bölümünde listelenir. Microsoft, yeni bağlantılarla birlikte tek bir yerde yönetebilmeniz için bu bakan bağlantıları otomatik olarak bir Azure kaynağına dönüştürür. Daha fazla bilgi için bkz: [Portalı kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürün.](../howto-legacy-exchange-portal.md)
        >

1. **Peering bağlantıları**altında, kurmak istediğiniz her yeni bağlantı için satır eklemek için **yeni oluştur'u** seçin.

    * Bağlantı ayarlarını yapılandırmak veya değiştirmek için bir satır Için Düzenle düğmesini seçin.

        > [!div class="mx-imgBorder"]
        > ![Düzenle düğmesi](../media/setup-exchange-conf-tab-edit.png)

    * Bir satırı silmek için **, ...**  >  **Sil.**

        > [!div class="mx-imgBorder"]
        > ![Sil düğmesi](../media/setup-exchange-conf-tab-delete.png)

    * Burada gösterildiği gibi, bir bağlantı için tüm ayarları sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![Exchange Peering Connection sayfası](../media/setup-exchange-conf-tab-connection.png)

        1. Bağlantının kurulması gereken **Eşleme tesisini** seçin.
        1. **IPv4 adresi** ve **IPv6 adres** kutularında, komşu komutunu kullanarak Microsoft yönlendiricilerinde yapılandırılacak iPv4 ve IPv6 adreslerini sırasıyla girin.
        1. Sırasıyla **Maksimum reklamı yapılan Maksimum IPv4 adreslerine** ve Maksimum reklamı yapılan **IPv6 adres kutularına** reklam verecekolan IPv4 ve IPv6 önekleri numarasını girin.
        1. Bağlantı ayarlarınızı kaydetmek için **Tamam'ı** seçin.

1. Microsoft'un ağınızla birlikte bulunduğu herhangi bir tesise, daha önce seçilen **Metro** içinde daha fazla bağlantı eklemek için adımı yineleyin.

1. Gerekli tüm bağlantıları ekledikten sonra **Gözden Geçir + oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Yapılandırması sekmesi](../media/setup-exchange-conf-tab-final.png)

1. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığına dikkat edin. Üstteki *şerit, son doğrulamayı çalıştıran*iletiyi görüntüler... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. İleti nin *Doğrulama'ya geçirilmesinden*sonra bilgilerinizi doğrulayın. **Oluştur'u**seçerek isteği gönderin. İsteğinizi değiştirmek için **Önceki'yi** seçin ve adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Akran gönderimi](../media/setup-exchange-review-tab-submit.png)

1. İsteğe göndermeniz inden sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Burada gösterildiği gibi başarılı bir dağıtım görüntülenir.

    > [!div class="mx-imgBorder"]
    > ![Akran başarısı](../media/setup-direct-success.png)
