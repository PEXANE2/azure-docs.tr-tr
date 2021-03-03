---
title: Azure Blob 'Ları için işletimsel yedeklemeyi yapılandırma
description: Azure Blobları için işlemsel yedeklemeyi yapılandırma ve yönetme hakkında bilgi edinin (önizlemede)
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: 7fe302de0e93575e7bb62f7c5cdefd7acc0720c3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746784"
---
# <a name="configure-operational-backup-for-azure-blobs-in-preview"></a>Azure Blobları için işletimsel yedeklemeyi yapılandırma (önizlemede)

Azure Backup, depolama hesaplarınızdaki blok bloblarını korumak için işlemsel yedeklemeyi kolayca yapılandırmanıza olanak tanır. Bu makalede, Azure portal kullanarak bir veya daha fazla depolama hesabında işletimsel yedeklemenin nasıl yapılandırılacağı açıklanır. Makalesinde aşağıdakiler ele alınmaktadır:

- Başlamadan önce bilmeniz gerekenler
- Yedekleme Kasası oluşturma
- Korunacak depolama hesaplarında yedekleme kasasının izinlerini verme
- Yedekleme ilkesi oluşturma
- İşlemsel yedeklemeyi bir veya daha fazla depolama hesabında yapılandırma
- Yedekleme depolama hesaplarında etkileri

## <a name="before-you-start"></a>Başlamadan önce

- Blobların işletimsel yedeklemesi, kaynak depolama hesabının kendisinde belirli bir süre için verileri tutan bir yerel yedekleme çözümüdür. Bu çözüm, kasadaki verilerin ek bir kopyasını korumaz.
- Bu çözüm, verilerinizi en fazla 360 gün süreyle geri yükleme için korumanızı sağlar. Ancak uzun bekletme süreleri, geri yükleme işlemi sırasında daha uzun süre alınmasına neden olabilir.
- Çözüm yalnızca kaynak depolama hesabına geri yükleme gerçekleştirmek için kullanılabilir ve verilerin üzerine yazılmasına neden olabilir.
- Kapsayıcıyı silme işlemini çağırarak depolama hesabından bir kapsayıcıyı silerseniz, o kapsayıcı geri yükleme işlemiyle geri yüklenemez. Bir kapsayıcının tamamını silmek yerine, daha sonra geri yüklemek istiyorsanız ayrı Blobları silin. Ayrıca, Microsoft kapsayıcıları yanlışlıkla silinmeye karşı korumak için işlemsel yedeklemeye ek olarak kapsayıcılar için geçici silme olanağı da sağlar.
- Desteklenen senaryolar, sınırlamalar ve kullanılabilirlik hakkında daha fazla bilgi edinmek için [destek matrisine](blob-backup-support-matrix.md) bakın.

## <a name="create-a-backup-vault"></a>Yedekleme Kasası oluşturma

[Yedekleme Kasası](backup-vault-overview.md) , zaman içinde oluşturulan kurtarma noktalarını depolayan ve yedeklemeyle ilgili işlemleri gerçekleştirmek için bir arabirim sağlayan bir yönetim varlıktır. Bu işlemler arasında isteğe bağlı yedekleme, geri yükleme gerçekleştirme ve yedekleme ilkesi oluşturma sayılabilir. Blobların işletimsel yedeklemesi yerel bir yedekleme olmasına rağmen, kasada "depolama" verileri olmasa da, çeşitli yönetim işlemleri için kasa gereklidir.

>[!NOTE]
>Yedekleme Kasası, yeni desteklenen iş yüklerini yedeklemek için kullanılan ve zaten var olan kurtarma hizmetleri kasasından farklı olan yeni bir kaynaktır.

