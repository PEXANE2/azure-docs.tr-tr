---
title: Azure VM 'lerinde yedeklenen SAP HANA veritabanlarını yönetme
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarını yönetmek ve izlemek için ortak görevler hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: a9462f8608fc5ae35255ac321a0742b3f1834fde
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75390598"
---
# <a name="manage-and-monitor-backed-up-sap-hana-databases"></a>Yedeklenen SAP HANA veritabanlarını yönetme ve izleme

Bu makalede, bir Azure sanal makinesinde (VM) çalışan ve [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından Azure Backup kurtarma hizmetleri kasasına yedeklenen SAP HANA veritabanlarını yönetmek ve izlemek için kullanılan ortak görevler açıklanmaktadır. İşlerin ve uyarıların nasıl izleneceğini, isteğe bağlı bir yedekleme tetiklemeyi, ilkeleri düzenlemenizi, veritabanı korumasını durdurup sürdürmeyi ve bir VM 'nin yedeklerden kaydını nasıl sileceğinizi öğreneceksiniz.

Yedeklemeleri henüz SAP HANA veritabanları için yapılandırmadıysanız, bkz. [Azure VM 'lerinde SAP HANA veritabanlarını yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Portalda el ile yedekleme işlerini izleme

Azure Backup Azure portal tüm el ile tetiklenen işleri, **yedekleme işleri** bölümünde gösterir.

![Yedekleme işleri bölümü](./media/sap-hana-db-manage/backup-jobs.png)

Bu portalda gördüğünüz işler veritabanı bulma ve kaydetme, yedekleme ve geri yükleme işlemlerini içerir. Bu bölümde günlük yedeklemeleri dahil olmak üzere zamanlanmış işler gösterilmez. SAP HANA yerel istemcilerden el ile tetiklenen yedeklemeler (Studio/kokpit/DBA kokpiti) da burada gösterilmez.

![Yedekleme işleri listesi](./media/sap-hana-db-manage/backup-jobs-list.png)

İzleme hakkında daha fazla bilgi edinmek için [Azure izleyici kullanarak](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)Azure Portal ve izleme ' [de izleme](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) ' ye gidin.

## <a name="view-backup-alerts"></a>Yedekleme uyarılarını görüntüle

Uyarılar SAP HANA veritabanlarının yedeklerini izlemenin kolay bir yöntemidir. Uyarılar, bir yedeklemenin oluşturduğu çok sayıda olayın kaybedilmesi gerekmeden en iyi şekilde ilgilendiğiniz olaylara odaklanmaya yardımcı olur. Azure Backup, uyarıları ayarlamanıza olanak sağlar ve aşağıdaki gibi izlenebilir:

* [Azure Portal](https://portal.azure.com/)’ında oturum açın.
* Kasa panosunda **yedekleme uyarıları**' nı seçin.

  ![Kasa panosunda yedekleme uyarıları](./media/sap-hana-db-manage/backup-alerts-dashboard.png)

* Uyarıları görebileceksiniz:

  ![Yedekleme uyarıları listesi](./media/sap-hana-db-manage/backup-alerts-list.png)

* Daha fazla ayrıntı görmek için uyarılara tıklayın:

  ![Uyarı ayrıntıları](./media/sap-hana-db-manage/alert-details.png)

Bugün Azure Backup, e-posta ile uyarıların gönderilmesini sağlar. Bu uyarılar şunlardır:

* Tüm yedekleme hatalarıyla ilgili olarak tetiklendi.
* Veritabanı düzeyinde hata koduna göre birleştirildi.
* Yalnızca bir veritabanının ilk yedekleme hatası için gönderilir.

İzleme hakkında daha fazla bilgi edinmek için [Azure izleyici 'yi kullanarak](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)Azure Portal ve izleme ' [de izleme](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) ' ye gidin.

## <a name="management-operations"></a>Yönetim Işlemleri

Azure Backup, yedeklenen bir SAP HANA veritabanının yönetimini desteklediği çok sayıda yönetim işlemi ile kolay hale getirir. Bu işlemler aşağıdaki bölümlerde daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="run-an-ad-hoc-backup"></a>Geçici yedekleme çalıştırma

Yedeklemeler, ilke zamanlamasına uygun olarak çalışır. İsteğe bağlı bir yedeklemeyi aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde, **yedekleme öğeleri**' ne tıklayın.
2. **Yedekleme öğeleri**' nde, SAP HANA VERITABANıNı çalıştıran VM 'yi seçin ve **Şimdi Yedekle**' ye tıklayın.
3. **Şimdi Yedekle**' de, kurtarma noktasının tutulacağı son günü seçmek için Takvim denetimini kullanın. Daha sonra, **Tamam**'a tıklayın.
4. Portal bildirimlerini izleyin. İş ilerlemesini kasa panosunda izleyebilirsiniz > **yedekleme işleri** > **devam**ediyor. Veritabanınızın boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

### <a name="run-sap-hana-native-client-backup-on-a-database-with-azure-backup-enabled"></a>Azure Backup etkin olan bir veritabanında SAP HANA yerel istemci yedeklemesi çalıştırma

Azure Backup yedeklenmekte olan bir veritabanının yerel yedeklemesini (HANA Studio/kokpit kullanarak) almak istiyorsanız aşağıdakileri yapın:

1. Veritabanının tamamlaması için herhangi bir tam veya günlük yedeklemesi bekleyin. SAP HANA Studio/Kokpit içindeki durumu denetleyin.
2. Günlük yedeklemelerini devre dışı bırakın ve ilgili veritabanı için yedekleme kataloğunu dosya sistemine ayarlayın.
3. Bunu yapmak için **SystemDB** > **yapılandırması** ' na çift tıklayın > veritabanı > **filtresi (günlük)** **seçeneğini belirleyin** .
4. **Enable_auto_log_backup** **Hayır**olarak ayarlayın.
5. **Log_backup_using_backint** **false**olarak ayarlayın.
6. Veritabanının isteğe bağlı tam yedeklemesini yapın.
7. Tam yedekleme ve Katalog yedeklemesinin bitmesini bekleyin.
8. Önceki ayarları Azure için geri döndürür:
   * **Enable_auto_log_backup** **Evet**olarak ayarlayın.
   * **Log_backup_using_backint** **true**olarak ayarlayın.

### <a name="change-policy"></a>İlkeyi Değiştir

SAP HANA yedekleme öğesi için temel olan ilkeyi değiştirebilirsiniz.

* Kasa panosunda **yedekleme öğeleri**' ne gidin:

  ![Yedekleme öğelerini seçin](./media/sap-hana-db-manage/backup-items.png)

* **Azure VM 'de SAP HANA** seçin

  ![Azure VM 'de SAP HANA seçin](./media/sap-hana-db-manage/sap-hana-in-azure-vm.png)

* Temel ilkesini değiştirmek istediğiniz yedekleme öğesini seçin
* Mevcut yedekleme ilkesine tıklayın

  ![Mevcut yedekleme ilkesini seçin](./media/sap-hana-db-manage/existing-backup-policy.png)

* Listeden seçim yaparak ilkeyi değiştirin. Gerekirse [Yeni bir yedekleme Ilkesi oluşturun](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database#create-a-backup-policy) .

  ![Açılan listeden ilke ' yi seçin](./media/sap-hana-db-manage/choose-backup-policy.png)

* Değişiklikleri Kaydet

  ![Değişiklikleri Kaydet](./media/sap-hana-db-manage/save-changes.png)

* İlke değişikliği, ilişkili tüm yedekleme öğelerini etkiler ve ilgili **yapılandırma koruma** işlerini tetikler.

>[!NOTE]
> Saklama döneminde yapılan herhangi bir değişiklik, yeni olanlar da içinde olmak üzere daha eski kurtarma noktalarına daha geriye dönük olarak uygulanır.
>
> Artımlı yedekleme ilkeleri SAP HANA veritabanları için kullanılamaz. Artımlı yedekleme şu anda bu veritabanları için desteklenmiyor.

### <a name="stop-protection-for-an-sap-hana-database"></a>SAP HANA veritabanı korumasını durdurma

SAP HANA veritabanının korunmasını birkaç yolla durdurabilirsiniz:

* Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını silin.
* Gelecekteki tüm yedekleme işlerini durdurun ve kurtarma noktalarını bozulmadan bırakın.

Kurtarma noktalarından ayrıldığınızda bu ayrıntıları göz önünde bulundurun:

* Tüm kurtarma noktaları süresiz olarak kalır, tüm ayıklama, verileri sakla ile korumayı durdurma sırasında durdurulur.
* Korumalı örnek ve tüketilen depolama alanı için ücretlendirilirsiniz. Daha fazla bilgi için bkz. [Azure Backup fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/).
* Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur.

Bir veritabanının korumasını durdurmak için:

* Kasa panosunda **yedekleme öğeleri**' ni seçin.
* **Yedekleme yönetimi türü**altında **Azure VM 'de SAP HANA** ' yi seçin.

  ![Azure VM 'de SAP HANA seçin](./media/sap-hana-db-manage/sap-hana-azure-vm.png)

* Korumasını durdurmak istediğiniz veritabanını seçin:

  ![Korumayı durdurmak için veritabanını seçin](./media/sap-hana-db-manage/select-database.png)

* Veritabanı menüsünde **Yedeklemeyi Durdur**' u seçin.

  ![Yedeklemeyi Durdur ' u seçin](./media/sap-hana-db-manage/stop-backup.png)

* **Yedeklemeyi Durdur** menüsünde, verileri tutmayı veya silmeyi seçin. İsterseniz, bir neden ve açıklama belirtin.

  ![Veri tutmayı veya silmeyi seçin](./media/sap-hana-db-manage/retain-backup-data.png)

* **Yedeklemeyi Durdur**' u seçin.

### <a name="resume-protection-for-an-sap-hana-database"></a>SAP HANA veritabanı için korumayı sürdürür

SAP HANA veritabanı için korumayı durdurduğunuzda, **yedekleme verilerini sakla** seçeneğini belirlerseniz, daha sonra korumayı devam ettirebilirsiniz. Yedeklenen verileri korumazsanız korumaya devam edemeyeceksiniz.

Bir SAP HANA veritabanı korumasını sürdürmesini sağlamak için:

* Yedekleme öğesini açın ve yedeklemeyi yeniden **başlatma**' yı seçin.

   ![Yedeklemeyi sürdürür ' ı seçin](./media/sap-hana-db-manage/resume-backup.png)

* **Yedekleme ilkesi** menüsünde bir ilke seçin ve ardından **Kaydet**' i seçin.

### <a name="upgrading-from-sap-hana-10-to-20"></a>SAP HANA 1,0 ' den 2,0 ' ye yükseltme

[SAP HANA 1,0 ' den 2,0 ' e yükselttikten sonra](backup-azure-sap-hana-database-troubleshoot.md#upgrading-from-sap-hana-10-to-20)bir SAP HANA veritabanı için yedeklemeye nasıl devam edeceğinizi öğrenin.

### <a name="upgrading-without-a-sid-change"></a>SID değişikliği olmadan yükseltme

[SID, yükseltmeden sonra değişmemiş](backup-azure-sap-hana-database-troubleshoot.md#upgrading-without-an-sid-change)bir SAP HANA veritabanının yedeğine nasıl devam edeceğinizi öğrenin.

### <a name="unregister-an-sap-hana-database"></a>SAP HANA veritabanının kaydını silme

Korumayı devre dışı bıraktıktan sonra, kasayı silmeden önce bir SAP HANA örneğinin kaydını silin:

* Kasa panosunda, **Yönet**altında, **Yedekleme altyapısı**' nı seçin.

   ![Yedekleme altyapısını seçin](./media/sap-hana-db-manage/backup-infrastructure.png)

* **Azure VM 'de Iş yükü** olarak **yedekleme yönetimi türünü** seçin

   ![Azure VM 'de Iş yükü olarak yedekleme yönetimi türünü seçin](./media/sap-hana-db-manage/backup-management-type.png)

* **Korumalı sunucular**' da, silmek için örneği seçin. Kasayı silmek için tüm sunucuların/örneklerin kaydını silmeniz gerekir.

* Korunan örneğe sağ tıklayıp **kayıt kaldır**' ı seçin.

   ![Kayıt silmeyi Seç](./media/sap-hana-db-manage/unregister.png)

## <a name="next-steps"></a>Sonraki adımlar

* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları gidermeye nasıl sorun](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot) alabileceğinizi öğrenin.
