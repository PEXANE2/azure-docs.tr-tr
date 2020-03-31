---
title: Azure VM'lerde yedeklenmiş SAP HANA veritabanlarını yönetme
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarını yönetmek ve izlemek için ortak görevleri öğrenin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: 89fd7f23163d301817e767771257d9bc6f4ed526
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480071"
---
# <a name="manage-and-monitor-backed-up-sap-hana-databases"></a>Yedeklenen SAP HANA veritabanlarını yönetme ve izleme

Bu makalede, Bir Azure sanal makinede (VM) çalışan ve [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview) hizmeti tarafından Azure Yedekleme Kurtarma Hizmetleri kasasına yedeklenen SAP HANA veritabanlarını yönetmek ve izlemek için kullanılan yaygın görevler açıklanmaktadır. İşleri ve uyarıları izlemeyi, isteğe bağlı yedeklemeyi tetiklemeyi, ilkeleri nasıl edineceğinizi, veritabanı korumasını durdurmayı ve devam ettirmeyi ve yedeklemelerden bir VM'yi silmeyi öğreneceksiniz.

SAP HANA veritabanlarınız için henüz yedekleme yapılandırmadıysanız, [Azure VM'lerinde SAP HANA veritabanlarını](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)yedekle'ye bakın.

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Portaldaki manuel yedekleme işlerini izleme

Azure Yedekleme, Azure portalındaki **Yedekleme işleri** bölümünde el ile tetiklenen tüm işleri gösterir.

![Yedek işler bölümü](./media/sap-hana-db-manage/backup-jobs.png)

Bu portalda gördüğünüz işler veritabanı bulma ve kaydetme ve yedekleme ve geri yükleme işlemleri içerir. Günlük yedeklemeleri de dahil olmak üzere zamanlanmış işler bu bölümde gösterilmez. SAP HANA yerel istemcilerinden (Studio/ Cockpit/ DBA Cockpit) elle tetiklenen yedeklemeler de burada görünmüyor.

![Yedek işler listesi](./media/sap-hana-db-manage/backup-jobs-list.png)

İzleme hakkında daha fazla bilgi edinmek için [Azure portalında İzleme](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) ve [Azure Monitörünü kullanarak İzleme'ye](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)gidin.

## <a name="view-backup-alerts"></a>Yedekleme uyarılarını görüntüleme

Uyarılar, SAP HANA veritabanlarının yedeklemelerini izlemenin kolay bir yoluolur. Uyarılar, yedeklemenin ürettiği çok sayıda olayda kaybolmadan en çok değer verdiğiniz olaylara odaklanmanıza yardımcı olur. Azure Yedekleme uyarıları ayarlamanızı sağlar ve bunlar aşağıdaki gibi izlenebilir:

* [Azure portalında](https://portal.azure.com/)oturum açın.
* Kasa panosunda **Yedek Uyarıları'nı**seçin.

  ![Kasa panosunda yedekleme uyarıları](./media/sap-hana-db-manage/backup-alerts-dashboard.png)

* Uyarıları görebilirsiniz:

  ![Yedekleme uyarıları listesi](./media/sap-hana-db-manage/backup-alerts-list.png)

* Daha fazla ayrıntı görmek için uyarıları tıklayın:

  ![Uyarı ayrıntıları](./media/sap-hana-db-manage/alert-details.png)

Azure Yedekleme bugün e-posta yoluyla uyarı gönderilmesine izin verir. Bu uyarılar şunlardır:

* Tüm yedekleme hataları için tetiklenir.
* Hata kodu ile veritabanı düzeyinde konsolide.
* Yalnızca bir veritabanının ilk yedekleme hatası için gönderildi.

Toİzleme hakkında daha fazla bilgi edinmek için [Azure portalında İzleme](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) ve [Azure Monitörünü kullanarak İzleme'ye](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)gidin.

## <a name="management-operations"></a>Yönetim İşlemleri

Azure Yedekleme, desteklediği çok sayıda yönetim işlemiyle yedeklenmiş SAP HANA veritabanının yönetimini kolaylaştırır. Bu işlemler aşağıdaki bölümlerde daha ayrıntılı olarak ele alınmıştır.

### <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

Yedeklemeler ilke zamanlamasına uygun olarak çalışır. İsteğe bağlı yedeklemeyi aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde Yedek **öğeleri**tıklatın.
2. **Yedekleme Öğeleri'nde**SAP HANA veritabanını çalıştıran VM'yi seçin ve şimdi **Yedekleme'yi**tıklatın.
3. **Yedekleme Şimdi,** kurtarma noktası nın korunması gereken son günü seçmek için takvim denetimini kullanın. Ardından **Tamam**'a tıklayın.
4. Portal bildirimlerini izleyin. Devam**Eden** **Yedekleme İşleri** > > kasa panosundaki iş ilerlemesini izleyebilirsiniz. Veritabanınızın boyutuna bağlı olarak, ilk yedeklemeyi oluşturmak biraz zaman alabilir.

### <a name="hana-native-client-integration"></a>HANA yerel istemci entegrasyonu

Şimdi, HANA yerel istemcilerinden herhangi birinden tetiklenen isteğe bağlı tam yedeklemeler **Yedekleme Öğeleri** sayfasında tam yedekleme olarak gösterecektir.

![Son yedeklemeler çalışır](./media/sap-hana-db-manage/last-backups.png)

Bu geçici tam yedeklemeler de geri yükleme noktaları listesinde gösterecektir.

![Geri yükleme noktaları listesi](./media/sap-hana-db-manage/list-restore-points.png)

### <a name="run-sap-hana-native-client-backup-on-a-database-with-azure-backup-enabled"></a>Azure yedekleme etkinleştirilmiş bir veritabanında SAP HANA yerel istemci yedeklemeçalıştırın

Azure Yedekleme ile yedeklenen bir veritabanının yerel bir yedeklemesini (HANA Studio / Kokpit kullanarak) almak istiyorsanız, aşağıdakileri yapın:

1. Veritabanının tamamlanması için tam veya günlük yedeklemelerini bekleyin. SAP HANA Studio/ Cockpit'deki durumu kontrol edin.
2. Günlük yedeklemelerini devre dışı bırak ve yedekleme kataloğunu ilgili veritabanı için dosya sistemine ayarlayın.
3. Bunu yapmak için **systemdb** > **Configuration** > **Select Database** > **Filtresi (Log)** çift tıklayın.
4. **enable_auto_log_backup'ı** **Hayır**olarak ayarlayın.
5. **False** **için log_backup_using_backint** ayarlayın.
6. Veritabanının isteğe bağlı tam yedeklemesini alın.
7. Tam yedekleme ve katalog yedeklemesinin tamamlanmasını bekleyin.
8. Önceki ayarları Azure ayarlarına geri döndürün:
   * **enable_auto_log_backup** **Evet**olarak ayarlayın.
   * **log_backup_using_backint** **True**olarak ayarlayın.

### <a name="change-policy"></a>İlkeyi değiştir

SAP HANA yedekleme öğesinin temel ilkesini değiştirebilirsiniz.

* Kasa panosunda, **Yedekleme öğelerine**gidin:

  ![Yedek öğeleri seçin](./media/sap-hana-db-manage/backup-items.png)

* **Azure VM'de SAP HANA'yı** seçin

  ![Azure VM'de SAP HANA'yı seçin](./media/sap-hana-db-manage/sap-hana-in-azure-vm.png)

* Temel ilkesini değiştirmek istediğiniz yedekleme öğesini seçin
* Varolan Yedekleme ilkesini tıklatın

  ![Varolan yedekleme ilkesini seçin](./media/sap-hana-db-manage/existing-backup-policy.png)

* Listeden seçerek ilkeyi değiştirin. Gerekirse [yeni bir yedekleme ilkesi oluşturun.](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database#create-a-backup-policy)

  ![Açılan listeden ilke seçin](./media/sap-hana-db-manage/choose-backup-policy.png)

* Değişiklikleri kaydetme

  ![Değişiklikleri kaydetme](./media/sap-hana-db-manage/save-changes.png)

* İlke değişikliği, ilişkili tüm Yedekleme Öğeleri'ni etkileyecek ve ilgili **yapılandırma koruma** işlerini tetikler.

>[!NOTE]
> Bekletme süresindeki herhangi bir değişiklik, yenilerinin yanı sıra tüm eski kurtarma noktalarına geriye dönük olarak uygulanacaktır.
>
> Sap HANA veritabanları için artımlı yedekleme ilkeleri kullanılamaz. Artımlı yedekleme şu anda bu veritabanları için desteklenmez.

### <a name="modify-policy"></a>İlkeyi Değiştir

Yedekleme türlerini, frekansları ve bekletme aralığını değiştirmek için ilkeyi değiştirin.

>[!NOTE]
>Bekletme dönemindeki herhangi bir değişiklik, yenilerine ek olarak tüm eski kurtarma noktalarına geriye dönük olarak uygulanır.

1. Kasa panosunda, **Yedekleme İlkeleri > Yönet'e** gidin ve yönetmek istediğiniz ilkeyi seçin.

   ![Dolandırılmak için ilkeyi seçin](./media/sap-hana-db-manage/manage-backup-policies.png)

1. **Değiştir'i**seçin.

   ![Değiştir'i seçin](./media/sap-hana-db-manage/modify-policy.png)

1. Yedekleme türlerinin sıklığını seçin.

   ![Yedekleme frekansı seçin](./media/sap-hana-db-manage/choose-frequency.png)

İlke değişikliği, ilişkili tüm yedekleme öğelerini etkileyecek ve ilgili **yapılandırma koruma** işlerini tetikler.

### <a name="inconsistent-policy"></a>Tutarsız ilke

Bazen bir değişiklik ilkesi işlemi bazı yedekleme öğeleri için **tutarsız** bir ilke sürümüne yol açabilir. Bu, bir değişiklik ilkesi işlemi tetiklendikten sonra ilgili **yapılandırma koruma** işi yedekleme öğesi için başarısız olduğunda olur. Yedekleme öğesi görünümünde aşağıdaki gibi görünür:

![Tutarsız ilke](./media/sap-hana-db-manage/inconsistent-policy.png)

Etkilenen tüm öğelerin ilke sürümünü tek bir tıklamayla düzeltebilirsiniz:

![İlke sürümünü düzeltme](./media/sap-hana-db-manage/fix-policy-version.png)

### <a name="stop-protection-for-an-sap-hana-database"></a>SAP HANA veritabanı için korumayı durdurma

SAP HANA veritabanını birkaç şekilde korumayı durdurabilirsiniz:

* Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını silin.
* Gelecekteki tüm yedekleme işlerini durdurun ve kurtarma noktalarını sağlam bırakın.

Kurtarma noktalarını bırakmayı seçerseniz, şu ayrıntıları aklınızda bulundurun:

* Tüm kurtarma noktaları sonsuza kadar bozulmadan kalacak, tüm budama veri korumak ile durdurma koruma duracaktır.
* Korunan örnek ve tüketilen depolama için ücretlendirilirsiniz. Daha fazla bilgi için Azure [Yedekleme fiyatlandırması'na](https://azure.microsoft.com/pricing/details/backup/)bakın.
* Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur.

Veritabanı nın korumasını durdurmak için:

* Kasa panosunda **Yedek Öğeler'i**seçin.
* **Yedekleme Yönetim Türü**altında Azure **VM'de SAP HANA'yı** seçin

  ![Azure VM'de SAP HANA'yı seçin](./media/sap-hana-db-manage/sap-hana-azure-vm.png)

* Korumayı durdurmak istediğiniz veritabanını seçin:

  ![Korumayı durdurmak için veritabanını seçin](./media/sap-hana-db-manage/select-database.png)

* Veritabanı menüsünde **yedeklemeyi durdur'u**seçin.

  ![Yedeklemeyi durdur'u seçin](./media/sap-hana-db-manage/stop-backup.png)

* **Yedeklemeyi Durdur** menüsünde, verileri saklayıp saklamayacağınızveya silmeyi seçin. İstersen, bir neden ve yorum sağlayın.

  ![Verileri tutma veya silme seçeneğini belirleyin](./media/sap-hana-db-manage/retain-backup-data.png)

* **Yedeklemeyi Durdur'u**seçin.

### <a name="resume-protection-for-an-sap-hana-database"></a>SAP HANA veritabanı için özgeçmiş koruması

SAP HANA veritabanının korumasını durdurduğunuzda, **Yedekleme Verilerini Koru** seçeneğini seçerseniz, daha sonra korumaya devam edebilirsiniz. Yedeklenen verileri saklamazsanız, korumaya devam edemezsin.

SAP HANA veritabanı için korumayı sürdürmek için:

* Yedekleme öğesini açın ve **Yedeklemeye Devam'ı**seçin.

   ![Devam yedeklemeyi seçin](./media/sap-hana-db-manage/resume-backup.png)

* Yedekleme **ilkesi** menüsünde bir ilke seçin ve sonra **Kaydet'i**seçin.

### <a name="upgrading-from-sap-hana-10-to-20"></a>SAP HANA 1.0'dan 2.0'a yükseltme

[SAP HANA 1.0'dan 2.0'a yükselttikten sonra](backup-azure-sap-hana-database-troubleshoot.md#upgrading-from-sap-hana-10-to-20)SAP HANA veritabanı için yedeklemeye nasıl devam edeceğiz öğrenin.

### <a name="upgrading-without-a-sid-change"></a>SID değişikliği olmadan yükseltme

[SID'si yükseltmeden sonra değişmemiş](backup-azure-sap-hana-database-troubleshoot.md#upgrading-without-an-sid-change)bir SAP HANA veritabanının yedeklemesine nasıl devam edeceğiz öğrenin.

### <a name="unregister-an-sap-hana-instance"></a>SAP HANA örneğinin kaydını silme

Korumayı devre dışı düşürdükten sonra ancak kasayı silmeden önce bir SAP HANA örneğini kayıt dışı edin:

* Kasa panosunda, **Yönet**altında Yedek **Altyapı'yı**seçin.

   ![Yedekleme Altyapıseçin](./media/sap-hana-db-manage/backup-infrastructure.png)

* **Azure VM'de İş Yükü** olarak Yedekleme Yönetimi **türünü** seçin

   ![Azure VM'de İş Yükü olarak Yedekleme Yönetimi türünü seçin](./media/sap-hana-db-manage/backup-management-type.png)

* **Korumalı Sunucularda,** kaydını kısımak için örneği seçin. Kasayı silmek için tüm sunucuların/örneklerin kaydını silmeniz gerekir.

* Korumalı örneği sağ tıklatın ve **Kayıt Dışı'yı**seçin.

   ![Kayıt dışı yı seçin](./media/sap-hana-db-manage/unregister.png)

### <a name="re-register-extension-on-the-sap-hana-server-vm"></a>SAP HANA sunucusu VM'de uzantıyı yeniden kaydedin

Bazen VM'deki iş yükü uzantısı şu veya bu nedenle etkilenebilir. Bu gibi durumlarda, VM'de tetiklenen tüm işlemler başarısız olmaya başlar. Daha sonra VM'deki uzantıyı yeniden kaydetmeniz gerekebilir. Yeniden kayıt işlemi, işlemlerin devam etmesi için VM'deki iş yükü yedekleme uzantısını yeniden yükler.

Bu seçeneği dikkatli kullanın: zaten sağlıklı bir uzantıya sahip bir VM'de tetiklendiğinde, bu işlem uzantınyeniden başlatılmasına neden olur. Bu, devam eden tüm işlerin başarısız lığa neden olabilir. Yeniden kaydetme işlemini tetiklemeden önce [belirtilerden](backup-azure-sap-hana-database-troubleshoot.md#re-registration-failures) birini veya birkaçını kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot) nasıl gidereceklerini öğrenin.
