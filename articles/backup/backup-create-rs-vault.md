---
title: Kurtarma Hizmetleri kasaları oluşturma
description: Bu makalede, yedeklemeleri ve kurtarma noktalarını depolayan kurtarma hizmetleri kasaları oluşturmayı öğrenin.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 6ac1c7e887f80767d6ff1819476e91cb4b06bf1b
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744937"
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

   - **Ad**: kasayı tanımlamak için bir kolay ad girin. Ad, Azure aboneliğine özgü olmalıdır. En az iki, 50 karakterden daha fazla olmayan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içermelidir.
   - **Abonelik**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliğin kullanılacağı konusunda emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınızın birden fazla Azure aboneliğiyle ilişkilendirilmesi durumunda birden çok seçenek vardır.
   - **Kaynak grubu**: var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından aşağı açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin. Kaynak grupları hakkında tüm bilgiler için bkz. [Azure Resource Manager genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   - **Konum**: kasa için coğrafi bölgeyi seçin. Sanal makineleri korumaya yönelik bir kasa oluşturmak için kasanın sanal makinelerle aynı bölgede olması **gerekir** .

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

## <a name="set-cross-region-restore"></a>Çapraz bölge geri yüklemeyi ayarla

Geri yükleme seçeneklerinden biri olan çapraz bölge geri yükleme (CRR), Azure sanal makinelerini bir [Azure eşlenmiş bölgesi](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)olan ikincil bir bölgeye geri yüklemenize olanak tanır. Bu seçenek şunları yapmanıza olanak sağlar:

- Denetim veya uyumluluk gereksinimi olduğunda ayrıntıya ait ayrıntıları yürütün
- Birincil bölgede bir olağanüstü durum varsa VM 'yi veya diskini geri yükleyin.

Bu özelliği seçmek için **yedekleme yapılandırması** dikey penceresinden **çapraz bölge geri yüklemeyi etkinleştir** ' i seçin.

Bu işlem için, depolama düzeyinde olduğu kadar fiyatlandırma etkileri vardır.

>[!NOTE]
>Başlamadan önce:
>
>- Desteklenen yönetilen türlerin ve bölgelerin listesi için [destek matrisini](backup-support-matrix.md#cross-region-restore) gözden geçirin.
>- Çapraz bölge geri yükleme (CRR) özelliği artık tüm Azure genel bölgelerinde önizlenebilir.
>- CRR, herhangi bir GRS Kasası için kasa düzeyi katılım özelliğidir (varsayılan olarak kapalıdır).
>- Aboneliğinizi bu özellik için eklemek üzere lütfen aşağıdaki komutu kullanın:<br>
>  `Register-AzProviderFeature -FeatureName CrossRegionRestore -ProviderNamespace Microsoft.RecoveryServices`
>- Genel sınırlı önizleme sırasında bu özelliğe eklendi yapıyorsanız, onay e-postasını gözden geçir fiyatlandırma ilkesi ayrıntılarını içerir.
>- Bu işlem yapıldıktan sonra, yedekleme öğelerinin ikincil bölgelerde kullanılabilmesi 48 saat kadar sürebilir.
>- Şu anda CRR yalnızca yedekleme yönetimi türü ARM Azure VM için desteklenir (klasik Azure VM desteklenmez).  Ek yönetim türleri CRR 'yi desteklediklerinde, bunlar **otomatik olarak** kaydedilir.

### <a name="configure-cross-region-restore"></a>Çapraz bölge geri yüklemeyi yapılandırma

GRS yedekliliği ile oluşturulan bir kasa, çapraz bölge geri yükleme özelliğini yapılandırma seçeneğini içerir. Her GRS kasasının bir başlığı olur ve bu, belgelere bağlanır. Kasa için CRR 'yi yapılandırmak için, bu özelliği etkinleştirme seçeneğini içeren yedekleme yapılandırması dikey penceresine gidin.

 ![Yedekleme yapılandırması başlığı](./media/backup-azure-arm-restore-vms/banner.png)

1. Portaldan kurtarma hizmetleri Kasası > ayarlar > Özellikler ' e gidin.
2. İşlevselliği etkinleştirmek için **Bu kasada çapraz bölge geri yüklemeyi etkinleştir** ' e tıklayın.

   ![Bu kasada çapraz bölge geri yüklemeyi etkinleştir ' e tıklamadan önce](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![Bu kasada çapraz bölge geri yüklemeyi etkinleştir ' e tıkladıktan sonra](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

[İkincil bölgedeki yedekleme öğelerini görüntülemeyi](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region)öğrenin.

[İkincil bölgede geri yüklemeyi](backup-azure-arm-restore-vms.md#restore-in-secondary-region)öğrenin.

[İkincil bölge geri yükleme işlerini izlemeyi](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs)öğrenin.

## <a name="modifying-default-settings"></a>Varsayılan ayarları değiştirme

Kasadaki yedeklemeleri yapılandırmadan önce **depolama çoğaltma türü** ve **güvenlik ayarları** için varsayılan ayarları incelemenizi kesinlikle öneririz.

- **Depolama çoğaltma türü** varsayılan olarak **coğrafi olarak yedekli**olarak ayarlanır. Yedeklemeyi yapılandırdıktan sonra, değiştirme seçeneği devre dışı bırakılır. Ayarları gözden geçirmek ve değiştirmek için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy) izleyin.

- Varsayılan olarak **geçici silme** , yeni oluşturulan kasaların yanlışlıkla veya kötü amaçlı silmelerden yedekleme verilerini korumak için **etkinleştirilir** . Ayarları gözden geçirmek ve değiştirmek için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#enabling-and-disabling-soft-delete) izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) Kurtarma Hizmetleri kasaları.
[Hakkında bilgi edinin](backup-azure-delete-vault.md) Kurtarma Hizmetleri kasalarını silin.
