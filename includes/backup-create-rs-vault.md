---
title: dosya dahil etme
description: dosya dahil etme
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: include
ms.date: 10/18/2018
ms.author: dacurwin
ms.custom: include file
ms.openlocfilehash: 2509bdae01fc9de5511dcd67eb95bf0d13d5cf39
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391170"
---
## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan kurtarma noktalarını depolayan ve yedeklemeyle ilgili işlemleri gerçekleştirmek için bir arabirim sağlayan bir yönetim varlıktır. Bunlar isteğe bağlı yedeklemeler almayı, geri yükleme gerçekleştirmeyi ve yedekleme ilkeleri oluşturmayı içerir.

Bir kurtarma hizmetleri Kasası oluşturmak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/)aboneliğinizde oturum açın.

1. Sol taraftaki menüden **tüm hizmetler**' i seçin.

    ![Tüm hizmetleri seçin](./media/backup-create-rs-vault/click-all-services.png)

1. **Tüm hizmetler** Iletişim kutusunda *Kurtarma Hizmetleri*' ni girin. Giriş listenize göre filtrelerin kaynak listesi. Kaynak listesinde, **Kurtarma Hizmetleri kasaları**' nı seçin.

    ![Kurtarma Hizmetleri kasalarını girin ve seçin](./media/backup-create-rs-vault/all-services.png)

    Abonelikteki kurtarma hizmetleri kasalarının listesi görüntülenir.

1. **Kurtarma Hizmetleri kasaları** panosunda **Ekle**' yi seçin.

    ![Kurtarma Hizmetleri Kasası ekleme](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri Kasası** iletişim kutusu açılır. **Ad**, **abonelik**, **kaynak grubu**ve **konum**için değerler sağlayın.

    ![Kurtarma Hizmetleri kasasını yapılandırma](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Ad**: kasayı tanımlamak için bir kolay ad girin. Ad, Azure aboneliğine özgü olmalıdır. En az 2 ve 50 karakterden uzun olmayan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içermelidir.
   - **Abonelik**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliğin kullanılacağı konusunda emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınızın birden fazla Azure aboneliğiyle ilişkilendirilmesi durumunda birden çok seçenek vardır.
   - **Kaynak grubu**: var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından açılır listeden bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../articles/azure-resource-manager/management/overview.md).
   - **Konum**: kasa için coğrafi bölgeyi seçin. Herhangi bir veri kaynağını korumaya yönelik bir kasa oluşturmak için kasanın veri kaynağıyla aynı bölgede olması *gerekir* .

      > [!IMPORTANT]
      > Veri kaynağınızın konumundan emin değilseniz iletişim kutusunu kapatın. Portaldaki kaynaklarınızın listesine gidin. Birden çok bölgede veri kaynaklarınız varsa, her bölge için bir kurtarma hizmetleri Kasası oluşturun. Kasayı, başka bir konum için oluşturmadan önce ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesapları belirtmeniz gerekmez. Kurtarma Hizmetleri kasası ve Azure Backup tanıtıcısı otomatik olarak yapılır.
      >
      >

1. Kurtarma Hizmetleri kasasını oluşturmaya hazırsanız **Oluştur**' u seçin.

    ![Kurtarma Hizmetleri kasasını oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanızın oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

     ![Yedekleme kasaları listesini yenileme](./media/backup-create-rs-vault/refresh-button.png)
