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
ms.openlocfilehash: fda22346a44388248e37473bc7891b8a130569c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681047"
---
1. **Eşleme oluştur** sayfasında, **yapılandırma** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin.

    * **Eşleme türü**için **doğrudan**' yi seçin.
    * **Microsoft ağı**için **AS8075**öğesini seçin. ASN 8069 ile eşleme oluşturmayın. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft eşlemesi](mailto:peering@microsoft.com)tarafından kullanılır.
    * **SKU 'Yu** **temel ücretsiz**olarak seçin. Premium ücretsiz seçmeyin çünkü özel uygulamalar için ayrılmıştır.
    * Eşlemeyi ayarlamak istediğiniz **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten eşleme bağlantılarınız varsa ve bu konumda eşleme ayarlamak için Azure Portal kullanıyorsanız, mevcut eşleme bağlantılarınız, gösterilen şekilde **eşleme bağlantıları** bölümünde listelenecektir. Microsoft bu eşleme bağlantılarını, yeni bağlantılarla birlikte tek bir yerde yönetebilmeniz için bir Azure kaynağına otomatik olarak dönüştürür. Daha fazla bilgi için bkz. [portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](../howto-legacy-direct-portal.md).
        >

1. **Eşleme bağlantıları**altında, **Yeni oluştur** ' u seçerek ayarlamak istediğiniz her yeni bağlantı için bir satır ekleyin.

    * Bağlantı ayarlarını yapılandırmak veya değiştirmek için, bir satır için Düzenle düğmesini seçin.

        > [!div class="mx-imgBorder"]
        > ![Düzenle düğmesi](../media/setup-direct-conf-tab-edit.png)
    
    * Bir satırı silmek için... seçeneğini belirleyin **.**  >  **Silin**.

        > [!div class="mx-imgBorder"]
        > ![Sil düğmesi](../media/setup-direct-conf-tab-delete.png)

    * Burada gösterildiği gibi, bir bağlantı için tüm ayarları sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![Doğrudan eşleme bağlantı sayfası](../media/setup-direct-conf-tab-connection.png)

        1. Bağlantının ayarlanması gereken **eşleme tesis** ' ı seçin.
        1. **Oturum adres sağlayıcısı** , ağınız ile MICROSOFT arasında BGP oturumu ayarlamak için gereken alt ağı kimlerin sağladığını belirlemede kullanılır. Alt ağı sağlayabiliyorsanız, **eş**' i seçin. Aksi takdirde, **Microsoft** ve [Microsoft eşlemesi](mailto:peering@microsoft.com) sizinle iletişim kuracaktır. Bu seçeneğin belirlenmesi Microsoft 'un eşleme isteğini işlemesi için daha uzun sürer. Bazı durumlarda Microsoft, istek reddetme isteğine neden olacak alt ağlar sağlayamayabilir.
        1. **Oturum adresi sağlayıcısı** seçeneğini **eş**olarak seçtiyseniz, **oturum IPv4 ön eki** ve **oturum IPv6 ön eki** kutularındaki önek maskeleri birlikte IPv4 ve IPv6 adreslerini girin.
        1. **En fazla tanıtılan IPv4 adreslerinde** ve **en yüksek tanıtılan IPv6 adresi** kutularında duyurmanız gereken IPv4 ve IPv6 ön ekleri sayısını girin.
        1. **Toplam bant genişliği** kaydırıcısını bağlantının bant genişliğini yansıtacak şekilde ayarlayın.
        1. Bağlantı ayarlarınızı kaydetmek için **Kaydet** ' i seçin.

1. Daha önce seçtiğiniz **Metro** kapsamında Microsoft 'un ağınızla birlikte bulunduğu herhangi bir tesiste daha fazla bağlantı eklemek için önceki adımı tekrarlayın.

1. Gerekli tüm bağlantıları ekledikten sonra, **gözden geçir + oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme yapılandırma sekmesi son](../media/setup-direct-conf-tab-final.png)

1. Portalın girdiğiniz bilgilerin temel doğrulamasını çalıştırdığına dikkat edin. Üstteki bir şerit *son doğrulama çalıştıran iletiyi görüntüler...*.

    > [!div class="mx-imgBorder"]
    > ![Eşleme doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Doğrulama başarılı*olduktan sonra, bilgilerinizi doğrulayın. **Oluştur**seçeneğini belirleyerek isteği gönderebilirsiniz. İsteğinizi değiştirmek için, **önceki** seçeneğini belirleyip adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme gönderimi](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra dağıtımın bitmesini bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım burada gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Eşleme başarısı](../media/setup-direct-success.png)
