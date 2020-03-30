---
title: Kurtarma Hizmetleri kasaları oluşturun
description: Bu makalede, yedeklemeleri ve kurtarma noktalarını depolayan Kurtarma Hizmetleri kasalarını nasıl oluşturabileceğinizi öğrenin.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: e722996f516d21445d8e0028df925ca44eb02bfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295010"
---
# <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası, korumalı sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. [Azure portalında](https://portal.azure.com/)aboneliğinizde oturum açın.

2. Sol menüde **Tüm hizmetler'i**seçin.

    ![Tüm hizmetleri seçin](./media/backup-create-rs-vault/click-all-services.png)

3. Tüm **hizmetler** iletişim kutusunda, **Kurtarma Hizmetleri'ni**girin. Kaynak listesi, girişinize göre filtreler. Kaynaklar listesinde Kurtarma **Hizmetleri kasalarını**seçin.

    ![Kurtarma Hizmetleri kasalarını girin ve seçin](./media/backup-create-rs-vault/all-services.png)

    Abonelikteki Kurtarma Hizmetleri kasalarının listesi görüntülenir.

4. Kurtarma **Hizmetleri kasaları** panosunda **Ekle'yi**seçin.

    ![Kurtarma Hizmetleri kasası ekleme](./media/backup-create-rs-vault/add-button-create-vault.png)

    **Kurtarma Hizmetleri kasa** iletişim kutusu açılır. **Ad,** **Abonelik,** Kaynak **grubu**ve **Konum**değerleri sağlayın.

    ![Kurtarma Hizmetleri kasasını yapılandırma](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Adı**: Tonoz tanımlamak için dostça bir ad girin. Ad Azure aboneliğine özgü olmalıdır. En az iki, ancak en fazla 50 karakter olan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harfler, sayılar ve tirelerden oluşmalıdır.
   - **Abonelik**: Kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliği kullanacağından emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınız birden fazla Azure aboneliğiyle ilişkiliyse birden çok seçenek vardır.
   - **Kaynak grubu**: Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **varolan kaynağı kullan'ı**seçin ve ardından açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin ve adı girin. Kaynak grupları hakkında tam bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)bilgisine bakın.
   - **Konum**: Kasa için coğrafi bölgeyi seçin. Sanal makineleri korumak için bir kasa oluşturmak için, tonoz sanal makinelerle aynı bölgede **olmalıdır.**

      > [!IMPORTANT]
      > VM'nizin konumundan emin değilseniz, iletişim kutusunu kapatın. Portaldaki sanal makineler listesine gidin. Çeşitli bölgelerde sanal makineleriniz varsa, her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Başka bir konum için tonoz oluşturmadan önce, ilk konumda tonoz oluşturun. Yedekleme verilerini depolamak için depolama hesapları belirtmenize gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Yedekleme hizmeti otomatik olarak işleyebilir.
      >
      >

5. Kurtarma Hizmetleri kasasını oluşturmaya hazır olduğunuzda **Oluştur'u**seçin.

    ![Kurtarma Hizmetleri kasasını oluşturma](./media/backup-create-rs-vault/click-create-button.png)

    Kurtarma Hizmetleri kasasını oluşturmak biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra, Kurtarma Hizmetleri kasaları listesinde görünür. Kasanızı **görmüyorsanız, Yenile'yi**seçin.

     ![Yedek kasalistesini yenileyin](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>Depolama artıklığını ayarlama

Azure Yedekleme kasaiçin depolamayı otomatik olarak işler. Depolama alanının nasıl çoğaltılacağını belirtmeniz gerekir.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. **Ayarlar** bölümünün altında **Özellikler'i**tıklatın.
2. **Özellikler'de**, **Yedekleme Yapılandırması**altında, **Güncelleştir'i**tıklatın.

3. Depolama çoğaltma türünü seçin ve **Kaydet'i**tıklatın.

     ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Azure'u birincil yedekleme depolama bitiş noktası olarak kullanıyorsanız, varsayılan **Coğrafi yedeka ayarını** kullanmaya devam etmenizi öneririz.
   - Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin.
   - [Coğrafi](../storage/common/storage-redundancy-grs.md) ve [yerel](../storage/common/storage-redundancy-lrs.md) artıklık hakkında daha fazla bilgi edinin.

> [!NOTE]
> Kurtarma hizmetleri kasası için **Depolama Çoğaltma türünü** (Yerel olarak yedekli/ Coğrafi yedekli) değiştirme, yedeklemeleri tonozda yapılandırmadan önce yapılmalıdır. Yedeklemeyi yapılandırdıktan sonra, değiştirme seçeneği devre dışı bırakılır ve **Depolama Çoğaltma türünü**değiştiremezsiniz.

## <a name="set-cross-region-restore"></a>Çapraz Bölge Geri Yükleme'yi Ayarla

Geri yükleme seçeneklerinden biri olarak, Çapraz Bölge Geri Yükleme (CRR), [Azure eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bir bölge olan ikinci bir bölgede Azure VM'lerini geri yüklemenize olanak tanır. Bu seçenek şunları yapmanızı sağlar:

- bir denetim veya uyumluluk gereksinimi olduğunda matkaplar yürütmek
- Birincil bölgede bir felaket varsa VM'yi veya diskini geri yükleyin.

Bu özelliği seçmek **için, Yedekleme Yapılandırması** bıçağından **Bölgeler Arası Geri Yüklemeyi Etkinleştir'i** seçin.

Bu işlem için, depolama düzeyinde olduğu gibi fiyatlandırma etkileri vardır.

>[!NOTE]
>Başlamadan önce:
>
>- Desteklenen yönetilen türlerin ve bölgelerin listesi için [destek matrisini](backup-support-matrix.md#cross-region-restore) gözden geçirin.
>- Çapraz Bölge Geri Yükleme (CRR) özelliği şu anda yalnızca aşağıdaki bölgelerde kullanılabilir: 
>    - Orta Batı ABD
>    - Batı ABD 2
>    - Orta Güney ABD
>    - Doğu ABD
>    - Doğu ABD 2
>    - Orta Kuzey ABD
>    - Orta Kanada
>    - Doğu Kanada
>    - Doğu Avustralya
>    - Güneydoğu Avustralya
>    - Orta Hindistan
>    - Güney Hindistan
>    - Doğu Japonya
>    - Batı Japonya
>    - Güneydoğu Asya
>    - Güney Birleşik Krallık
>    - Batı Birleşik Krallık
>    - Orta Fransa
>    - Güney Kore - Orta
>    - Güney Kore - Güney
>- CRR, herhangi bir GRS kasası için kasa düzeyinde bir tercih özelliğidir (varsayılan olarak kapatılır).
>- Bu özellik için aboneliğinizde bulunan lara aşağıdaki komutu kullanın:<br>
>  `Register-AzProviderFeature -FeatureName CrossRegionRestore -ProviderNamespace Microsoft.RecoveryServices`
>- Genel sınırlı önizleme sırasında bu özelliğe binmişseniz, inceleme onay e-postası fiyatlandırma politikası ayrıntılarını içerir.
>- Kabul ettikten sonra, yedekleme öğelerinin ikincil bölgelerde kullanılabilir olması 48 saat kadar sürebilir.
>- Şu anda CRR yalnızca Yedekleme Yönetim Türü - ARM Azure VM için desteklenir (klasik Azure VM desteklenmez).  Ek yönetim türleri CRR'yi desteklediğinde, **otomatik olarak** kaydedilirler.

### <a name="configure-cross-region-restore"></a>Çapraz Bölge Geri Yükleme'yi Yapılandır

GRS artıklığı ile oluşturulan bir kasa, Çapraz Bölge Geri Yükleme özelliğini yapılandırma seçeneğini içerir. Her GRS kasasının belgelere bağlanacak bir afişi olacak. Kasa için CRR'yi yapılandırmak için, bu özelliği etkinleştirme seçeneğini içeren Yedekleme Yapılandırması bıçağına gidin.

 ![Yedekleme Yapılandırma banner](./media/backup-azure-arm-restore-vms/banner.png)

1. Portaldan, Ayarlar > Özellikleri > Kurtarma Hizmetleri kasasına gidin.
2. İşlevselliği etkinleştirmek için **bu kasada Bölgeler Arası Geri Yükleme'yi Etkinleştir'i** tıklatın.

   ![Bu kasada Çapraz Bölge geri yüklemesini etkinleştir'i tıklatmadan önce](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![Bu kasada Çapraz Bölge geri yüklemesini etkinleştir'i tıklattıktan sonra](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

Yedekleme öğelerini ikincil bölgede nasıl [görüntüleyizleyin.](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region)

İkincil bölgede nasıl [geri yükleyin](backup-azure-arm-restore-vms.md#restore-in-secondary-region)için.

[İkincil bölge geri yükleme işlerini](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs)nasıl izleyeceğizi öğrenin.

## <a name="modifying-default-settings"></a>Varsayılan ayarları değiştirme

Yedeklemeleri kasada yapılandırmadan önce **Depolama Çoğaltma türü** ve Güvenlik **ayarları** için varsayılan ayarları incelemenizi öneririz.

- **Depolama Çoğaltma türü** varsayılan olarak **Geo-yedekli**olarak ayarlanır. Yedeklemeyi yapılandırdıktan sonra değiştirme seçeneği devre dışı bırakılır. Ayarları gözden geçirmek ve değiştirmek için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy) izleyin.

- Varsayılan olarak **yumuşak silme,** yedek verileri yanlışlıkla veya kötü amaçlı silmelere karşı korumak için yeni oluşturulan kasalarda **etkinleştirilir.** Ayarları gözden geçirmek ve değiştirmek için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#disabling-soft-delete) izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) Kurtarma Hizmetleri kasaları.
[Hakkında bilgi edinin](backup-azure-delete-vault.md) Kurtarma Hizmetleri kasalarını silin.
