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
ms.openlocfilehash: cd51eca0ea4563e1b56f74677df0829669d9e177
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774503"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinin altında, aşağıda gösterildiği gibi alanları doldurun.

    > [!div class="mx-imgBorder"]
    > ![Peering Yapılandırma - Exchange](../media/setup-exchange-conf-tab.png)

    * **Peering türü için** *Exchange'i*seçin.
    * *Temel Ücretsiz*olarak **SKU'yı** seçin.
    * Bakmayı ayarlamak istediğiniz yer için **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten karşılaş bağlantılarınız varsa ve bu konumda ki eşlemi ayarlamak için portalı ilk kez kullanıyorsanız, varolan görünen bağlantılarınız aşağıda gösterildiği gibi **Peering bağlantıları** bölümünde listelenir. Microsoft, yeni bağlantılarla birlikte tek bir yerde yönetebilmeniz için bu bakan bağlantıları otomatik olarak Azure kaynağına dönüştürür. Bkz. Daha fazla bilgi için [portalı kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürün.](../howto-legacy-exchange-portal.md)
        >

1. **Peering bağlantıları**altında, kurmak istediğiniz her yeni bağlantı için bir satır eklemek için **yeni oluştur'u** tıklatın.

    * Bağlantı ayarlarını yapılandırmak/değiştirmek için bir satırın düzenle düğmesini tıklatın.

        > [!div class="mx-imgBorder"]
        > ![Peering Yapılandırma - Exchange Düzenleme](../media/setup-exchange-conf-tab-edit.png)

    * Bir satırı silmek için **Sil'>** **Delete**... düğmesine tıklayın.

        > [!div class="mx-imgBorder"]
        > ![Peering Yapılandırma - Exchange Düzenleme](../media/setup-exchange-conf-tab-delete.png)

    * Aşağıda gösterildiği gibi bir bağlantı için tüm ayarları sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![Peering Yapılandırması - Exchange Bağlantısı](../media/setup-exchange-conf-tab-connection.png)

        1. Bağlantının kurulması gereken **Eşleme tesisini** seçin.
        1. **IPv4 adresi** ve **IPv6 adresi**alanlarında, komşu komutu kullanarak Microsoft yönlendiricilerinde yapılandırılacak iPv4 ve IPv6 adresini sırasıyla girin.
        1. Sırasıyla **Maksimum reklamı yapılan IPv4 adresleri** ve Maksimum reklamı yapılan IPv6 adresleri alanlarında reklamını yapacağınız **IPv4** ve IPv6 önekleri numarasını girin.
        1. Bağlantı ayarlarınızı kaydetmek için **Tamam'ı** tıklatın.

1. Microsoft'un ağınızla birlikte bulunduğu herhangi bir tesise, daha önce seçilen **Metro** içinde daha fazla bağlantı eklemek için yukarıdaki adımı yineleyin.

1. Gerekli tüm bağlantıları ekledikten sonra **Gözden Geçir + oluştur'a**tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Peering Configuration Tab Final](../media/setup-exchange-conf-tab-final.png)

1. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığını gözlemleyin. Bu, *son doğrulamayı çalıştırırken*üstteki şeritte görüntülenir... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama Sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Validation Geçti*döndükten sonra, bilgilerinizi doğrulayın ve **Oluştur'a**tıklayarak isteğinizi gönderin. İsteğinizi değiştirmeniz gerekiyorsa, **Önceki'yi** tıklatın ve yukarıdaki adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Gönder](../media/setup-exchange-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımı tamamlamasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Başarılı bir dağıtım aşağıdaki gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Akran Başarısı](../media/setup-direct-success.png)
