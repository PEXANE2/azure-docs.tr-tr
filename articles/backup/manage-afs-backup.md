---
title: Azure dosya paylaşımı yedeklemelerini yönetme
description: Bu makalede, Azure Backup tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için kullanılan ortak görevler açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: cb764fa441c063328dc350cf26f42c5bc7a0ca99
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84712097"
---
# <a name="manage-azure-file-share-backups"></a>Azure dosya paylaşımı yedeklemelerini yönetme

Bu makalede, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için kullanılan ortak görevler açıklanmaktadır. Kurtarma Hizmetleri kasasında yönetim görevlerini nasıl gerçekleştireceğinizi öğreneceksiniz.

## <a name="monitor-jobs"></a>İşleri izleme

Bir yedekleme veya geri yükleme işlemi tetiklemeniz durumunda, yedekleme hizmeti izleme için bir iş oluşturur. **Yedekleme İşleri** sayfasında tüm işlerin ilerleme durumunu izleyebilirsiniz.

**Yedekleme İşleri** sayfasını açmak için:

1. Dosya paylaşımlarınız için yedeklemeyi yapılandırmak üzere kullandığınız kurtarma hizmetleri kasasını açın. **Genel bakış** bölmesinde, **Izleme** bölümü altında **yedekleme işleri** ' ni seçin.

   ![Izleme bölümünde yedekleme Işleri](./media/manage-afs-backup/backup-jobs.png)

1. **Tamam**' ı seçtikten sonra, **yedekleme işleri** bölmesi tüm işlerin durumunu listeler. İzlemek istediğiniz dosya paylaşımının karşılık gelen iş yükü adını seçin.

   ![İş yükü adı](./media/manage-afs-backup/workload-name.png)

## <a name="create-a-new-policy"></a>Yeni ilke oluşturma

Kurtarma Hizmetleri kasasının **yedekleme ilkeleri** bölümünden Azure dosya paylaşımlarını yedeklemek için yeni bir ilke oluşturabilirsiniz. Dosya paylaşımları için yedeklemeyi yapılandırdığınız sırada oluşturulan tüm ilkeler, **Azure dosya paylaşımı** **ilke türü** ile görünür.

Mevcut yedekleme ilkelerini görüntülemek için:

1. Dosya paylaşımının yedeklemesini yapılandırmak için kullandığınız kurtarma hizmetleri kasasını açın. Kurtarma Hizmetleri Kasası menüsünde, **Yönet** bölümünün altındaki **yedekleme ilkeleri** ' ni seçin. Kasada yapılandırılan tüm yedekleme ilkeleri görüntülenir.

   ![Tüm yedekleme ilkeleri](./media/manage-afs-backup/all-backup-policies.png)

1. **Azure dosya paylaşımıyla**ilgili ilkeleri görüntülemek için sağ üst köşedeki açılan listeden **Azure dosya paylaşma** ' yı seçin.

   ![Azure dosya paylaşma seçin](./media/manage-afs-backup/azure-file-share.png)

Yeni bir yedekleme ilkesi oluşturmak için:

1. **Yedekleme ilkeleri** bölmesinde **+ Ekle**' yi seçin.

   ![Yeni yedekleme ilkesi](./media/manage-afs-backup/new-backup-policy.png)

1. **Ekle** bölmesinde, **Ilke türü**olarak **Azure dosya paylaşma** ' yı seçin. **Azure dosya paylaşımının** **yedekleme ilkesi** bölmesi açılır. Kurtarma noktaları için ilke adı, yedekleme sıklığı ve bekletme aralığını belirtin. İlkeyi tanımladıktan sonra **Tamam**' ı seçin.

   ![Yedekleme ilkesini tanımlama](./media/manage-afs-backup/define-backup-policy.png)

## <a name="modify-policy"></a>İlkeyi Değiştir

Yedekleme sıklığını veya bekletme aralığını değiştirmek için bir yedekleme ilkesi değiştirebilirsiniz.

Bir ilkeyi değiştirmek için:

1. Dosya paylaşımının yedeklemesini yapılandırmak için kullandığınız kurtarma hizmetleri kasasını açın. Kurtarma Hizmetleri Kasası menüsünde, **Yönet** bölümünün altındaki **yedekleme ilkeleri** ' ni seçin. Kasada yapılandırılan tüm yedekleme ilkeleri görüntülenir.

   ![Kasadaki tüm yedekleme ilkeleri](./media/manage-afs-backup/all-backup-policies-modify.png)

