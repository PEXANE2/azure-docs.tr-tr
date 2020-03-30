---
title: Azure VM'lerinde SAP HANA veritabanlarını geri yükleme
description: Bu makalede, Azure Sanal Makinelerde çalışan SAP HANA veritabanlarını nasıl geri yükleyin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 999edba61177758ad9039e81e789efcef99ca1de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74287924"
---
# <a name="restore-sap-hana-databases-on-azure-vms"></a>Azure VM'lerinde SAP HANA veritabanlarını geri yükleme

Bu makalede, Azure Yedekleme hizmetinin Azure Yedekleme Kurtarma Hizmetleri kasasına yedeklediği Azure Sanal Makine 'de (VM) çalışan SAP HANA veritabanlarının nasıl geri yüklenilen açıklanmaktadır. Geri yüklemeler, geliştirme / test senaryoları için verilerin kopyalarını oluşturmak veya önceki duruma dönmek için kullanılabilir.

SAP HANA veritabanlarını yedekleme hakkında daha fazla bilgi için [Azure VM'lerde SAP HANA veritabanlarını yedekleyin.](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)

## <a name="restore-to-a-point-in-time-or-to-a-recovery-point"></a>Zaman içinde bir noktaya veya kurtarma noktasına geri yükleme

Azure Yedekleme, Azure VM'lerinde çalışan SAP HANA veritabanlarını aşağıdaki gibi geri yükleyebilir:

* Günlük yedeklemelerini kullanarak belirli bir tarihe veya saate (ikinciye) geri yükleyin. Azure Yedekleme, seçilen süreye bağlı olarak geri yüklemesi gereken uygun tam, diferansiyel yedeklemeleri ve günlük yedeklemeleri zincirini otomatik olarak belirler.

* Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya diferansiyel yedekleme geri yükleyin.

## <a name="prerequisites"></a>Ön koşullar

Veritabanını geri vermeden önce aşağıdakileri unutmayın:

* Veritabanını yalnızca aynı bölgede bulunan bir SAP HANA örneğine geri yükleyebilirsiniz

* Hedef örnek, kaynakla aynı kasaya kaydedilmelidir

* Azure Yedekleme aynı VM'de iki farklı SAP HANA örneğini tanımlayamaz. Bu nedenle, aynı VM'de bir örnekten diğerine veri geri vermek mümkün değildir

* Hedef SAP HANA örneğinin geri yüklemeye hazır olduğundan emin olmak için Yedekleme **hazır durumunu** denetleyin:

  * Hedef SAP HANA örneğinin kayıtlı olduğu kasayı açın

  * Tonoz panosunda, **Başlarken**altında, **Yedekleme'yi** seçin

![Tonoz panosunda yedekleme](media/sap-hana-db-restore/getting-started-backup.png)

* **Yedekleme'de**, **yedeklemek istediğiniz şey altında?** **Azure VM'de SAP HANA'yı** seçin

![Azure VM'de SAP HANA'yı seçin](media/sap-hana-db-restore/sap-hana-backup.png)

* **VM'lerde DB'leri Keşfedin'in** altında **Görünüm ayrıntılarını** tıklayın

![Ayrıntıları görüntüle](media/sap-hana-db-restore/view-details.png)

* Hedef VM'nin **Yedekleme hazırlığını** gözden geçirin

![Korumalı sunucular](media/sap-hana-db-restore/protected-servers.png)

* SAP HANA'nın desteklediği geri yükleme türleri hakkında daha fazla bilgi edinmek için SAP HANA Note [1642148'e](https://launchpad.support.sap.com/#/notes/1642148) bakın

## <a name="restore-a-database"></a>Veritabanını geri yükleme

* Geri yüklenecek SAP HANA veritabanının kayıtlı olduğu kasayı açın

* Korumalı **Öğeler'in**altındaki kasa panosunda **Yedek Öğeler'i** seçin

![Yedek öğeler](media/sap-hana-db-restore/backup-items.png)

* **Yedekleme**Öğeleri'nde, **Yedekleme Yönetimi Türü** altında Azure **VM'de SAP HANA'yı** seçin

![Yedekleme yönetim türü](media/sap-hana-db-restore/backup-management-type.png)

* Geri yüklenecek veritabanını seçin

 ![Geri yüklenir](media/sap-hana-db-restore/database-to-restore.png)

* Veritabanı menüsünü gözden geçirin. Veritabanı yedeklemesi hakkında bilgi sağlar:

  * En eski ve en son geri yükleme noktaları

  * Veritabanı için son 24 ve 72 saat için günlük yedekleme durumu

![Veritabanı menüsü](media/sap-hana-db-restore/database-menu.png)

* **DB'yi Geri Yükle'yi** seçin

* **Yapılandırmayı Geri Yükle'nin**altında, verileri nerede (veya nasıl) geri yükleyebileceğinizi belirtin:

  * **Alternatif Konum**: Veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını tutun.

  * **Overwrite DB**: Verileri orijinal kaynakla aynı SAP HANA örneğine geri yükleyin. Bu seçenek özgün veritabanının üzerine yazar.

![Yapılandırmayı geri yükleme](media/sap-hana-db-restore/restore-configuration.png)

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükleme

* **Yapılandırmayı Geri Yükle** menüsünde, **Geri Yüklenilen Yer'in** **altında, Alternatif Konum'u**seçin.

![Alternatif konuma geri yükleme](media/sap-hana-db-restore/restore-alternate-location.png)

* Veritabanını geri yüklemek istediğiniz SAP HANA ana bilgisayar adını ve örnek adını seçin.
* Hedef SAP HANA örneğinin Yedekleme Hazırlığı'nı sağlayarak geri yüklemeye hazır olup olmadığını kontrol **edin.** Daha fazla bilgi için [ön koşullar bölümüne](#prerequisites) bakın.
* Geri **yüklenen DB Adı** kutusuna hedef veritabanının adını girin.

> [!NOTE]
> Tek Veritabanı Kapsayıcı (SDC) geri yüklemeleri bu [denetimleri](backup-azure-sap-hana-database-troubleshoot.md#single-container-database-sdc-restore)izlemeli.

* Varsa, **seçili HANA örneğinde aynı ada sahip DB zaten varsa Overwrite'ı**seçin.
* **Tamam'ı**seçin.

![Yapılandırmayı geri yükleme - son ekran](media/sap-hana-db-restore/restore-configuration-last.png)

* **Geri Yükleme noktasını seç'te,** belirli bir noktaya zaman içinde geri yüklemek için **Günlükleri (Point in Time)** seçeneğini [belirleyin.](#restore-to-a-specific-point-in-time) Veya belirli bir kurtarma [noktasına geri yüklemek için](#restore-to-a-specific-recovery-point)Tam & **Diferansiyel'i** seçin.

### <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

* **Yapılandırmayı Geri Yükle** menüsünde, **Geri Yüklenilen Yer'in**altında, DB > **Ok'u** **Üzerine Yaz'ı**seçin.

![Veritabanının Üzerine Yazma](media/sap-hana-db-restore/overwrite-db.png)

* **Geri Yükleme noktasını seç'te,** belirli bir noktaya zaman içinde geri yüklemek için **Günlükleri (Point in Time)** seçeneğini [belirleyin.](#restore-to-a-specific-point-in-time) Veya belirli bir kurtarma [noktasına geri yüklemek için](#restore-to-a-specific-recovery-point)Tam & **Diferansiyel'i** seçin.

### <a name="restore-to-a-specific-point-in-time"></a>Zaman içinde belirli bir noktaya geri yükleme

Geri yükleme türü olarak **Günlükleri (Point in Time)** seçtiyseniz, aşağıdakileri yapın:

* Günlük grafiğinden bir kurtarma noktası seçin ve geri yükleme noktasını seçmek için **Tamam'ı** seçin.

![Geri yükleme noktası](media/sap-hana-db-restore/restore-point.png)

* Geri **Yükle** menüsünde, geri yükleme işini başlatmak için **Geri Yükle'yi** seçin.

![Geri yükleme yi seçin](media/sap-hana-db-restore/restore-restore.png)

* **Bildirimler** alanındaki geri yükleme ilerlemesini izleyin veya veritabanı menüsünde **işleri geri yükle'yi** seçerek izleyin.

![Başarıyla tetiklenen geri yükleme](media/sap-hana-db-restore/restore-triggered.png)

### <a name="restore-to-a-specific-recovery-point"></a>Belirli bir kurtarma noktasına geri yükleme

Geri yükleme türü olarak **Tam & Diferansiyel'i** seçtiyseniz, aşağıdakileri yapın:

* Listeden bir kurtarma noktası seçin ve geri yükleme noktasını seçmek için **Tamam'ı** seçin.

![Belirli kurtarma noktasını geri yükleme](media/sap-hana-db-restore/specific-recovery-point.png)

* Geri **Yükle** menüsünde, geri yükleme işini başlatmak için **Geri Yükle'yi** seçin.

![İşi geri yüklemeye başlayın](media/sap-hana-db-restore/restore-specific.png)

* **Bildirimler** alanındaki geri yükleme ilerlemesini izleyin veya veritabanı menüsünde **işleri geri yükle'yi** seçerek izleyin.

![İlerlemeyi geri yükleme](media/sap-hana-db-restore/restore-progress.png)

> [!NOTE]
> Birden Çok Veritabanı Kapsayıcısı'nda (MDC) sistem DB hedef örneğe geri yüklendikten sonra, ön kayıt komut dosyasını yeniden çalıştırmak gerekir. Ancak o zaman sonraki kiracı DB geri yüklemeleri başarılı olacaktır. Daha fazla bilgi edinmek için [Sorun Giderme – MDC Geri Yükleme'ye](backup-azure-sap-hana-database-troubleshoot.md#multiple-container-database-mdc-restore)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Yedekleme'yi kullanarak yedeklenen SAP HANA veritabanlarını nasıl yöneteceğimize [öğrenin](sap-hana-db-manage.md)
