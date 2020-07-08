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
ms.openlocfilehash: 268703081a2a40e8bcc665889eaeaf8edd673bfd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81680928"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, kutuları gösterilen şekilde girin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme sayfası değişim eşleme türü oluştur](../media/setup-exchange-conf-tab.png)

    * **Eşleme türü**için **Exchange**' i seçin.
    * **SKU 'Yu** **temel ücretsiz**olarak seçin.
    * Eşlemeyi ayarlamak istediğiniz **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten eşleme bağlantılarınız varsa ve bu konumda eşlemeyi ilk kez kullanıyorsanız, mevcut eşleme bağlantılarınız, gösterilen şekilde **eşleme bağlantıları** bölümünde listelenecektir. Microsoft, bu eşleme bağlantılarını, yeni bağlantılarla birlikte tek bir yerden yönetebilmeniz için bir Azure kaynağına otomatik olarak dönüştürür. Daha fazla bilgi için bkz. [portalı kullanarak eski Exchange eşlemesini bir Azure kaynağına dönüştürme](../howto-legacy-exchange-portal.md).
        >

1. **Eşleme bağlantıları**altında, **Yeni oluştur** ' u seçerek ayarlamak istediğiniz her yeni bağlantı için bir satır ekleyin.

    * Bağlantı ayarlarını yapılandırmak veya değiştirmek için, bir satır için Düzenle düğmesini seçin.

        > [!div class="mx-imgBorder"]
        > ![Düzenle düğmesi](../media/setup-exchange-conf-tab-edit.png)

    * Bir satırı silmek için... seçeneğini belirleyin **.**  >  **Silin**.

        > [!div class="mx-imgBorder"]
        > ![Sil düğmesi](../media/setup-exchange-conf-tab-delete.png)

    * Burada gösterildiği gibi, bir bağlantı için tüm ayarları sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![Exchange eşleme bağlantı sayfası](../media/setup-exchange-conf-tab-connection.png)

        1. Bağlantının ayarlanması gereken **eşleme tesis** ' ı seçin.
        1. **IPv4 adresi** ve **IPv6 adresi** kutularına, sırasıyla komşu komutu kullanılarak Microsoft yönlendiricilerde yapılandırılacak IPv4 ve IPv6 adreslerini girin.
        1. **En fazla tanıtılan IPv4 adreslerinde** ve **en yüksek tanıtılan IPv6 adresi** kutularında duyurmanız gereken IPv4 ve IPv6 ön ekleri sayısını girin.
        1. Bağlantı ayarlarınızı kaydetmek için **Tamam ' ı** seçin.

1. Daha önce seçilen **Metro** dahilinde, Microsoft 'un ağınızla birlikte bulunduğu herhangi bir tesiste daha fazla bağlantı eklemek için bu adımı tekrarlayın.

1. Gerekli tüm bağlantıları ekledikten sonra, **gözden geçir + oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme yapılandırması sekmesi](../media/setup-exchange-conf-tab-final.png)

1. Portalın girdiğiniz bilgilerin temel doğrulamasını çalıştırdığına dikkat edin. Üstteki bir şerit *son doğrulama çalıştıran iletiyi görüntüler...*.

    > [!div class="mx-imgBorder"]
    > ![Eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, bilgilerinizi doğrulayın. **Oluştur**seçeneğini belirleyerek isteği gönderebilirsiniz. İsteğinizi değiştirmek için, **önceki** seçeneğini belirleyip adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme gönderimi](../media/setup-exchange-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın bitmesini bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım burada gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Eşleme başarısı](../media/setup-direct-success.png)