1. Bir Azure dosya paylaşımıyla ilgili ilkeleri görüntülemek için sağ üst köşedeki açılan listeden **Azure dosya paylaşma** ' yı seçin. Değiştirmek istediğiniz yedekleme ilkesini seçin.

   ![Değiştirilecek Azure dosya paylaşma](./media/manage-afs-backup/azure-file-share-modify.png)

1. **Zamanlama** bölmesi açılır. **Yedekleme zamanlamasını** ve **bekletme aralığını** gereken şekilde düzenleyin ve **Kaydet**' i seçin. Bölmesinde bir "devam eden güncelleştirme" iletisi görürsünüz. İlke değiştirildikten sonra, "yedekleme ilkesi başarıyla güncelleştirildi." iletisini görürsünüz.

   ![Değiştirilen ilkeyi Kaydet](./media/manage-afs-backup/save-policy.png)

## <a name="stop-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı durdurma

Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

* Gelecekteki tüm yedekleme işlerini durdurun ve *tüm kurtarma noktalarını silin*.
* Gelecekteki tüm yedekleme işlerini durdurun, ancak *kurtarma noktalarından ayrılın*.

Azure Backup tarafından oluşturulan temeldeki anlık görüntüler korunduğundan, kurtarma noktalarını depolamada bırakma ile ilişkili bir maliyet olabilir. Kurtarma noktalarından ayrılmasının avantajı, daha sonra dosya paylaşımının geri yüklenmesini sağlayabilir. Kurtarma noktalarından ayrılma maliyeti hakkında daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/backup/). Tüm kurtarma noktalarını silmeye karar verirseniz dosya paylaşımından geri yükleme yapamazsınız.

Azure dosya paylaşımının korumasını durdurmak için:

1. Dosya paylaşma kurtarma noktalarını içeren kurtarma hizmetleri kasasını açın. **Korumalı öğeler** bölümünün altında **yedekleme öğeleri** ' ni seçin. Yedekleme öğesi türleri listesi görüntülenir.

   ![Yedekleme Öğeleri](./media/manage-afs-backup/backup-items.png)

1. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. **Yedekleme öğeleri (Azure depolama (Azure dosyaları))** listesi görüntülenir.

   ![Azure depolama (Azure dosyaları) seçeneğini belirleyin](./media/manage-afs-backup/azure-storage-azure-files.png)

1. **Yedekleme öğeleri (Azure depolama (Azure dosyaları))** listesinde, korumasını durdurmak istediğiniz yedekleme öğesini seçin.