Yedekleme Kasası oluşturma yönergeleri için bkz. [Yedekleme Kasası belgeleri](backup-vault-overview.md#create-a-backup-vault).

## <a name="grant-permissions-to-the-backup-vault-on-storage-accounts"></a>Depolama hesaplarında yedekleme kasasıyla ilgili izinleri verme

İşlemsel yedekleme, yedeklemeye ait silme kilidi uygulayarak yanlışlıkla silinmelerden (korunacak Blobları içeren) depolama hesabını da korur. Bu, yedekleme kasasının korunması gereken depolama hesaplarında belirli izinlere sahip olmasını gerektirir. Kullanım kolaylığı için, bu izinler depolama hesabı yedek katılımcısı rolü kapsamında birleştirildi. Korunması gereken depolama hesapları için aşağıdaki yönergeleri izleyin:

1. Korunacak depolama hesabında, sol gezinti bölmesindeki **Access Control (IAM) sekmesine** gidin.
1. Gerekli rolü atamak için **rol atamaları Ekle** ' yi seçin.

    ![Rol atamaları ekleme](./media/blob-backup-configure-manage/add-role-assignments.png)

1. Rol ataması Ekle bölmesinde:

    1. **Rol** altında, **depolama hesabı yedeklemesi katılımcısı**' nı seçin.
    1. **Erişim ata**' nın altında **Kullanıcı, Grup veya hizmet sorumlusu**' nı seçin.
    1. Bu depolama hesabındaki Blobları korumak istediğiniz **yedekleme kasasının adını** yazın ve arama sonuçlarından aynısını seçin.
    1. İşiniz bittiğinde **Kaydet**' i seçin.

        ![Rol atama seçenekleri](./media/blob-backup-configure-manage/role-assignment-options.png)

        >[!NOTE]
        >Rol atamasının etkili olabilmesi için en fazla 10 dakika bekleyin.

## <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi genellikle yedeklemelerinizin bekletme ve zamanlamasını yönetir. Blobların işletimsel yedeklemesi doğası gereği sürekli olduğundan, yedeklemeleri gerçekleştirmek için bir zamanlamanın olması gerekmez. İlke, bekletme dönemini belirtmek için gereklidir. Birden çok depolama hesabının yedeklemesini bir kasaya yapılandırmak için yedekleme ilkesini kullanabilir ve yeniden kullanabilirsiniz.

Bloblarınızın işletimsel yedeklemesi için yedekleme ilkesi oluşturma adımları aşağıda verilmiştir:

1. Yedekleme kasanızda yedekleme **ilkeleri** ' ne gidin ve bir yedekleme ilkesi oluşturmaya başlamak Için **+ Ekle** ' yi seçin.

    ![Yedekleme ilkeleri](./media/blob-backup-configure-manage/backup-policies.png)

1. **Temel bilgiler** sekmesinde, yedekleme ilkeniz için bir ad girin ve veri kaynağı türü olarak **Azure Blob** ' u seçin. Seçtiğiniz kasanızın ayrıntılarını da görüntüleyebilirsiniz.

    ![Yedekleme İlkesi Oluştur](./media/blob-backup-configure-manage/create-backup-policy.png)

    >[!NOTE]
    >Kasanın **yedek depolama yedekliliği** ' nı görebileceksiniz, ancak yedekleme doğası gereği ve yedekleme kasasında hiçbir veri depolanmasından bu yana yedeklilik gerçekten Blobların işletimsel yedeklemesi için geçerlidir. Yedekleme Kasası, depolama hesaplarınızdaki blok bloblarının korunmasını yönetmenize yardımcı olan yönetim varlıktır.

1. **Yedekleme ilkesi** sekmesi, bekletme ayrıntılarını belirlediğiniz yerdir. **Varsayılan** olarak, bekleme süresi 30 gün olan bir bekletme kuralı olduğunu göreceksiniz. Saklama süresini düzenlemek isterseniz, düzenlemek için **bekletme kuralını Düzenle** simgesini kullanın ve verilerin saklanmasını istediğiniz süreyi belirtin. 360 güne kadar bekletme belirtebilirsiniz.

    ![Yedekleme İlkesi sekmesi](./media/blob-backup-configure-manage/backup-policy-tab.png)

    >[!NOTE]
    >Uzun sürelerin geri yüklenmesi, geri yükleme işlemlerinin daha uzun sürmesine neden olabilir. Ayrıca, bir veri kümesini geri yüklemek için geçen süre, geri yükleme dönemi sırasında yapılan yazma ve silme işlemlerinin sayısına bağlıdır. Örneğin, 1.000.000 nesne başına 3.000 nesne ve gün başına silinen 1.000 olan bir hesap, geçmişte 30 günlük bir noktaya geri yüklemek için yaklaşık iki saat gerektirir. Geçmişte 90 günden fazla bir bekletme dönemi ve geri yükleme bu değişiklik oranına sahip bir hesap için önerilmez.

1. **Gözden geçir + oluştur** bölmesinde ilkenin tüm ayrıntılarını doğrulayın ve ilke oluşturmayı tamamladıktan sonra **Oluştur** ' u seçin. Yedekleme ilkesi oluşturulduktan ve kullanılmak üzere hazırlandıktan sonra bir bildirim onaylanır.

    ![İlkeyi gözden geçirme ve oluşturma](./media/blob-backup-configure-manage/review-create.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

Blobların yedeklenmesi depolama hesabı düzeyinde yapılandırılır. Bu nedenle, depolama hesabındaki tüm Bloblar işlemsel yedekleme ile korunur.

Yedeklemeyi yapılandırmaya başlamak için:

1. Arama çubuğunda **yedekleme merkezi** araması yapın.
1. **Genel bakış**  ->  **+ yedekleme** sayfasına gidin.

    ![Yedekleme merkezi 'ne genel bakış](./media/blob-backup-configure-manage/backup-center-overview.png)

1. **Başlat: yedeklemeyi Yapılandır** sekmesinde, veri kaynağı türü olarak **Azure Blob 'Ları (Azure depolama)** seçin.

    ![Başlat: Yedekleme sekmesini Yapılandır](./media/blob-backup-configure-manage/initiate-configure-backup.png)

1. **Temel bilgiler** sekmesinde, **veri kaynağı** türü olarak **Azure bloblarını (Azure depolama)** belirtin ve depolama hesaplarınızı ilişkilendirmek istediğiniz yedekleme kasasını seçin. Seçili kasasının ayrıntılarını bölmesinde görüntüleyebilirsiniz.

    ![Temel bilgiler sekmesi](./media/blob-backup-configure-manage/basics-tab.png)

1. Ardından, saklama 'yı belirtmek için kullanmak istediğiniz yedekleme ilkesini seçin. Seçili ilkenin ayrıntılarını ekranın alt bölümünde görüntüleyebilirsiniz. İşletimsel veri deposu sütunu, ilkede tanımlanan saklama alanını gösterir. "İşletimsel", verilerin kaynak depolama hesabının kendisinde yerel olarak korunmasıdır.

    ![Yedekleme ilkesi seçin](./media/blob-backup-configure-manage/choose-backup-policy.png)

    Yeni bir yedekleme ilkesi de oluşturabilirsiniz. Bunu yapmak için **Yeni oluştur** ' u seçin ve aşağıdaki adımları izleyin:

    1. Oluşturmak istediğiniz ilke için bir ad girin. Diğer kutuların doğru veri kaynağı türünü ve kasa adını göstermesini sağlayın.
    1. **Yedekleme ilkesi** sekmesinde, düzenlemek için bekletme kuralını Düzenle simgesini seçin ve verilerin bekletilmesini istediğiniz süreyi belirtin. 360 güne kadar bekletme belirtebilirsiniz. Uzun sürelerin geri yüklenmesi, geri yükleme işlemlerinin daha uzun sürmesine neden olabilir.

        ![Yeni yedekleme İlkesi Oluştur](./media/blob-backup-configure-manage/new-backup-policy.png)

    1. İşiniz bittiğinde, yedekleme ilkesini oluşturmak için **gözden geçir + oluştur** ' u seçin.

1. Ardından, Blobların korumasını yapılandırmak istediğiniz depolama hesaplarını seçmeniz gerekir. Aynı anda birden çok depolama hesabı seçebilir ve Seç ' **i** seçebilirsiniz.

    Ancak, seçtiğiniz kasanın depolama hesaplarında yedekleme [kasasında Izin verme](#grant-permissions-to-the-backup-vault-on-storage-accounts)bölümünde daha ayrıntılı olarak depolama hesaplarında yedekleme yapılandırmak için gerekli izinlere sahip olduğundan emin olun.

    ![Yedeklenecek kaynakları seçin](./media/blob-backup-configure-manage/select-resources.png)

    Yedek, kasasının seçili depolama hesaplarında yedekleme yapılandırmasına izin vermek için yeterli izinlere sahip olup olmadığını denetler.

    ![Yedekleme izinleri doğrular](./media/blob-backup-configure-manage/validate-permissions.png)

    Doğrulama hatalar (ekran görüntüsündeki depolama hesaplarından birinde olduğu gibi) ile sonuçlanırsa, seçilen depolama hesaplarına gidin ve [burada](#grant-permissions-to-the-backup-vault-on-storage-accounts)açıklandığı gibi uygun rolleri atayın ve **yeniden doğrula**' yı seçin. Yeni rol atamasının etkili olması 10 dakika kadar sürebilir.

1. Tüm seçili depolama hesapları için doğrulama başarılı olduktan sonra, yedeklemeyi yapılandırmak için **İnceleme ve yapılandırma** yapmaya devam edin. Korumayı yapılandırmanın durumunu ve tamamlanmasını bildiren bildirimler görürsünüz.

## <a name="effects-on-backed-up-storage-accounts"></a>Yedeklenen depolama hesaplarının etkileri

Yedekleme yapılandırıldıktan sonra, depolama hesaplarındaki blok Blobları üzerinde gerçekleşen değişiklikler izlenir ve veriler yedekleme ilkesine göre tutulur. Yedeklemenin yapılandırıldığı depolama hesaplarında aşağıdaki değişiklikleri fark edeceksiniz:

- Depolama hesabında aşağıdaki yetenekler etkinleştirilmiştir. Bunlar, depolama hesabının **veri koruma** sekmesinde görüntülenebilir.
  - Kapsayıcılar için zaman içinde geri yükleme: yedekleme ilkesinde belirtildiği şekilde bekletme Ile
  - Blob 'lar için geçici silme: yedekleme ilkesinde belirtilen şekilde bekletme Ile 5 gün
  - Blob 'lar için sürüm oluşturma
  - Blob değişiklik akışı

  Yedekleme için yapılandırılmış depolama hesabı zaten  **kapsayıcılar için bir zaman için geri yükleme** veya **BLOB 'lar için geçici silme** (Yedekleme yapılandırıldıktan önce) için zaten bir işaret içeriyorsa yedekleme, saklama 'nin yedekleme ilkesinde tanımlandığı şekilde en az düzeyde olmasını sağlar. Bu nedenle, her özellik için:

  - Yedekleme ilkesindeki bekletme, depolama hesabında başlangıçta bulunan bekletenden fazlaysa: depolama hesabındaki bekletme, yedekleme ilkesine göre değiştirilir
  - Yedekleme ilkesindeki bekletme, depolama hesabında başlangıçta bulunan saklama süresinden daha azsa: depolama hesabındaki bekletme, ilk ayarlanan süre içinde değişmeden bırakılır.

  ![Veri koruma sekmesi](./media/blob-backup-configure-manage/data-protection.png)

- Korumalı depolama hesabında yedekleme tarafından bir **silme kilidi** uygulanır. Kilit, depolama hesabını yanlışlıkla silmenin durumlarında karşı korumaya yöneliktir. Bu, **depolama hesabı**  >  **kilitleri** altında görüntülenebilir.

    ![Silme kilitleri](./media/blob-backup-configure-manage/delete-lock.png)

## <a name="manage-operational-backup"></a>İşletimsel yedeklemeyi yönetme

Yedekleme merkezini, tüm yedeklemelerinizi yönetmek için tek bir cam bölmesinize göre kullanabilirsiniz. Azure Blobları için işlemsel yedekleme ile ilgili olarak aşağıdakileri gerçekleştirmek üzere yedekleme merkezi 'ni kullanabilirsiniz:

- Yukarıda gördüğünüz gibi, yedekleme kasaları ve ilkeleri oluşturmak için kullanabilirsiniz. Ayrıca, tüm kasaların ve ilkelerin seçili Abonelikler altında de görüntüleyebilirsiniz.
- Yedekleme merkezi, korunan depolama hesaplarının korunmasından ve yedeklemenin durumunu izlemenin kolay bir yolunu ve yedekleme 'nin şu anda yapılandırılmadığı depolama hesaplarını izlemenizi sağlar.
- Herhangi bir depolama hesabı için yedeklemeyi, **+ Backup** düğmesini kullanarak yapılandırabilirsiniz.
- **Geri yükle** düğmesini kullanarak geri yüklemeyi başlatabilir ve **yedekleme Işlerini** kullanarak geri yükleme işlemini izleyebilirsiniz. Geri yüklemeler gerçekleştirme hakkında daha fazla bilgi için bkz. [Azure Bloblarını geri yükleme](blob-backup-support-matrix.md).
- Yedekleme raporlarını kullanarak yedekleme kullanımınızı çözümleyin.

    ![Yedekleme Merkezi](./media/blob-backup-configure-manage/backup-center.png)

Daha fazla bilgi için bkz. [yedekleme merkezi 'Ne genel bakış (Önizleme)](backup-center-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Bloblarını geri yükleme](blob-backup-support-matrix.md)
