---
title: Azure VM 'lerinde yedeklenen SAP HANA veritabanlarını yönetme
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarını yönetmek ve izlemek için ortak görevler hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: f76054c7c78c55a9754975267ee4fa3caab968a3
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74288353"
---
# <a name="manage-and-monitor-backed-up-sap-hana-databases"></a>Yedeklenen SAP HANA veritabanlarını yönetme ve izleme

Bu makalede, bir Azure sanal makinesinde (VM) çalışan ve [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından Azure Backup kurtarma hizmetleri kasasına yedeklenen SAP HANA veritabanlarını yönetmek ve izlemek için kullanılan ortak görevler açıklanmaktadır. İşlerin ve uyarıların nasıl izleneceğini, isteğe bağlı bir yedekleme tetiklemeyi, ilkeleri düzenlemenizi, veritabanı korumasını durdurup sürdürmeyi ve bir VM 'nin yedeklerden kaydını nasıl sileceğinizi öğreneceksiniz.

Yedeklemeleri henüz SAP HANA veritabanları için yapılandırmadıysanız, bkz. [Azure VM 'lerinde SAP HANA veritabanlarını yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Portalda el ile yedekleme işlerini izleme

Azure Backup Azure portal tüm el ile tetiklenen işleri, **yedekleme işleri** bölümünde gösterir.

![Yedekleme işleri bölümü](./media/sap-hana-db-manage/backup-jobs.png)

Bu portalda gördüğünüz işler veritabanı bulma ve kaydetme, yedekleme ve geri yükleme işlemlerini içerir. Bu bölümde günlük yedeklemeleri dahil olmak üzere zamanlanmış işler gösterilmez. SAP HANA yerel istemcilerden el ile tetiklenen yedeklemeler (Studio/kokpit/DBA kokpiti) burada da gösterilmez.

![Yedekleme işleri listesi](./media/sap-hana-db-manage/backup-jobs-list.png)

İzleme hakkında daha fazla bilgi edinmek için [Azure izleyici kullanarak](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)Azure Portal ve izleme ' [de izleme](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) ' ye gidin.

## <a name="view-backup-alerts"></a>Yedekleme uyarılarını görüntüle

Uyarılar SAP HANA veritabanlarının yedeklerini izlemenin kolay bir yöntemidir. Uyarılar, bir yedeklemenin oluşturduğu çok sayıda olayın kaybedilmesi gerekmeden en iyi şekilde ilgilendiğiniz olaylara odaklanmaya yardımcı olur. Azure Backup, uyarıları ayarlamanıza olanak sağlar ve aşağıdaki gibi izlenebilir:

* [Azure portalında](https://portal.azure.com/) oturum açın.
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
6. Veritabanının geçici bir tam yedeklemesini alın.
7. Tam yedekleme ve Katalog yedeklemesinin bitmesini bekleyin.
8. Önceki ayarları Azure için geri döndürür:
   * **Enable_auto_log_backup** **Evet**olarak ayarlayın.
   * **Log_backup_using_backint** **true**olarak ayarlayın.

### <a name="edit-underlying-policy"></a>Temel alınan ilkeyi Düzenle

Yedekleme sıklığını veya bekletme aralığını değiştirmek için ilkeyi değiştirin:

* Kasa panosunda > **yedekleme Ilkelerini** **Yönet** ' e gidin

  ![Kasa panosunda yedekleme ilkeleri](./media/sap-hana-db-manage/backup-policies-dashboard.png)

* Düzenlemek istediğiniz ilkeyi seçin:

  ![Yedekleme ilkelerinin listesi](./media/sap-hana-db-manage/backup-policies-list.png)

  ![Yedekleme ilkesi ayrıntıları](./media/sap-hana-db-manage/backup-policy-details.png)

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

