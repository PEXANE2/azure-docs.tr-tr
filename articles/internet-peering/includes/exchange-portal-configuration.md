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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774503"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    > [!div class="mx-imgBorder"]
    > ![eşleme yapılandırması-Exchange](../media/setup-exchange-conf-tab.png)

    * **Eşleme türü**için *Exchange*' i seçin.
    * **SKU 'Yu** *temel ücretsiz*olarak seçin.
    * Eşlemeyi ayarlamak istediğiniz **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten eşleme bağlantılarınız varsa ve bu konumda eşleme ayarlamak için Portal kullanıyorsanız, mevcut eşleme bağlantılarınız aşağıda gösterildiği gibi **eşleme bağlantıları** bölümünde listelenecektir. Microsoft bu eşleme bağlantılarını, yeni bağlantılarla birlikte tek bir yerde yönetebilmeniz için Azure kaynağına otomatik olarak dönüştürecek. Daha fazla bilgi için bkz. [portalı kullanarak eski Exchange eşlemesini Azure kaynağına dönüştürme](../howto-legacy-exchange-portal.md) .
        >

1. **Eşleme bağlantıları**altında, kurmak istediğiniz her yeni bağlantı için bir satır eklemek üzere **Yeni oluştur** ' a tıklayın.

    * Bağlantı ayarlarını yapılandırmak/değiştirmek için bir satır için Düzenle düğmesine tıklayın.

        > [!div class="mx-imgBorder"]
        > ![eşleme yapılandırması-Exchange düzenleme](../media/setup-exchange-conf-tab-edit.png)

    * Bir satırı silmek için, **sil**>. **..** düğmesine tıklayın.

        > [!div class="mx-imgBorder"]
        > ![eşleme yapılandırması-Exchange düzenleme](../media/setup-exchange-conf-tab-delete.png)

    * Bir bağlantı için tüm ayarları aşağıda gösterildiği gibi sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![eşleme yapılandırması-Exchange bağlantısı](../media/setup-exchange-conf-tab-connection.png)

        1. Bağlantının ayarlanması gereken **eşleme tesis** ' ı seçin.
        1. **IPv4 adresi** ve **IPv6 adresi**alanları ' nda, komşu komutu kullanılarak Microsoft yönlendiricilerde yapılandırılacak IPv4 ve IPv6 adresini girin.
        1. **En fazla tanıtılan IPv4 adresi** ve **en fazla tanıtılan IPv6 adreslerini** sırasıyla tanıtmak istediğiniz IPv4 ve IPv6 ön ekleri sayısını girin.
        1. Bağlantı ayarlarınızı kaydetmek için **Tamam** ' ı tıklatın.

1. Daha önce seçilen **Metro** dahilinde, Microsoft 'un ağınızla birlikte bulunduğu herhangi bir tesiste daha fazla bağlantı eklemek için yukarıdaki adımı tekrarlayın.

1. Gerekli tüm bağlantıları ekledikten sonra, **gözden geçir + oluştur**' a tıklayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme yapılandırması sekmesi son](../media/setup-exchange-conf-tab-final.png)

1. Portalın, girdiğiniz bilgilerin temel doğrulamasını çalıştırmasını gözlemleyin. Bu, *son doğrulama çalıştırılırken*en üstteki şeritte görüntülenir....

    > [!div class="mx-imgBorder"]
    > ![eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, **Oluştur**' a tıklayarak bilgilerinizi doğrulayın ve isteği iletin. İsteğinizi değiştirmeniz gerekiyorsa, **önceki** seçeneğine tıklayın ve yukarıdaki adımları tekrarlayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme gönderme](../media/setup-exchange-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım aşağıda gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![eşleme başarısı](../media/setup-direct-success.png)
