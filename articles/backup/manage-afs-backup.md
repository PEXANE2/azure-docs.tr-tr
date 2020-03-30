---
title: Azure dosya paylaşımı yedeklemelerini yönetme
description: Bu makalede, Azure Yedekleme tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için yaygın görevler açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: cb764fa441c063328dc350cf26f42c5bc7a0ca99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247663"
---
# <a name="manage-azure-file-share-backups"></a>Azure dosya paylaşımı yedeklemelerini yönetme

Bu makalede, [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için yaygın görevler açıklanmaktadır. Kurtarma Hizmetleri kasasında yönetim görevlerini nasıl yapacağınızı öğreneceksiniz.

## <a name="monitor-jobs"></a>İşleri izleme

Bir yedekleme veya geri yükleme işlemini tetiklediğinde, yedekleme hizmeti izleme için bir iş oluşturur. **Yedekleme İşleri** sayfasında tüm işlerin ilerleme durumunu izleyebilirsiniz.

**Yedekleme İşleri** sayfasını açmak için:

1. Dosya paylaşımlarınızın yedeklemesini yapılandırmak için kullandığınız Kurtarma Hizmetleri kasasını açın. Genel **Bakış** bölmesinde, **İzleme** bölümü altında **Yedek İşler'i** seçin.

   ![İzleme bölümünde Yedekleme İşleri](./media/manage-afs-backup/backup-jobs.png)

1. **Tamam'ı**seçtikten sonra **Yedek İşler** bölmesi tüm işlerin durumunu listeler. İzlemek istediğiniz dosya paylaşımına karşılık gelen iş yükü adını seçin.

   ![İş yükü adı](./media/manage-afs-backup/workload-name.png)

## <a name="create-a-new-policy"></a>Yeni ilke oluşturma

Kurtarma Hizmetleri kasasının Yedekleme ilkeleri bölümünden Azure dosya **paylaşımlarını** yedeklemek için yeni bir ilke oluşturabilirsiniz. Dosya paylaşımları için yedekleme yapılandırDığınızda oluşturulan tüm **ilkeler, İlke Türü** ile **Azure Dosya Paylaşımı**olarak gösteriş gösterir.

Varolan yedekleme ilkelerini görüntülemek için:

1. Dosya paylaşımı için yedeklemeyi yapılandırmak için kullandığınız Kurtarma Hizmetleri kasasını açın. Kurtarma Hizmetleri kasası menüsünde, **Yönet** bölümü altında **Yedekleme ilkeleri** seçin. Kasada yapılandırılan tüm yedekleme ilkeleri görüntülenir.

   ![Tüm yedekleme ilkeleri](./media/manage-afs-backup/all-backup-policies.png)

1. **Azure Dosya Paylaşımı'na**özgü ilkeleri görüntülemek için sağ üstteki açılır listeden **Azure Dosya Paylaşımı'nı** seçin.

   ![Azure Dosya Paylaşımı'nı seçin](./media/manage-afs-backup/azure-file-share.png)

Yeni bir yedekleme ilkesi oluşturmak için:

1. Yedekleme **ilkeleri** bölmesinde + **Ekle'yi**seçin.

   ![Yeni yedekleme ilkesi](./media/manage-afs-backup/new-backup-policy.png)

1. **Ekle** bölmesinde, **İlke Türü**olarak Azure Dosya **Paylaşımı'nı** seçin. **Azure Dosya Paylaşımı** için Yedekleme **ilkesi** bölmesi açılır. Kurtarma noktaları için ilke adını, yedekleme sıklığını ve bekletme aralığını belirtin. İlkeyi tanımladıktan sonra **Tamam'ı**seçin.

   ![Yedekleme ilkesini tanımlama](./media/manage-afs-backup/define-backup-policy.png)

## <a name="modify-policy"></a>İlkeyi değiştir

Yedekleme sıklığını veya bekletme aralığını değiştirmek için bir yedekleme ilkesini değiştirebilirsiniz.

Bir ilkeyi değiştirmek için:

1. Dosya paylaşımı için yedeklemeyi yapılandırmak için kullandığınız Kurtarma Hizmetleri kasasını açın. Kurtarma Hizmetleri kasası menüsünde, **Yönet** bölümü altında **Yedekleme ilkeleri** seçin. Kasada yapılandırılan tüm yedekleme ilkeleri görüntülenir.

   ![Kasadaki tüm yedekleme ilkeleri](./media/manage-afs-backup/all-backup-policies-modify.png)

1. Azure dosya paylaşımına özgü ilkeleri görüntülemek için sağ üstteki açılır listeden **Azure Dosya Paylaşımı'nı** seçin. Değiştirmek istediğiniz yedekleme ilkesini seçin.

   ![Değiştirmek için Azure dosya paylaşımı](./media/manage-afs-backup/azure-file-share-modify.png)

1. **Zamanlama** bölmesi açılır. **Yedekleme zamanlamasını** ve **Bekletme aralığını** gerektiği gibi düzenleyin ve **Kaydet'i**seçin. Bölmede bir "Devam Ediyor Güncelleştir" iletisi görürsünüz. İlke değişiklikleri başarıyla güncelleştirildikten sonra "Yedekleme ilkesini başarıyla güncelleştir" iletisini görürsünüz.

   ![Değiştirilen ilkeyi kaydetme](./media/manage-afs-backup/save-policy.png)

## <a name="stop-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı durdurma

Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

* Gelecekteki tüm yedekleme işlerini durdurun ve *tüm kurtarma noktalarını silin.*
* Gelecekteki tüm yedekleme işlerini *durdurun, ancak kurtarma noktalarını bırakın.*

Azure Yedekleme tarafından oluşturulan temel anlık görüntüler korunacağı için, kurtarma noktalarını depolama alanında bırakmanın bir maliyeti olabilir. Kurtarma noktalarını bırakmanın yararı, dosya paylaşımını daha sonra geri yükleyebilmektir. Kurtarma noktalarından çıkmanın maliyeti hakkında bilgi için [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/backup/)bakın. Tüm kurtarma noktalarını silmeye karar verirseniz, dosya paylaşımını geri yükleyemezsiniz.

Azure dosya paylaşımının korumasını durdurmak için:

1. Dosya paylaşımı kurtarma noktalarını içeren Kurtarma Hizmetleri kasasını açın. **Korumalı Öğeler** bölümünün altındaki **Yedek Öğeler'i** seçin. Yedek madde türleri listesi görüntülenir.

   ![Yedek Öğeler](./media/manage-afs-backup/backup-items.png)

1. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. **Yedekleme Öğeleri (Azure Depolama (Azure Dosyaları))** listesi görüntülenir.

   ![Azure Depolama (Azure Dosyaları) seçin](./media/manage-afs-backup/azure-storage-azure-files.png)

1. Yedekleme **Öğeleri (Azure Depolama (Azure Dosyaları))** listesinde, korumayı durdurmak istediğiniz yedekleme öğesini seçin.

1. **Yedeklemeyi Durdur** seçeneğini belirleyin.

   ![Yedeklemeyi Durdur'u seçin](./media/manage-afs-backup/stop-backup.png)

1. **Yedeklemeyi Durdur** bölmesinde **Yedek Verileri Koru** veya **Yedekleme Verilerini Sil'i**seçin. Ardından **Yedeklemeyi Durdur'u**seçin.

    ![Yedekleme Verilerini Koru'u seçin veya Yedekleme Verilerini Sil](./media/manage-afs-backup/retain-or-delete-backup-data.png)

## <a name="resume-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı sürdürme

Dosya paylaşımı için koruma durdurulduğunda **Yedekleme Verilerini Koru** seçeneği seçildiyse, korumaya devam etmek mümkündür. Yedekleme **Verilerini Sil** seçeneği seçildiyse, dosya paylaşımı için koruma devam edemez.

Azure dosya paylaşımı için korumayı sürdürmek için:

1. Dosya paylaşımı kurtarma noktalarını içeren Kurtarma Hizmetleri kasasını açın. **Korumalı Öğeler** bölümünün altındaki **Yedek Öğeler'i** seçin. Yedek madde türleri listesi görüntülenir.

   ![Özgeçmiş için yedekleme öğeleri](./media/manage-afs-backup/backup-items-resume.png)

1. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. **Yedekleme Öğeleri (Azure Depolama (Azure Dosyaları))** listesi görüntülenir.

   ![Azure Depolama (Azure Dosyaları) Listesi](./media/manage-afs-backup/azure-storage-azure-files.png)

1. Yedekleme **Öğeleri (Azure Depolama (Azure Dosyaları))** listesinde, korumayı sürdürmek istediğiniz yedekleme öğesini seçin.

1. Geri **Lekme yedekleme** seçeneğini seçin.

   ![Devam yedeklemeyi seçin](./media/manage-afs-backup/resume-backup.png)

1. **Yedekleme İlkesi** bölmesi açılır. Yedeklemeye devam etmek için seçtiğiniz bir ilke seçin.

1. Bir yedekleme ilkesi seçtikten sonra **Kaydet'i**seçin. Portalda bir "Devam Ediyor Güncelleştir" iletisi görürsünüz. Yedekleme başarıyla devam ettikten sonra "Korumalı Azure Dosya Paylaşımı için başarıyla güncelleştirilen yedekleme ilkesi" iletisini görürsünüz.

   ![Başarıyla güncelleştirilen yedekleme ilkesi](./media/manage-afs-backup/successfully-updated.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme

**Yedeklemeyi Durdur** iş sırasında veya korumayı durdurduktan sonra istediğiniz zaman bir dosya paylaşımının yedeklemesini silebilirsiniz. Kurtarma noktalarını silmeden önce günlerca, hatta haftalarca beklemek yararlı olabilir. Yedekleme verilerini sildiğinizde, silecek belirli kurtarma noktalarını seçemezsiniz. Yedekleme verilerinizi silmeye karar verirseniz, dosya paylaşımıyla ilişkili tüm kurtarma noktalarını silersiniz.

Aşağıdaki yordam, dosya paylaşımı için korumanın durdurulduğunu varsayar.

Azure dosya paylaşımı için yedekleme verilerini silmek için:

1. Yedekleme işi durdurulduktan sonra, **Yedekleme** Öğesi panosunda **Yedekleme yedeği ve** Sil yedekleme **verisi** seçenekleri kullanılabilir. Yedekleme **verilerini sil** seçeneğini seçin.

   ![Yedekleme verilerini silme](./media/manage-afs-backup/delete-backup-data.png)

1. **Yedekleme Verilerini Sil** bölmesi açılır. Silme işlemini onaylamak için dosya paylaşımının adını girin. İsteğe bağlı olarak, **Neden** veya **Açıklama** kutularında daha fazla bilgi sağlayın. Yedekleme verilerini sildiğinizden emin olduktan sonra **Sil'i**seçin.

   ![Verileri sil'i onayla](./media/manage-afs-backup/confirm-delete-data.png)

## <a name="unregister-a-storage-account"></a>Depolama hesabının kaydını açma

Farklı bir kurtarma hizmetleri kasası kullanarak belirli bir depolama hesabındaki dosya paylaşımlarınızı korumak için, önce bu depolama hesabındaki [tüm dosya paylaşımları için korumayı durdurun.](#stop-protection-on-a-file-share) Ardından, koruma için kullanılan geçerli kurtarma hizmetleri kasasından hesabı silin.

Aşağıdaki yordam, kaydını açmak istediğiniz depolama hesabındaki tüm dosya paylaşımları için korumanın durdurulduğunu varsayar.

Depolama hesabının kaydını çıkarmak için:

1. Depolama hesabınızın kayıtlı olduğu Kurtarma Hizmetleri kasasını açın.
1. Genel **Bakış** bölmesindeki, **Yönet** bölümünün altındaki **Yedek Altyapı** seçeneğini seçin.

   ![Yedekleme Altyapıseçin](./media/manage-afs-backup/backup-infrastructure.png)

1. **Yedekleme Altyapısı** bölmesi açılır. **Azure Depolama Hesapları** bölümünde Depolama **Hesapları'nı** seçin.

   ![Depolama Hesapları Seçin](./media/manage-afs-backup/storage-accounts.png)

1. **Depolama Hesapları'nı**seçtikten sonra kasaya kayıtlı depolama hesaplarının listesi görüntülenir.
1. Kaydını çıkarmak istediğiniz depolama hesabına sağ tıklayın ve **Kaydı Nıseçin.**

   ![Kaydı Nı Seçin](./media/manage-afs-backup/select-unregister.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için Bkz. [Sorun Gideri Azure dosya paylaşımları](https://docs.microsoft.com/azure/backup/troubleshoot-azure-files)yedeklemesi.
