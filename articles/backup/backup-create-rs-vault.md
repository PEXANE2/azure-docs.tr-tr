---
title: 'Azure Backup: Kurtarma Hizmetleri kasaları oluşturma'
description: yedeklemeleri ve kurtarma noktalarını depolayan kurtarma hizmetleri kasaları oluşturma
ms.reviewer: sogup
author: dcurwin
manager: carmonm
keywords: Kurtarma Hizmetleri Kasası; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: dacurwin
ms.openlocfilehash: d13aff40d735e98e7745358f8caddcd8cdc20727
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688555"
---
# <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri Kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. [Azure Portal](https://portal.azure.com/)aboneliğinizde oturum açın.

2. Sol taraftaki menüden **tüm hizmetler**' i seçin.

    ![Tüm hizmetleri seçin](./media/backup-create-rs-vault/click-all-services.png)

3. **Tüm hizmetler** Iletişim kutusunda **Kurtarma Hizmetleri**' ni girin. Giriş listenize göre filtrelerin kaynak listesi. Kaynak listesinde, **Kurtarma Hizmetleri kasaları**' nı seçin.

    ![Kurtarma Hizmetleri kasalarını girin ve seçin](./media/backup-create-rs-vault/all-services.png)

    Abonelikteki kurtarma hizmetleri kasalarının listesi görüntülenir.

4. **Kurtarma Hizmetleri kasaları** panosunda **Ekle**' yi seçin.

    ![Kurtarma Hizmetleri Kasası ekleme](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri Kasası** iletişim kutusu açılır. **Ad**, **abonelik**, **kaynak grubu**ve **konum**için değerler sağlayın.

    ![Kurtarma Hizmetleri kasasını yapılandırma](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Ad**: Kasayı tanımlamak için bir kolay ad girin. Ad, Azure aboneliğine özgü olmalıdır. En az iki, 50 karakterden daha fazla olmayan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içermelidir.
   - **Abonelik**: Kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliğin kullanılacağı konusunda emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınızın birden fazla Azure aboneliğiyle ilişkilendirilmesi durumunda birden çok seçenek vardır.
   - **Kaynak grubu**: Var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından aşağı açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin. Kaynak grupları hakkında tüm bilgiler için bkz. [Azure Resource Manager genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   - **Konum**: Kasa için coğrafi bölgeyi seçin. Sanal makineleri korumaya yönelik bir kasa oluşturmak için kasanın sanal makinelerle aynı bölgede olması **gerekir** .

      > [!IMPORTANT]
      > SANAL makinenizin konumundan emin değilseniz iletişim kutusunu kapatın. Portaldaki sanal makineler listesine gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir kurtarma hizmetleri Kasası oluşturun. Kasayı, başka bir konum için oluşturmadan önce ilk konumda oluşturun. Yedekleme verilerini depolamak için depolama hesapları belirtmeniz gerekmez. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti otomatik olarak yapılır.
      >
      >

5. Kurtarma Hizmetleri kasasını oluşturmaya hazırsanız **Oluştur**' u seçin.

    ![Kurtarma Hizmetleri kasasını oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanızın oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

     ![Yedekleme kasaları listesini yenileme](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>Depolama artıklığı ayarlama

Azure Backup kasa için depolamayı otomatik olarak işler. Bu depolamanın nasıl çoğaltılacağı belirtmeniz gerekir.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. **Ayarlar** bölümünde, **Özellikler**' e tıklayın.
2. **Özellikler**' de, **yedekleme yapılandırması**altında **Güncelleştir**' e tıklayın.

3. Depolama çoğaltma türünü seçin ve **Kaydet**' e tıklayın.

     ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Azure 'u birincil yedek depolama uç noktası olarak kullanıyorsanız, varsayılan **coğrafi olarak yedekli** ayarını kullanmaya devam edebilirsiniz.
   - Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin.
   - [Coğrafi](../storage/common/storage-redundancy-grs.md) ve [Yerel](../storage/common/storage-redundancy-lrs.md) artıklık hakkında daha fazla bilgi edinin.

> [!NOTE]
> Bir kurtarma hizmetleri Kasası için **depolama çoğaltma türünü** (yerel olarak yedekli/coğrafi olarak yedekli) değiştirmek, kasadaki yedeklemeleri yapılandırmadan önce yapılmalıdır. Yedeklemeyi yapılandırdıktan sonra, değiştirme seçeneği devre dışı bırakılır ve **depolama çoğaltma türünü**değiştiremezsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) Kurtarma Hizmetleri kasaları.
[Hakkında bilgi edinin](backup-azure-delete-vault.md) Kurtarma Hizmetleri kasalarını silin.
