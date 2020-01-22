---
title: Azure dosya paylaşımı yedeklemelerini yönetme
description: Bu makalede, Azure Backup hizmeti tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için kullanılan ortak görevler açıklanmaktadır.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: a5477d021b6e3600693e183d8707e11592b7cc38
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294558"
---
# <a name="manage-azure-file-share-backups"></a>Azure dosya paylaşımı yedeklemelerini yönetme

Bu makalede, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için kullanılan ortak görevler açıklanmaktadır. Kurtarma Hizmetleri kasasında aşağıdaki yönetim görevlerinin nasıl yürütüleceğini öğreneceksiniz:

* [İşleri izleme](#monitor-jobs)
* [Yeni ilke oluşturma](#create-a-new-policy)
* [Ilkeyi Değiştir](#modify-policy)
* [Dosya paylaşımı üzerindeki korumayı durdurma](#stop-protection-on-a-file-share)
* [Dosya paylaşımı üzerindeki korumayı sürdürme](#resume-protection-on-a-file-share)
* [Yedekleme verilerini silme](#delete-backup-data)
* [Depolama hesabının kaydını sil](#unregister-storage-account)

## <a name="monitor-jobs"></a>İşleri izle

Bir yedekleme veya geri yükleme işlemi tetiklemeniz durumunda, yedekleme hizmeti izleme için bir iş oluşturur. **Yedekleme İşleri** sayfasında tüm işlerin ilerleme durumunu izleyebilirsiniz.

**Yedekleme İşleri** sayfasını açmak için:

1. Dosya paylaşımlarınız için yedeklemeyi yapılandırmak üzere kullandığınız kurtarma hizmetleri kasasını açın. **Genel bakış** dikey penceresinde, **Izleme** bölümü altında **yedekleme işleri** ' ne tıklayın.

   ![Izleme bölümünde yedekleme Işleri](./media/manage-afs-backup/backup-jobs.png)

2. Tamam ' a tıkladığınızda, tüm işlerin durumunu listelemek için **yedekleme işleri** dikey penceresi görüntülenir. İzlemek istediğiniz dosya paylaşımında karşılık gelen iş yükü adına tıklayabilirsiniz.

   ![İş yükü adı](./media/manage-afs-backup/workload-name.png)

## <a name="create-a-new-policy"></a>Yeni ilke oluşturma

Kurtarma Hizmetleri kasasının **yedekleme ilkeleri** bölümünden Azure dosya paylaşımlarını yedeklemek için yeni bir ilke oluşturabilirsiniz. Dosya paylaşımları için yedeklemeyi yapılandırdığınız sırada oluşturulan tüm ilkeler, Azure dosya paylaşımı Ilke türü ile görünür.

Mevcut Yedekleme ilkelerini görüntülemek için:

1. Dosya paylaşımının yedeklemesini yapılandırmak için kullandığınız kurtarma hizmetleri kasasını açın ve kurtarma hizmetleri Kasası menüsünde, Yönet bölümünde **yedekleme ilkeleri** ' ne tıklayın. Kasada yapılandırılan tüm yedekleme ilkeleri listelenecektir.

   ![Tüm yedekleme ilkeleri](./media/manage-afs-backup/all-backup-policies.png)

2. Azure dosya paylaşımıyla ilgili ilkeleri görüntülemek için sağ üst köşedeki açılan listeden **Azure dosya paylaşma** ' yı seçin.

   ![Azure dosya paylaşma seçin](./media/manage-afs-backup/azure-file-share.png)

Yeni bir Yedekleme ilkesi oluşturmak için:

1. Yedekleme Ilkeleri dikey penceresinde **+ Ekle** ' ye tıklayın.

   ![Yeni yedekleme Ilkesi](./media/manage-afs-backup/new-backup-policy.png)

2. **Ekle** dikey penceresinde **Ilke türü** olarak **Azure dosya paylaşma** ' yı seçin. Azure dosya paylaşımının yedekleme ilkesi dikey penceresi açılır. Kurtarma noktaları için ilke adı, yedekleme sıklığı ve bekletme aralığını belirtin. İlkeyi tanımladığınızda **Tamam** ' ı tıklatın.

   ![Yedekleme ilkesini tanımlama](./media/manage-afs-backup/define-backup-policy.png)

## <a name="modify-policy"></a>İlkeyi Değiştir

Yedekleme sıklığını veya bekletme aralığını değiştirmek için bir yedekleme ilkesi değiştirebilirsiniz.

Bir ilkeyi değiştirmek için:

1. Dosya paylaşımının yedeklemesini yapılandırmak için kullandığınız kurtarma hizmetleri kasasını açın ve kurtarma hizmetleri Kasası menüsünde, Yönet bölümünün altındaki **yedekleme ilkeleri** ' ne tıklayın. Kasada yapılandırılan tüm yedekleme ilkeleri listelenecektir.

   ![Kasadaki tüm yedekleme ilkeleri](./media/manage-afs-backup/all-backup-policies-modify.png)

2. Bir Azure dosya paylaşımıyla ilgili ilkeleri görüntülemek için sağ üst köşedeki açılan listeden **Azure dosya paylaşma** ' yı seçin. Değiştirmek istediğiniz yedekleme ilkesine tıklayın.

   ![Değiştirilecek Azure dosya paylaşma](./media/manage-afs-backup/azure-file-share-modify.png)

3. **Zamanlama** dikey penceresi açılır. Yedekleme zamanlaması/bekletme aralığını gereken şekilde düzenleyin ve **Kaydet**' e tıklayın. Dikey pencerede bir "devam eden güncelleştirme" iletisi görürsünüz ve ilke değişikliklerinin başarıyla güncelleştirildiği zaman, "yedekleme ilkesini başarıyla güncelleştirdiniz" iletisini görürsünüz.

   ![Değiştirilen ilkeyi Kaydet](./media/manage-afs-backup/save-policy.png)

## <a name="stop-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı durdurma

Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

* Gelecekteki tüm yedekleme işlerini Durdur ve *tüm kurtarma noktalarını Sil*
* Gelecekteki tüm yedekleme işlerini Durdur *, ancak kurtarma noktalarını bırak*

Azure Backup tarafından oluşturulan temeldeki anlık görüntüler bekletileceği için, kurtarma noktalarını depolamada bırakma ile ilişkili bir maliyet olabilir. Bununla birlikte, kurtarma noktalarını bırakmanın avantajı, daha sonra isterseniz Dosya paylaşımını geri yükleyebilmenizdir. Kurtarma noktalarından ayrılma maliyeti hakkında daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/backup/). Tüm kurtarma noktalarını silmeyi seçerseniz, Dosya paylaşımını geri yükleyemezsiniz.

Azure dosya paylaşımının korumasını durdurmak için:

1. Dosya paylaşma kurtarma noktalarını içeren kurtarma hizmetleri kasasını açın ve korumalı öğeler altında **öğeleri yedekle** ' ye tıklayın. Yedekleme Öğesi türlerinin listesi gösterilir.

   ![Yedekleme öğeleri](./media/manage-afs-backup/backup-items.png)

2. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. **(Azure depolama (Azure dosyaları)) Için Yedekleme öğelerinin** listesi görüntülenir.

   ![Azure depolama (Azure dosyaları) seçeneğini belirleyin](./media/manage-afs-backup/azure-storage-azure-files.png)

3. **Yedekleme öğeleri (Azure depolama (Azure dosyaları))** listesinde, korumasını durdurmak istediğiniz yedekleme öğesini seçin.

4. **Yedekleme öğesi** dikey penceresinin menüsünde **Yedeklemeyi Durdur** seçeneğini belirleyin.

   ![Yedeklemeyi Durdur ' u seçin](./media/manage-afs-backup/stop-backup.png)

5. **Yedeklemeyi Durdur** dikey penceresinde, **yedekleme verilerini tutmayı** veya **yedekleme verilerini silmeyi** seçin ve **Yedeklemeyi Durdur**' a tıklayın.

    ![Yedekleme verilerini sakla veya Sil ' i seçin](./media/manage-afs-backup/retain-or-delete-backup-data.png)

## <a name="resume-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı sürdürme

Dosya paylaşımının koruması durdurulduğunda **yedek verileri sakla** seçeneği seçilmişse, korumayı sürdürmek mümkündür. **Yedekleme Verilerini Sil** seçeneği belirtildiyse, dosya paylaşımının koruması sürdürülemez.

Azure dosya paylaşımının korumasını sürdürmesini sağlamak için:

1. Dosya paylaşma kurtarma noktalarını içeren kurtarma hizmetleri kasasını açın ve korumalı öğeler altında **öğeleri yedekle** ' ye tıklayın. Yedekleme Öğesi türlerinin listesi gösterilir.

   ![Sürdürülmesi için yedekleme öğeleri](./media/manage-afs-backup/backup-items-resume.png)

2. **Yedekleme Yönetimi Türü** listesinde **Azure Depolama (Azure Dosyaları)** öğesini seçin. **(Azure depolama (Azure dosyaları)) Için Yedekleme öğelerinin** listesi görüntülenir.

   ![Azure depolama (Azure dosyaları) listesi](./media/manage-afs-backup/azure-storage-azure-files.png)

3. **Yedekleme öğeleri (Azure depolama (Azure dosyaları))** listesinde, korumayı yeniden başlatmak istediğiniz yedekleme öğesini seçin.

4. **Yedekleme öğesi** dikey penceresinin menüsünde **yedeklemeyi yeniden başlatma** seçeneğini belirleyin.

   ![Yedeklemeyi sürdürür ' ı seçin](./media/manage-afs-backup/resume-backup.png)

5. **Yedekleme ilkesi** dikey penceresi açılır ve yedeklemeyi sürdürecek bir ilke seçebilirsiniz.

6. İstenen **yedekleme ilkesini**seçtikten sonra **Kaydet** ' e tıklayın. Portalda bir "devam eden güncelleştirme" iletisi görürsünüz ve yedekleme başarıyla sürdürüldüğünde, "korumalı Azure dosya paylaşımının yedekleme Ilkesi başarıyla güncelleştirildi" iletisini görürsünüz.

   ![Yedekleme ilkesi başarıyla güncelleştirildi](./media/manage-afs-backup/successfully-updated.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme

**Yedeklemeyi durdurma** işi sırasında veya korumayı durdurduktan sonra herhangi bir zamanda bir dosya paylaşımının yedeklemesini silebilirsiniz. Hatta kurtarma noktalarını silmeden önce birkaç gün veya birkaç hafta beklemek yararlı olabilir. Yedekleme verileri silinirken, silinecek belirli kurtarma noktalarını seçemezsiniz. Yedekleme verilerinizi silmeyi seçerseniz, dosya paylaşımıyla ilişkili tüm kurtarma noktalarını silersiniz.

Aşağıdaki yordamda dosya paylaşımında korumanın durdurulduğunu varsayılmaktadır.

Azure dosya paylaşımının yedekleme verilerini silmek için:

1. Yedekleme işi durdurulduğunda, yedekleme **öğesi** panosunda yedekleme **ve** **yedekleme verilerini silme** seçenekleri kullanılabilir. **Yedekleme öğesi** dikey penceresinin menüsündeki **yedekleme verilerini sil** seçeneğine tıklayın.

   ![Yedekleme verilerini silme](./media/manage-afs-backup/delete-backup-data.png)

2. **Yedekleme verilerini sil** dikey penceresi açılır. Silmeyi onaylamak için dosya paylaşımının adını yazın. İsteğe bağlı olarak, silmek veya **yorum**yapmak Için bir **neden** belirtin. Yedekleme verilerinin silinmesinden emin olduktan sonra **Sil** ' e tıklayın.

   ![Veri Silmeyi Onayla](./media/manage-afs-backup/confirm-delete-data.png)

## <a name="unregister-storage-account"></a>Depolama hesabının kaydını sil

Farklı bir kurtarma hizmetleri Kasası kullanarak belirli bir depolama hesabındaki dosya paylaşımlarınızı korumak istiyorsanız, önce bu depolama hesabındaki [tüm dosya paylaşımları için korumayı durdurun](#stop-protection-on-a-file-share) . Ardından, koruma için kullanılan geçerli kurtarma hizmetleri kasasından hesabın kaydını silin.

Aşağıdaki yordamda, kaldırmak istediğiniz depolama hesabındaki tüm dosya paylaşımları için korumanın durdurulduğunu varsayılmaktadır.

Depolama hesabının kaydını silmek için:

1. Depolama hesabınızın kaydedildiği kurtarma hizmetleri kasasını açın.
2. **Genel bakış** dikey penceresinin **Yönet** bölümünün altındaki **Yedekleme altyapısı** seçeneğine tıklayın.

   ![Yedekleme Altyapısı ' na tıklayın](./media/manage-afs-backup/backup-infrastructure.png)

3. **Yedekleme altyapısı** dikey penceresi açılır. Bu dikey penceredeki **Azure Storage hesapları** bölümünde **depolama hesapları** ' na tıklayın.

   ![Depolama hesapları ' na tıklayın](./media/manage-afs-backup/storage-accounts.png)

4. **Depolama hesapları**' na tıkladığınızda kasayla birlikte kaydedilen depolama hesaplarının bir listesi görüntülenir.
5. Kaydını kaldırmak istediğiniz depolama hesabına sağ tıklayın ve **kaydı sil**' i seçin.

   ![Kayıt silmeyi Seç](./media/manage-afs-backup/select-unregister.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure dosya paylaşımları Için yedekleme/geri yükleme hatalarıyla Ilgili sorunları giderme](https://docs.microsoft.com/azure/backup/troubleshoot-azure-files)
