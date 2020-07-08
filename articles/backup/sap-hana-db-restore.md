---
title: Azure VM 'lerinde SAP HANA veritabanlarını geri yükleme
description: Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarının nasıl geri yükleneceğini öğrenin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: a3db88ca3c995c3c190da051dbf9df6ae5e29530
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851453"
---
# <a name="restore-sap-hana-databases-on-azure-vms"></a>Azure VM 'lerinde SAP HANA veritabanlarını geri yükleme

Bu makalede, Azure Backup hizmetinin bir kurtarma hizmetleri kasasına yedeklemiş olduğu bir Azure sanal makinesinde (VM) çalışan SAP HANA veritabanlarının nasıl geri yükleneceği açıklanmaktadır. Geri yüklemeler, geliştirme/test senaryoları için veya önceki bir duruma geri dönmek üzere verilerin kopyalarını oluşturmak için kullanılabilir.

SAP HANA veritabanlarının nasıl yedekleneceği hakkında daha fazla bilgi için bkz. [Azure VM 'lerinde SAP HANA veritabanlarını yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="restore-to-a-point-in-time-or-to-a-recovery-point"></a>Bir zaman noktasına veya bir kurtarma noktasına geri yükleme

Azure Backup, Azure VM 'lerinde çalışan SAP HANA veritabanlarını şu şekilde geri yükleyebilir:

* Günlük yedeklemeleri kullanarak belirli bir tarih veya saate (ikinci olarak) geri yükleyin. Azure Backup, uygun tam, değişiklik yedeklemelerini ve seçilen saate göre geri yüklemek için gereken günlük yedeklemeleri zincirini otomatik olarak belirler.

* Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya değişiklik yedeklemesine geri yükleme yapın.

## <a name="prerequisites"></a>Ön koşullar

Bir veritabanını geri yüklemeden önce aşağıdakilere göz önüne alın:

* Veritabanını yalnızca aynı bölgedeki bir SAP HANA örneğine geri yükleyebilirsiniz

* Hedef örnek, kaynakla aynı kasada kayıtlı olmalıdır

* Azure Backup aynı VM 'de iki farklı SAP HANA örneği tanımlayamıyor. Bu nedenle, verileri aynı VM 'de bir örnekten diğerine geri yüklemek mümkün değildir.

* Hedef SAP HANA örneğinin geri yükleme için hazır olduğundan emin olmak için, **yedekleme hazırlığı** durumunu denetleyin:

  1. Hedef SAP HANA örneğinin kaydedildiği kasayı açın

  1. Kasa panosunda, **Başlarken**bölümünde **Yedekle** ' yi seçin.

      ![Kasa panosunda yedekleme](media/sap-hana-db-restore/getting-started-backup.png)

  1. **Yedekleme**'de, **ne yedeklemek** ıstıyorsunuz altında? **Azure VM 'de SAP HANA** seçin

      ![Azure VM 'de SAP HANA seçin](media/sap-hana-db-restore/sap-hana-backup.png)

  1. **VM 'lerde veritabanlarını bul**altında, **Ayrıntıları görüntüle** ' ye tıklayın.

      ![Ayrıntıları görüntüle](media/sap-hana-db-restore/view-details.png)

  1. Hedef VM 'nin **yedek hazırlığını** gözden geçirin

      ![Korumalı sunucular](media/sap-hana-db-restore/protected-servers.png)

* SAP HANA desteklediği geri yükleme türleri hakkında daha fazla bilgi edinmek için, SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) notuna bakın.

## <a name="restore-a-database"></a>Veritabanını geri yükleme

Geri yüklemek için aşağıdaki izinlere sahip olmanız gerekir:

* Geri yüklemeyi yaptığınız kasada **yedekleme işletmeni** izinleri.
* Yedeklenen kaynak VM 'ye **katkıda bulunan (yazma)** erişimi.
* Hedef VM 'ye **katkıda bulunan (yazma**) erişimi:
  * Aynı VM 'ye geri yüklüyorsanız, kaynak VM 'dir.
  * Alternatif bir konuma geri yüklüyorsanız, bu yeni hedef VM 'dir.

1. Geri yüklenecek SAP HANA veritabanının kaydedildiği kasayı açın

1. Kasa panosunda, **korumalı öğeler**altında, **yedekleme öğeleri** ' ni seçin.

    ![Yedekleme öğeleri](media/sap-hana-db-restore/backup-items.png)

1. **Yedekleme öğeleri**' nde, **yedekleme yönetimi türü** altında **Azure VM 'de SAP HANA '** yi seçin.

    ![Yedekleme yönetimi türü](media/sap-hana-db-restore/backup-management-type.png)

1. Geri yüklenecek veritabanını seçin

    ![Geri yüklenecek veritabanı](media/sap-hana-db-restore/database-to-restore.png)

1. Veritabanı menüsünü gözden geçirin. Aşağıdakiler dahil olmak üzere veritabanı yedeklemesi hakkında bilgi sağlar:

    * En eski ve en son geri yükleme noktaları

    * Veritabanı için son 24 ve 72 saat için günlük yedekleme durumu

    ![Veritabanı menüsü](media/sap-hana-db-restore/database-menu.png)

1. **Veritabanını geri yükle** ' yi seçin

1. **Yapılandırma geri yükleme**altında, verilerin nereye (veya nasıl) geri yükleneceğini belirtin:

    * **Alternatif konum**: veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını koruyun.

    * **DB üzerine yaz**: verileri özgün kaynakla aynı SAP HANA örneğine geri yükleyin. Bu seçenek özgün veritabanının üzerine yazar.

      ![Yapılandırma geri yükleme](media/sap-hana-db-restore/restore-configuration.png)

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükle

1. **Geri yükleme yapılandırma** menüsünde, **geri yükleme yeri**' nin altında, **alternatif konum**' u seçin.

    ![Alternatif konuma geri yükle](media/sap-hana-db-restore/restore-alternate-location.png)

1. Veritabanını geri yüklemek istediğiniz SAP HANA ana bilgisayar adını ve örnek adını seçin.
1. Hedef SAP HANA örneğinin geri yükleme için hazır olup olmadığını kontrol edin **.** Daha fazla ayrıntı için [Önkoşullar bölümüne](#prerequisites) bakın.
1. **Geri yüklenen veritabanı adı** kutusuna hedef veritabanının adını girin.

    > [!NOTE]
    > Tek Veritabanı Container (SDC) geri yükleme işlemleri bu [denetimleri](backup-azure-sap-hana-database-troubleshoot.md#single-container-database-sdc-restore)izlemelidir.

1. Geçerliyse, **SEÇILI Hana örneğinde aynı ada sahip BIR veritabanı zaten varsa üzerine yaz**' ı seçin.
1. **Tamam**’ı seçin.

    ![Yapılandırma geri yükleme-son ekran](media/sap-hana-db-restore/restore-configuration-last.png)

1. [Belirli bir noktaya geri yüklemek](#restore-to-a-specific-point-in-time)için **geri yükleme noktası seç**bölümünde Günlükler ' i **(zaman içindeki nokta)** seçin. Veya [belirli bir kurtarma noktasına geri yüklemek](#restore-to-a-specific-recovery-point)için **tam & türev** öğesini seçin.

### <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

1. **Geri yükleme yapılandırma** menüsünde, **geri yükleme yeri**' nin altında, **DB Tamam üzerine yaz**' ı seçin  >  **OK**.

    ![Veritabanının Üzerine Yazma](media/sap-hana-db-restore/overwrite-db.png)

1. [Belirli bir noktaya geri yüklemek](#restore-to-a-specific-point-in-time)için **geri yükleme noktası seç**bölümünde Günlükler ' i **(zaman içindeki nokta)** seçin. Veya [belirli bir kurtarma noktasına geri yüklemek](#restore-to-a-specific-recovery-point)için **tam & türev** öğesini seçin.

### <a name="restore-as-files"></a>Dosya olarak geri yükleme

Yedekleme verilerini bir veritabanı yerine dosya olarak geri yüklemek için **dosya olarak geri yükle**' yi seçin. Dosyalar belirtilen bir yola alındıktan sonra, bu dosyaları bir veritabanı olarak geri yüklemek istediğiniz herhangi bir SAP HANA makineye alabilirsiniz. Bu dosyaları herhangi bir makineye taşıyabildiğiniz için artık verileri abonelikler ve bölgeler arasında geri yükleyebilirsiniz.

1. Geri yükleme **yapılandırma** menüsünde, **konum ve nasıl geri yükleme**altında **dosya olarak geri yükle**' yi seçin.
1. Yedekleme dosyalarını geri yüklemek istediğiniz **konak** /Hana sunucu adını seçin.
1. **Sunucudaki hedef yolda**, adım 2 ' de seçilen sunucudaki klasör yolunu girin. Bu, hizmetin tüm gerekli yedekleme dosyalarının dökümünü alacak konumudur.

    Dökülebilir dosyalar şunlardır:

    * Veritabanı yedekleme dosyaları
    * Katalog dosyaları
    * JSON meta veri dosyaları (dahil edilen her yedekleme dosyası için)

    Genellikle, bir ağ paylaşımının yolu veya hedef yolu olarak belirtildiğinde bağlı bir Azure dosya paylaşımının yolu, bu dosyalara aynı ağdaki diğer makineler veya bunlara bağlı aynı Azure dosya paylaşımıyla daha kolay erişim sağlar.

    >[!NOTE]
    >Veritabanı yedekleme dosyalarını hedef kayıtlı VM 'ye bağlanmış bir Azure dosya paylaşımında geri yüklemek için, kök hesabın Azure dosya paylaşımında okuma/yazma izinlerine sahip olduğundan emin olun.

    ![Hedef yolu seçin](media/sap-hana-db-restore/restore-as-files.png)

1. Tüm yedekleme dosyalarının ve klasörlerinin geri yükleneceği **geri yükleme noktasını** seçin.

    ![Geri yükleme noktası seç](media/sap-hana-db-restore/select-restore-point.png)

1. Seçilen geri yükleme noktasıyla ilişkili tüm yedekleme dosyaları, hedef yoluna dökülür.
1. Seçilen geri yükleme noktası türüne göre (**zaman içinde** veya **tam & fark**), hedef yolunda oluşturulmuş bir veya daha fazla klasör görürsünüz. Adlı klasörlerden biri `Data_<date and time of restore>` , tam ve fark yedeklemelerini ve adlı diğer klasörü, `Log` günlük yedeklemelerini içerir.
1. Bu geri yüklenen dosyaları bir veritabanı olarak geri yüklemek istediğiniz SAP HANA sunucuya taşıyın.
1. Ardından aşağıdaki adımları izleyin:
    1. Yedekleme dosyalarının depolandığı klasör/dizin üzerinde aşağıdaki komutu kullanarak izinleri ayarlayın:

        ```bash
        chown -R <SID>adm:sapsys <directory>
        ```

    1. Sonraki komut kümesini şu şekilde Çalıştır`<SID>adm`

        ```bash
        su - <sid>adm
        ```

    1. Geri yükleme için katalog dosyasını oluşturun. Geri yükleme işleminde daha sonra kullanılacak olan tam yedekleme için, JSON meta veri dosyasından **BackupID** 'yi ayıklayın. Tam ve günlük yedeklemelerin farklı klasörlerde bulunduğundan emin olun ve bu klasörlerdeki katalog dosyalarını ve JSON meta veri dosyalarını silin.

        ```bash
        hdbbackupdiag --generate --dataDir <DataFileDir> --logDirs <LogFilesDir> -d <PathToPlaceCatalogFile>
        ```

        Yukarıdaki komutta:

        * `<DataFileDir>`-tam yedeklemeleri içeren klasör
        * `<LogFilesDir>`-günlük yedeklemelerini içeren klasör
        * `<PathToPlaceCatalogFile>`-Katalog dosyasının oluşturulduğu klasör yerleştirilmesi gerekir

    1. Yeni oluşturulan katalog dosyasını HANA Studio aracılığıyla geri yükleyin veya bu yeni oluşturulan katalogla HDBSQL restore sorgusunu çalıştırın. HDBSQL sorguları aşağıda listelenmiştir:

    * Zaman içinde bir noktaya geri yüklemek için:

        Geri yüklenen yeni bir veritabanı oluşturuyorsanız, HDBSQL komutunu çalıştırarak yeni bir veritabanı oluşturun `<DatabaseName>` ve ardından veritabanını geri yükleme için durdurun. Ancak, yalnızca var olan bir veritabanını geri yüklüyorsanız, veritabanını durdurmak için HDBSQL komutunu çalıştırın.

        Ardından, veritabanını geri yüklemek için aşağıdaki komutu çalıştırın:

        ```hdbsql
        RECOVER DATABASE FOR <DatabaseName> UNTIL TIMESTAMP '<TimeStamp>' CLEAR LOG USING SOURCE '<DatabaseName@HostName>'  USING CATALOG PATH ('<PathToGeneratedCatalogInStep3>') USING LOG PATH (' <LogFileDir>') USING DATA PATH ('<DataFileDir>') USING BACKUP_ID <BackupIdFromJsonFile> CHECK ACCESS USING FILE
        ```

        * `<DatabaseName>`-Geri yüklemek istediğiniz yeni veritabanının veya var olan veritabanının adı
        * `<Timestamp>`-Zaman içindeki noktaya geri yükleme için tam zaman damgası
        * `<DatabaseName@HostName>`-Backup 'ın geri yükleme için kullanıldığı veritabanının adı ve bu veritabanının bulunduğu **konak** /SAP HANA sunucu adı. `USING SOURCE <DatabaseName@HostName>`Seçeneği, veri yedeklemesinin (geri yükleme için kullanılan), hedef SAP HANA makineden farklı bır SID veya ada sahip bir veritabanı olduğunu belirtir. Bu nedenle, yedeklemenin alındığı aynı HANA sunucusunda yapılan geri yüklemeler için belirtilmesi gerekmez.
        * `<PathToGeneratedCatalogInStep3>`- **C adımında** oluşturulan katalog dosyasının yolu
        * `<DataFileDir>`-tam yedeklemeleri içeren klasör
        * `<LogFilesDir>`-günlük yedeklemelerini içeren klasör
        * `<BackupIdFromJsonFile>`- **C adımında** ayıklanan **BackupID**

    * Belirli bir tam veya değişiklik yedeklemesine geri yüklemek için:

        Geri yüklenen yeni bir veritabanı oluşturuyorsanız, HDBSQL komutunu çalıştırarak yeni bir veritabanı oluşturun `<DatabaseName>` ve ardından veritabanını geri yükleme için durdurun. Ancak, yalnızca var olan bir veritabanını geri yüklüyorsanız, veritabanını durdurmak için HDBSQL komutunu çalıştırın:

        ```hdbsql
        RECOVER DATA FOR <DatabaseName> USING BACKUP_ID <BackupIdFromJsonFile> USING SOURCE '<DatabaseName@HostName>'  USING CATALOG PATH ('<PathToGeneratedCatalogInStep3>') USING DATA PATH ('<DataFileDir>')  CLEAR LOG
        ```

        * `<DatabaseName>`-geri yüklemek istediğiniz yeni veritabanının veya var olan veritabanının adı
        * `<Timestamp>`-zaman içinde nokta geri yükleme 'nin tam zaman damgası
        * `<DatabaseName@HostName>`-geri yükleme için yedekleme kullanılan veritabanının adı ve bu veritabanının bulunduğu **konak** /SAP HANA sunucu adı. `USING SOURCE <DatabaseName@HostName>`Seçeneği, veri yedeklemesinin (geri yükleme için kullanılan), hedef SAP HANA makineden farklı bır SID veya ada sahip bir veritabanı olduğunu belirtir. Bu nedenle, yedeklemenin alındığı aynı HANA sunucusunda yapılan geri yüklemeler için belirtilmemesi gerekir.
        * `<PathToGeneratedCatalogInStep3>`- **C adımında** oluşturulan katalog dosyasının yolu
        * `<DataFileDir>`-tam yedeklemeleri içeren klasör
        * `<LogFilesDir>`-günlük yedeklemelerini içeren klasör
        * `<BackupIdFromJsonFile>`- **C adımında** ayıklanan **BackupID**

### <a name="restore-to-a-specific-point-in-time"></a>Belirli bir zaman noktasına geri yükleme

Geri yükleme türü olarak **Günlükler (zaman içinde)** seçtiyseniz şunları yapın:

1. Günlük grafiğinden bir kurtarma noktası seçin ve geri yükleme noktasını seçmek için **Tamam** ' ı seçin.

    ![Geri yükleme noktası](media/sap-hana-db-restore/restore-point.png)

1. **Geri yükleme menüsünde geri** **Yükle ' yi seçerek restore** işini başlatın.

    ![Geri yükle 'yi seçin](media/sap-hana-db-restore/restore-restore.png)

1. Geri yükleme ilerlemesini **Bildirimler** alanında izleyin veya veritabanı menüsünde **işleri geri yükle** seçeneğini belirleyerek izleyin.

    ![Geri yükleme başarıyla tetiklendi](media/sap-hana-db-restore/restore-triggered.png)

### <a name="restore-to-a-specific-recovery-point"></a>Belirli bir kurtarma noktasına geri yükleme

Geri yükleme türü olarak **tam & türev** seçtiyseniz şunları yapın:

1. Listeden bir kurtarma noktası seçin ve geri yükleme noktasını seçmek için **Tamam** ' ı seçin.

    ![Belirli kurtarma noktasını geri yükle](media/sap-hana-db-restore/specific-recovery-point.png)

1. **Geri yükleme menüsünde geri** **Yükle ' yi seçerek restore** işini başlatın.

    ![Geri yükleme işini Başlat](media/sap-hana-db-restore/restore-specific.png)

1. Geri yükleme ilerlemesini **Bildirimler** alanında izleyin veya veritabanı menüsünde **işleri geri yükle** seçeneğini belirleyerek izleyin.

    ![Geri yükleme ilerleme durumu](media/sap-hana-db-restore/restore-progress.png)

    > [!NOTE]
    > Birden çok veritabanı kapsayıcısı (MDC), sistem DB bir hedef örneğe geri yüklendikten sonra, birinin ön kayıt betiğini yeniden çalıştırması gerekir. Yalnızca sonraki kiracı DB geri yüklemeleri başarılı olur. Daha fazla bilgi edinmek için [– MDC geri yükleme](backup-azure-sap-hana-database-troubleshoot.md#multiple-container-database-mdc-restore)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Backup kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi [öğrenin](sap-hana-db-manage.md)
