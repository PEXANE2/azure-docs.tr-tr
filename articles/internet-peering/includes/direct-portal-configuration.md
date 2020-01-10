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
ms.openlocfilehash: 2a42ba8809e6895c9eea9f8762513b7fcaa9eb3a
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775283"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    * **Eşleme türü**için *doğrudan*' yi seçin.
    * **Microsoft ağı**için *AS8075*öğesini seçin. ASN 8069 ile eşleme oluşturmayın. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft eşlemesi](mailto:peering@microsoft.com)tarafından kullanılır.
    * **SKU 'Yu** *temel ücretsiz*olarak seçin. Özel uygulamalar için ayrıldığından *Premium ücretsiz* 'yi seçmeyin.
    * Eşlemeyi ayarlamak istediğiniz **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten eşleme bağlantılarınız varsa ve bu konumda eşleme ayarlamak için Portal kullanıyorsanız, mevcut eşleme bağlantılarınız aşağıda gösterildiği gibi **eşleme bağlantıları** bölümünde listelenecektir. Microsoft bu eşleme bağlantılarını, yeni bağlantılarla birlikte tek bir yerde yönetebilmeniz için Azure kaynağına otomatik olarak dönüştürecek. Daha fazla bilgi için bkz. [portalı kullanarak eski doğrudan eşlemeyi Azure kaynağına dönüştürme](../howto-legacy-direct-portal.md) .
        >

1. **Eşleme bağlantıları**altında, kurmak istediğiniz her yeni bağlantı için bir satır eklemek üzere **Yeni oluştur** ' a tıklayın.

    * Bağlantı ayarlarını yapılandırmak/değiştirmek için bir satır için Düzenle düğmesine tıklayın.

        > [!div class="mx-imgBorder"]
        > ![eşleme yapılandırması-doğrudan düzenleme](../media/setup-direct-conf-tab-edit.png)
    
    * Bir satırı silmek için, **sil**>. **..** düğmesine tıklayın.

        > [!div class="mx-imgBorder"]
        > ![eşleme yapılandırması-doğrudan düzenleme](../media/setup-direct-conf-tab-delete.png)

    * Bir bağlantı için tüm ayarları aşağıda gösterildiği gibi sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![eşleme yapılandırması-doğrudan bağlantı](../media/setup-direct-conf-tab-connection.png)

        1. Bağlantının ayarlanması gereken **eşleme tesis** ' ı seçin.
        1. **Oturum adres sağlayıcısı** , ağınız ile MICROSOFT arasında BGP oturumu ayarlamak için gereken alt ağı kimlerin sağladığını belirlemede kullanılır. Alt ağ sağlayabiliyorsanız, *eş*' i seçin. Aksi takdirde **Microsoft** ve [Microsoft eşlemesi](mailto:peering@microsoft.com) sizinle iletişim kuracaktır. Bunu seçtiğinizde, Microsoft 'un eşleme isteğini işlemesi daha uzun sürer. Bazı durumlarda Microsoft, istek reddine neden olacak alt ağlar sunmayabilir.
        1. *Eş*olarak **oturum adres sağlayıcısını** seçtiyseniz, alan **oturumu IPv4 öneki** ve **oturum IPv6 ön eki** ' nde önek maskesiyle birlikte IPv4 ve IPv6 adresi girin.
        1. **En fazla tanıtılan IPv4 adresi** ve **en fazla tanıtılan IPv6 adreslerini** sırasıyla tanıtmak istediğiniz IPv4 ve IPv6 ön ekleri sayısını girin.
        1. **Toplam bant genişliği** kaydırıcısını bağlantının bant genişliğini yansıtacak şekilde ayarlayın.
        1. Bağlantı ayarlarınızı kaydetmek için **Tamam** ' ı tıklatın.

1. Daha önce seçilen **Metro** dahilinde, Microsoft 'un ağınızla birlikte bulunduğu herhangi bir tesiste daha fazla bağlantı eklemek için yukarıdaki adımı tekrarlayın.

1. Gerekli tüm bağlantıları ekledikten sonra, **gözden geçir + oluştur**' a tıklayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme yapılandırması sekmesi son](../media/setup-direct-conf-tab-final.png)

1. Portalın, girdiğiniz bilgilerin temel doğrulamasını çalıştırmasını gözlemleyin. Bu, *son doğrulama çalıştırılırken*en üstteki şeritte görüntülenir....

    > [!div class="mx-imgBorder"]
    > ![eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, **Oluştur**' a tıklayarak bilgilerinizi doğrulayın ve isteği iletin. İsteğinizi değiştirmeniz gerekiyorsa, **önceki** seçeneğine tıklayın ve yukarıdaki adımları tekrarlayın.

    > [!div class="mx-imgBorder"]
    > ![eşleme gönderme](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım aşağıda gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![eşleme başarısı](../media/setup-direct-success.png)