1. **Yedeklemeyi Durdur** seçeneğini belirleyin.

   ![Yedeklemeyi Durdur ' u seçin](./media/manage-afs-backup/stop-backup.png)

1. **Yedeklemeyi Durdur** bölmesinde, **yedekleme verilerini sakla** veya **yedekleme verilerini sil**' i seçin. Sonra **Yedeklemeyi Durdur**' u seçin.

    ![Yedekleme verilerini sakla veya yedekleme verilerini Sil ' i seçin](./media/manage-afs-backup/retain-or-delete-backup-data.png)

## <a name="resume-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı sürdürme

Dosya paylaşımının koruması durdurulduğunda **yedek verileri sakla** seçeneği seçilmişse, korumayı sürdürmek mümkündür. **Yedekleme verilerini sil** seçeneği işaretliyse, dosya paylaşımının koruması sürdürülemez.

Azure dosya paylaşımının korumasını sürdürmesini sağlamak için:

1. Dosya paylaşma kurtarma noktalarını içeren kurtarma hizmetleri kasasını açın. **Korumalı öğeler** bölümünün altında **yedekleme öğeleri** ' ni seçin. Yedekleme öğesi türleri listesi görüntülenir.

   ![Sürdürülmesi için yedekleme öğeleri](./media/manage-afs-backup/backup-items-resume.png)

1. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. **Yedekleme öğeleri (Azure depolama (Azure dosyaları))** listesi görüntülenir.

   ![Azure depolama (Azure dosyaları) listesi](./media/manage-afs-backup/azure-storage-azure-files.png)

1. **Yedekleme öğeleri (Azure depolama (Azure dosyaları))** listesinde, korumayı yeniden başlatmak istediğiniz yedekleme öğesini seçin.

1. Yedeklemeyi yeniden **başlatma** seçeneğini belirleyin.

   ![Yedeklemeyi sürdürür ' ı seçin](./media/manage-afs-backup/resume-backup.png)

1. **Yedekleme ilkesi** bölmesi açılır. Yedeklemeyi yeniden başlatmak için istediğiniz ilkeyi seçin.

1. Bir yedekleme ilkesi seçtikten sonra **Kaydet**' i seçin. Portalda bir "devam eden güncelleştirme" iletisi görürsünüz. Yedekleme başarılı bir şekilde devam ettikten sonra, "korumalı Azure dosya paylaşımının yedekleme ilkesi başarıyla güncelleştirildi" iletisini görürsünüz.

   ![Yedekleme ilkesi başarıyla güncelleştirildi](./media/manage-afs-backup/successfully-updated.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme

**Yedeklemeyi** durdurma işi sırasında veya korumayı durdurduktan sonra herhangi bir zamanda bir dosya paylaşımının yedeklemesini silebilirsiniz. Kurtarma noktalarını silmeden önce, gün veya hafta boyunca hafta beklemeniz yararlı olabilir. Yedekleme verilerini sildiğinizde, silinecek belirli kurtarma noktalarını seçemezsiniz. Yedekleme verilerinizi silmeye karar verirseniz dosya paylaşımıyla ilişkili tüm kurtarma noktalarını silersiniz.

Aşağıdaki yordamda dosya paylaşımında korumanın durdurulduğu varsayılır.

Azure dosya paylaşımının yedekleme verilerini silmek için:

1. Yedekleme işi durdurulduktan sonra yedekleme **öğesi** panosunda yedekleme **ve** **yedekleme verilerini silme** seçenekleri kullanılabilir. **Yedekleme verilerini sil** seçeneğini belirleyin.

   ![Yedekleme verilerini silme](./media/manage-afs-backup/delete-backup-data.png)

1. **Yedekleme verilerini sil** bölmesi açılır. Silmeyi onaylamak için dosya paylaşımının adını girin. İsteğe bağlı olarak, **neden** veya **açıklamalar** kutularında daha fazla bilgi sağlayın. Yedekleme verilerini silme hakkında emin olduktan sonra **Sil**' i seçin.

   ![Veri Silmeyi Onayla](./media/manage-afs-backup/confirm-delete-data.png)

## <a name="unregister-a-storage-account"></a>Depolama hesabının kaydını silme

Farklı bir kurtarma hizmetleri Kasası kullanarak belirli bir depolama hesabındaki dosya paylaşımlarınızı korumak için, önce bu depolama hesabındaki [tüm dosya paylaşımlarının korumasını durdurun](#stop-protection-on-a-file-share) . Ardından, koruma için kullanılan geçerli kurtarma hizmetleri kasasından hesabın kaydını silin.

Aşağıdaki yordamda, kaldırmak istediğiniz depolama hesabındaki tüm dosya paylaşımları için korumanın durdurulduğu varsayılır.

Depolama hesabının kaydını silmek için:

1. Depolama hesabınızın kaydedildiği kurtarma hizmetleri kasasını açın.
1. **Genel bakış** bölmesinde, **Yönet** bölümünde **Yedekleme altyapısı** seçeneğini belirleyin.

   ![Yedekleme altyapısını seçin](./media/manage-afs-backup/backup-infrastructure.png)

1. **Yedekleme altyapısı** bölmesi açılır. **Azure depolama hesapları** bölümünde **depolama hesapları** ' nı seçin.

   ![Depolama hesaplarını seçin](./media/manage-afs-backup/storage-accounts.png)

1. **Depolama hesapları**' nı seçtikten sonra, kasaya kayıtlı depolama hesaplarının bir listesi görüntülenir.
1. Kaydını kaldırmak istediğiniz depolama hesabına sağ tıklayın ve **kayıt kaldır**' ı seçin.

   ![Kayıt silmeyi Seç](./media/manage-afs-backup/select-unregister.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure dosya paylaşımları yedeklemesi sorunlarını giderme](https://docs.microsoft.com/azure/backup/troubleshoot-azure-files).
