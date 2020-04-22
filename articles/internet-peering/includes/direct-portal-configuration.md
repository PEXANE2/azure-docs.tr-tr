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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681047"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinde, burada gösterildiği gibi kutuları doldurun.

    * **Peering türü için** **Doğrudan'ı**seçin.
    * **Microsoft ağı için** **AS8075'i**seçin. ASN 8069 ile bakanlık oluşturmayın. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft peering](mailto:peering@microsoft.com)tarafından kullanılır.
    * **Temel Ücretsiz**olarak **SKU'yı** seçin. Özel uygulamalar için ayrılmış olduğundan Premium Free'i seçmeyin.
    * Bakmayı ayarlamak istediğiniz **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten karşılaş bağlantılarınız varsa ve bu konumda eşleme ayarlamak için ilk kez Azure portalını kullanıyorsanız, varolan akran bağlantılarınız gösterildiği gibi **Eşleme bağlantıları** bölümünde listelenir. Microsoft, yeni bağlantılarla birlikte tüm bağlantıları tek bir yerde yönetebilmeniz için bu bakan bağlantıları otomatik olarak bir Azure kaynağına dönüştürür. Daha fazla bilgi için bkz. Portalı [kullanarak bir Azure kaynağına doğrudan bakan bir eskiye dönüştürün.](../howto-legacy-direct-portal.md)
        >

1. **Peering bağlantıları**altında, kurmak istediğiniz her yeni bağlantı için satır eklemek için **yeni oluştur'u** seçin.

    * Bağlantı ayarlarını yapılandırmak veya değiştirmek için bir satır Için Düzenle düğmesini seçin.

        > [!div class="mx-imgBorder"]
        > ![Düzenle düğmesi](../media/setup-direct-conf-tab-edit.png)
    
    * Bir satırı silmek için **, ...**  >  **Sil.**

        > [!div class="mx-imgBorder"]
        > ![Sil düğmesi](../media/setup-direct-conf-tab-delete.png)

    * Burada gösterildiği gibi, bir bağlantı için tüm ayarları sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![Doğrudan Eşleme Bağlantısı sayfası](../media/setup-direct-conf-tab-connection.png)

        1. Bağlantının kurulması gereken **Eşleme tesisini** seçin.
        1. **Oturum Adresi Sağlayıcısı,** ağınızla Microsoft arasında BGP oturumunu ayarlamak için gereken alt ağı kimin sağladığını belirlemek için kullanılır. Alt ağı sağlayabilirseniz, **Eş'i**seçin. Aksi takdirde, **Microsoft** ve [Microsoft'un bakışlarını](mailto:peering@microsoft.com) seçin, sizinle iletişim eve gelecektir. Bu seçeneğin seçilmesi, Microsoft'un eşleme isteğini işlemesi daha uzun sürer. Bazı durumlarda, Microsoft alt ağlar sağlayamadığı için istek reddi ile sonuçlanır.
        1. Oturum Adresi **Sağlayıcı** seçeneğini **Eş**olarak seçtiyseniz, sırasıyla **Oturum IPv4 öneki** ve **Oturum IPv6 öneki** kutularındaki önek maskeleriyle birlikte IPv4 ve IPv6 adreslerini girin.
        1. Sırasıyla **Maksimum reklamı yapılan Maksimum IPv4 adreslerine** ve Maksimum reklamı yapılan **IPv6 adres kutularına** reklam verecekolan IPv4 ve IPv6 önekleri numarasını girin.
        1. Toplam **bant genişliği** kaydırıcısını bağlantının bant genişliğini yansıtacak şekilde ayarlayın.
        1. Bağlantı ayarlarınızı kaydetmek için **Kaydet'i** seçin.

1. Microsoft'un ağınızla birlikte bulunduğu herhangi bir tesise, daha önce seçtiğiniz **Metro** içinde daha fazla bağlantı eklemek için önceki adımı yineleyin.

1. Gerekli tüm bağlantıları ekledikten sonra **Gözden Geçir + oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Yapılandırması sekmesi son](../media/setup-direct-conf-tab-final.png)

1. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığına dikkat edin. Üstteki *şerit, son doğrulamayı çalıştıran*iletiyi görüntüler... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama sekmesi](../media/setup-direct-review-tab-validation.png)

1. İleti nin *Doğrulama'ya geçirilmesinden*sonra bilgilerinizi doğrulayın. **Oluştur'u**seçerek isteği gönderin. İsteğinizi değiştirmek için **Önceki'yi** seçin ve adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Akran gönderimi](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra dağıtımın bitmesini bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Burada gösterildiği gibi başarılı bir dağıtım görüntülenir.

    > [!div class="mx-imgBorder"]
    > ![Akran başarısı](../media/setup-direct-success.png)
