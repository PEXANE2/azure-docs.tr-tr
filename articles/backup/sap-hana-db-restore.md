---
title: Azure VM 'lerinde SAP HANA veritabanlarını geri yükleme
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarının nasıl geri yükleneceğini öğrenin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 999edba61177758ad9039e81e789efcef99ca1de
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287924"
---
# <a name="restore-sap-hana-databases-on-azure-vms"></a>Azure VM 'lerinde SAP HANA veritabanlarını geri yükleme

Bu makalede, Azure Backup hizmetinin bir Azure Backup kurtarma hizmetleri kasasına yedeklemiş olduğunu Azure sanal makinesinde (VM) çalışan SAP HANA veritabanlarının nasıl geri yükleneceği açıklanmaktadır. Geri yüklemeler, geliştirme/test senaryoları için veya önceki bir duruma geri dönmek üzere verilerin kopyalarını oluşturmak için kullanılabilir.

SAP HANA veritabanlarının nasıl yedekleneceği hakkında daha fazla bilgi için bkz. [Azure VM 'lerinde SAP HANA veritabanlarını yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="restore-to-a-point-in-time-or-to-a-recovery-point"></a>Bir zaman noktasına veya bir kurtarma noktasına geri yükleme

Azure Backup, Azure VM 'lerinde çalışan SAP HANA veritabanlarını şu şekilde geri yükleyebilir:

* Günlük yedeklemeleri kullanarak belirli bir tarih veya saate (ikinci olarak) geri yükleyin. Azure Backup, uygun tam, değişiklik yedeklemelerini ve seçilen saate göre geri yüklemek için gereken günlük yedeklemeleri zincirini otomatik olarak belirler.

* Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya değişiklik yedeklemesine geri yükleme yapın.

## <a name="prerequisites"></a>Önkoşullar

Bir veritabanını geri yüklemeden önce aşağıdakilere göz önüne alın:

* Veritabanını yalnızca aynı bölgedeki bir SAP HANA örneğine geri yükleyebilirsiniz

* Hedef örnek, kaynakla aynı kasada kayıtlı olmalıdır

* Azure Backup aynı VM 'de iki farklı SAP HANA örneği tanımlayamıyor. Bu nedenle, verileri aynı VM 'de bir örnekten diğerine geri yüklemek mümkün değildir

* Hedef SAP HANA örneğinin geri yükleme için hazır olduğundan emin olmak için, **yedekleme hazırlığı** durumunu denetleyin:

  * Hedef SAP HANA örneğinin kaydedildiği kasayı açın

  * Kasa panosunda, **Başlarken**bölümünde **Yedekle** ' yi seçin.

![Kasa panosunda yedekleme](media/sap-hana-db-restore/getting-started-backup.png)

* **Yedekleme**'de, **ne yedeklemek** ıstıyorsunuz altında? **Azure VM 'de SAP HANA** seçin

![Azure VM 'de SAP HANA seçin](media/sap-hana-db-restore/sap-hana-backup.png)

* **VM 'lerde veritabanlarını bul** altında, **Ayrıntıları görüntüle** ' ye tıklayın.

![Ayrıntıları görüntüle](media/sap-hana-db-restore/view-details.png)

* Hedef VM 'nin **yedek hazırlığını** gözden geçirin

![Korumalı sunucular](media/sap-hana-db-restore/protected-servers.png)

* SAP HANA desteklediği geri yükleme türleri hakkında daha fazla bilgi edinmek için, SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) notuna bakın.

## <a name="restore-a-database"></a>Bir veritabanını geri yükleme

* Geri yüklenecek SAP HANA veritabanının kaydedildiği kasayı açın

* Kasa panosunda, **korumalı öğeler**altında, **yedekleme öğeleri** ' ni seçin.

![Yedekleme öğeleri](media/sap-hana-db-restore/backup-items.png)

* **Yedekleme öğeleri**' nde, **yedekleme yönetimi türü** altında **Azure VM 'de SAP HANA '** yi seçin.

![Yedekleme yönetimi türü](media/sap-hana-db-restore/backup-management-type.png)

* Geri yüklenecek veritabanını seçin

 ![Geri yüklenecek veritabanı](media/sap-hana-db-restore/database-to-restore.png)

* Veritabanı menüsünü gözden geçirin. Aşağıdakiler dahil olmak üzere veritabanı yedeklemesi hakkında bilgi sağlar:

  * En eski ve en son geri yükleme noktaları

  * Veritabanı için son 24 ve 72 saat için günlük yedekleme durumu

![Veritabanı menüsü](media/sap-hana-db-restore/database-menu.png)

* **Veritabanını geri yükle** ' yi seçin

* **Yapılandırma geri yükleme**altında, verilerin nereye (veya nasıl) geri yükleneceğini belirtin:

  * **Alternatif konum**: veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını koruyun.

  * **DB üzerine yaz**: verileri özgün kaynakla aynı SAP HANA örneğine geri yükleyin. Bu seçenek özgün veritabanının üzerine yazar.

![Yapılandırma geri yükleme](media/sap-hana-db-restore/restore-configuration.png)

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükle

* **Geri yükleme yapılandırma** menüsünde, **geri yükleme yeri**' nin altında, **alternatif konum**' u seçin.

![Alternatif konuma geri yükle](media/sap-hana-db-restore/restore-alternate-location.png)

* Veritabanını geri yüklemek istediğiniz SAP HANA ana bilgisayar adını ve örnek adını seçin.
* Hedef SAP HANA örneğinin geri yükleme için hazır olup olmadığını kontrol edin **.** Daha fazla ayrıntı için [Önkoşullar bölümüne](#prerequisites) bakın.
* **Geri yüklenen veritabanı adı** kutusuna hedef veritabanının adını girin.

> [!NOTE]
> Tek Veritabanı Container (SDC) geri yükleme işlemleri bu [denetimleri](backup-azure-sap-hana-database-troubleshoot.md#single-container-database-sdc-restore)izlemelidir.

* Geçerliyse, **SEÇILI Hana örneğinde aynı ada sahip BIR veritabanı zaten varsa üzerine yaz**' ı seçin.
* **Tamam**’ı seçin.

![Yapılandırma geri yükleme-son ekran](media/sap-hana-db-restore/restore-configuration-last.png)

* [Belirli bir noktaya geri yüklemek](#restore-to-a-specific-point-in-time)için **geri yükleme noktası seç**bölümünde Günlükler ' i **(zaman içindeki nokta)** seçin. Veya [belirli bir kurtarma noktasına geri yüklemek](#restore-to-a-specific-recovery-point)için **tam & Türev** öğesini seçin.

### <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

* **Geri yükleme yapılandırma** menüsünde, **geri yükleme yeri**' nin altında, **DB > üzerine yaz** **' ı seçin**.

![Veritabanının Üzerine Yazma](media/sap-hana-db-restore/overwrite-db.png)

* [Belirli bir noktaya geri yüklemek](#restore-to-a-specific-point-in-time)için **geri yükleme noktası seç**bölümünde Günlükler ' i **(zaman içindeki nokta)** seçin. Veya [belirli bir kurtarma noktasına geri yüklemek](#restore-to-a-specific-recovery-point)için **tam & Türev** öğesini seçin.

### <a name="restore-to-a-specific-point-in-time"></a>Belirli bir zaman noktasına geri yükleme

Geri yükleme türü olarak **Günlükler (zaman içinde)** seçtiyseniz şunları yapın:

* Günlük grafiğinden bir kurtarma noktası seçin ve geri yükleme noktasını seçmek için **Tamam** ' ı seçin.

![Geri yükleme noktası](media/sap-hana-db-restore/restore-point.png)

* **Geri yükleme menüsünde geri** **Yükle ' yi seçerek restore** işini başlatın.

![Geri yükle 'yi seçin](media/sap-hana-db-restore/restore-restore.png)

* Geri yükleme ilerlemesini **Bildirimler** alanında izleyin veya veritabanı menüsünde **işleri geri yükle** seçeneğini belirleyerek izleyin.

![Geri yükleme başarıyla tetiklendi](media/sap-hana-db-restore/restore-triggered.png)

### <a name="restore-to-a-specific-recovery-point"></a>Belirli bir kurtarma noktasına geri yükleme

Geri yükleme türü olarak **tam & türev** seçtiyseniz şunları yapın:

* Listeden bir kurtarma noktası seçin ve geri yükleme noktasını seçmek için **Tamam** ' ı seçin.

![Belirli kurtarma noktasını geri yükle](media/sap-hana-db-restore/specific-recovery-point.png)

* **Geri yükleme menüsünde geri** **Yükle ' yi seçerek restore** işini başlatın.

![Geri yükleme işini Başlat](media/sap-hana-db-restore/restore-specific.png)

* Geri yükleme ilerlemesini **Bildirimler** alanında izleyin veya veritabanı menüsünde **işleri geri yükle** seçeneğini belirleyerek izleyin.

![Geri yükleme ilerleme durumu](media/sap-hana-db-restore/restore-progress.png)

> [!NOTE]
> Birden çok veritabanı kapsayıcısı (MDC), sistem DB bir hedef örneğe geri yüklendikten sonra, birinin ön kayıt betiğini yeniden çalıştırması gerekir. Yalnızca sonraki kiracı DB geri yüklemeleri başarılı olur. Daha fazla bilgi edinmek için [– MDC geri yükleme](backup-azure-sap-hana-database-troubleshoot.md#multiple-container-database-mdc-restore)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Backup kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi [öğrenin](sap-hana-db-manage.md)
