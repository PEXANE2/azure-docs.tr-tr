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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775283"
---
1. **Eşleme Oluştur** sayfasında, **Yapılandırma** sekmesinin altında, aşağıda gösterildiği gibi alanları doldurun.

    * **Peering türü için** *Doğrudan'ı*seçin.
    * **Microsoft ağı için** *AS8075'i*seçin. ASN 8069 ile eşleme oluşturmayın. Özel uygulamalar için ayrılmıştır ve yalnızca [Microsoft peering](mailto:peering@microsoft.com)tarafından kullanılır.
    * *Temel Ücretsiz*olarak **SKU'yı** seçin. Özel uygulamalar için ayrılmış olduğundan *Premium Free'yi* seçmeyin.
    * Bakmayı ayarlamak istediğiniz yer için **Metro** konumunu seçin.

        > [!NOTE]
        > Seçili **Metro** konumunda Microsoft ile zaten karşılaş bağlantılarınız varsa ve bu konumda ki eşlemi ayarlamak için portalı ilk kez kullanıyorsanız, varolan görünen bağlantılarınız aşağıda gösterildiği gibi **Peering bağlantıları** bölümünde listelenir. Microsoft, yeni bağlantılarla birlikte tek bir yerde yönetebilmeniz için bu bakan bağlantıları otomatik olarak Azure kaynağına dönüştürür. Bkz. Daha fazla bilgi için portalı kullanarak azure kaynağına doğrudan [bakan bir eskiye dönüştürün.](../howto-legacy-direct-portal.md)
        >

1. **Peering bağlantıları**altında, kurmak istediğiniz her yeni bağlantı için bir satır eklemek için **yeni oluştur'u** tıklatın.

    * Bağlantı ayarlarını yapılandırmak/değiştirmek için bir satırın düzenle düğmesini tıklatın.

        > [!div class="mx-imgBorder"]
        > ![Peering Configuration - Doğrudan Düzenleme](../media/setup-direct-conf-tab-edit.png)
    
    * Bir satırı silmek için **Sil'>** **Delete**... düğmesine tıklayın.

        > [!div class="mx-imgBorder"]
        > ![Peering Configuration - Doğrudan Düzenleme](../media/setup-direct-conf-tab-delete.png)

    * Aşağıda gösterildiği gibi bir bağlantı için tüm ayarları sağlamanız gerekir.

         > [!div class="mx-imgBorder"]
         > ![Peering Configuration - Doğrudan Bağlantı](../media/setup-direct-conf-tab-connection.png)

        1. Bağlantının kurulması gereken **Eşleme tesisini** seçin.
        1. **Oturum Adresi Sağlayıcısı,** ağınızla Microsoft arasında BGP oturumu ayarlamak için gereken alt ağı kimin sağladığını belirlemek için kullanılır. Alt ağ sağlayabilirseniz, *Eş'i*seçin. Aksi takdirde Microsoft ve **Microsoft** bakan seçin sizinle irtibata [geçecektir.](mailto:peering@microsoft.com) Bunu seçmenin, Microsoft'un eşleme isteğini işlemesi daha uzun sürer. Bazı durumlarda Microsoft, istek reddi ile sonuçlanacak alt ağlar sağlayamayabilir.
        1. **Oturum Adres Sağlayıcısı'nı** *Eş*olarak seçtiyseniz, sırasıyla **Oturum IPv4 Öneki** ve **Oturum IPv6 Öneki** alanlarında önek maskesi ile birlikte IPv4 ve IPv6 adresini girin.
        1. Sırasıyla **Maksimum reklamı yapılan IPv4 adresleri** ve Maksimum reklamı yapılan IPv6 adresleri alanlarında reklamını yapacağınız **IPv4** ve IPv6 önekleri numarasını girin.
        1. Toplam **Bant Genişliği** kaydırıcısını bağlantının bant genişliğini yansıtacak şekilde ayarlayın.
        1. Bağlantı ayarlarınızı kaydetmek için **Tamam'ı** tıklatın.

1. Microsoft'un ağınızla birlikte bulunduğu herhangi bir tesise, daha önce seçilen **Metro** içinde daha fazla bağlantı eklemek için yukarıdaki adımı yineleyin.

1. Gerekli tüm bağlantıları ekledikten sonra **Gözden Geçir + oluştur'a**tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Peering Configuration Tab Final](../media/setup-direct-conf-tab-final.png)

1. Portalın girdiğiniz bilgilerin temel doğrulanmasını çalıştırdığını gözlemleyin. Bu, *son doğrulamayı çalıştırırken*üstteki şeritte görüntülenir... .

    > [!div class="mx-imgBorder"]
    > ![Eşleme Doğrulama Sekmesi](../media/setup-direct-review-tab-validation.png)

1. *Validation Geçti*döndükten sonra, bilgilerinizi doğrulayın ve **Oluştur'a**tıklayarak isteğinizi gönderin. İsteğinizi değiştirmeniz gerekiyorsa, **Önceki'yi** tıklatın ve yukarıdaki adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![Eşleme Gönder](../media/setup-direct-review-tab-submit.png)

1. İsteği gönderdikten sonra, dağıtımı tamamlamasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Başarılı bir dağıtım aşağıdaki gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![Akran Başarısı](../media/setup-direct-success.png)
