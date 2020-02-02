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
ms.openlocfilehash: 17ef8dd121c987c2771d274c8f0e6f9f199e386c
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76938239"
---
## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri Kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

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
   - **Kaynak grubu**: var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından açılır listeden bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../articles/azure-resource-manager/management/overview.md).
   - **Konum**: kasa için coğrafi bölgeyi seçin. Sanal makineleri korumaya yönelik bir kasa oluşturmak için kasanın sanal makinelerle aynı bölgede olması *gerekir* .

      > [!IMPORTANT]
      > SANAL makinenizin konumundan emin değilseniz iletişim kutusunu kapatın. Portaldaki sanal makineler listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir kurtarma hizmetleri Kasası oluşturun. Kasayı, başka bir konum için oluşturmadan önce ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesapları belirtmeniz gerekmez. Kurtarma Hizmetleri kasası ve Azure Backup tanıtıcısı otomatik olarak yapılır.
      >
      >

1. Kurtarma Hizmetleri kasasını oluşturmaya hazırsanız **Oluştur**' u seçin.

    ![Kurtarma Hizmetleri kasasını oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanızın oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

     ![Yedekleme kasaları listesini yenileme](./media/backup-create-rs-vault/refresh-button.png)
