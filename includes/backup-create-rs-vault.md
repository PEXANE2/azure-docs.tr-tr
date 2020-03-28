---
title: include dosyası
description: include dosyası
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: include
ms.date: 10/18/2018
ms.author: dacurwin
ms.custom: include file
ms.openlocfilehash: 6167774171affda7e5469d5852a79657a6da700d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78262477"
---
## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, zaman içinde oluşturulan kurtarma noktalarını depolayan bir depolama varlığıdır. Ayrıca, korumalı öğelerle ilişkili yedekleme ilkeleri de içerir.

Kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com/)aboneliğinizde oturum açın.

1. Sol menüde **Tüm hizmetler'i**seçin.

    ![Tüm hizmetleri seçin](./media/backup-create-rs-vault/click-all-services.png)

1. Tüm **hizmetler** iletişim kutusunda, *Kurtarma Hizmetleri'ni*girin. Kaynak listesi, girişinize göre filtreler. Kaynaklar listesinde Kurtarma **Hizmetleri kasalarını**seçin.

    ![Kurtarma Hizmetleri kasalarını girin ve seçin](./media/backup-create-rs-vault/all-services.png)

    Abonelikteki Kurtarma Hizmetleri kasalarının listesi görüntülenir.

1. Kurtarma **Hizmetleri kasaları** panosunda **Ekle'yi**seçin.

    ![Kurtarma Hizmetleri kasası ekleme](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri kasa** iletişim kutusu açılır. **Ad,** **Abonelik,** Kaynak **grubu**ve **Konum**değerleri sağlayın.

    ![Kurtarma Hizmetleri kasasını yapılandırma](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Adı**: Tonoz tanımlamak için dostça bir ad girin. Ad Azure aboneliğine özgü olmalıdır. En az 2 ancak en fazla 50 karakter içeren bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harfler, sayılar ve tirelerden oluşmalıdır.
   - **Abonelik**: Kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliği kullanacağından emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınız birden fazla Azure aboneliğiyle ilişkiliyse birden çok seçenek vardır.
   - **Kaynak grubu**: Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **varolan kaynağı kullan'ı**seçin ve ardından açılan listeden bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin ve adı girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/management/overview.md).
   - **Konum**: Kasa için coğrafi bölgeyi seçin. Sanal makineleri korumak için bir kasa oluşturmak için, tonoz sanal makinelerle aynı bölgede *olmalıdır.*

      > [!IMPORTANT]
      > VM'nizin konumundan emin değilseniz, iletişim kutusunu kapatın. Portaldaki sanal makineler listesine gidin. Çeşitli bölgelerde sanal makineleriniz varsa, her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Başka bir konum için tonoz oluşturmadan önce, ilk konumda tonoz oluşturun. Yedekleme verilerini depolamak için depolama hesapları belirtmenize gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Yedekleme otomatik olarak işleyebilir.
      >
      >

1. Kurtarma Hizmetleri kasasını oluşturmaya hazır olduğunuzda **Oluştur'u**seçin.

    ![Kurtarma Hizmetleri kasasını oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasını oluşturmak biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra, Kurtarma Hizmetleri kasaları listesinde görünür. Kasanızı **görmüyorsanız, Yenile'yi**seçin.

     ![Yedek kasalistesini yenileyin](./media/backup-create-rs-vault/refresh-button.png)
