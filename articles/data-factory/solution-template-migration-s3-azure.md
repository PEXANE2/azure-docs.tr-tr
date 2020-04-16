---
title: Verileri Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye geçirin
description: Azure Veri Fabrikası ile AWS S3'te bir bölüm listesini depolamak için harici bir denetim tablosu kullanarak Amazon S3'ten veri geçirmek için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/07/2019
ms.openlocfilehash: 23d799f84cb3ac3ca911a5669041b0a25394a7ff
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414770"
---
# <a name="migrate-data-from-amazon-s3-to-azure-data-lake-storage-gen2"></a>Verileri Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye geçirin

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye yüz milyonlarca dosyadan oluşan petabaytlarca veriyi geçirmek için şablonları kullanın. 

 > [!NOTE]
 > AWS S3'ten Azure'a küçük veri hacmini kopyalamak istiyorsanız (örneğin, 10 TB'den az), [Azure Veri Fabrikası Kopyalama Veri aracını](copy-data-tool.md)kullanmak daha verimli ve kolaydır. Bu makalede açıklanan şablon, gereksinim duyduğunuzşablondan daha fazladır.

## <a name="about-the-solution-templates"></a>Çözüm şablonları hakkında

Veri bölme, özellikle 10 TB'den fazla veri aktarırken önerilir. Verileri bölmek için, Amazon S3'teki klasör leri ve dosyaları ada göre filtrelemek için 'önek' ayarını uygulayın ve ardından her ADF kopyalama işi aynı anda bir bölümü kopyalayabilir. Daha iyi iş elde etmek için aynı anda birden çok ADF kopyalama işi çalıştırabilirsiniz.

Veri geçişi normalde bir kerelik geçmiş veri geçişi nin yanı sıra AWS S3'ten Azure'a değişiklikleri düzenli olarak eşitleme gerektirir. Aşağıda, bir şablonun bir kerelik geçmiş veri geçişini, diğer şablonun ise AWS S3'ten Azure'a değişiklikleri eşitlemeyi kapsadığı iki şablon vardır.

### <a name="for-the-template-to-migrate-historical-data-from-amazon-s3-to-azure-data-lake-storage-gen2"></a>Şablonun geçmiş verileri Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye aktarması için

Bu şablon *(şablon adı: Geçmiş verileri AWS S3'ten Azure Veri Gölü Depolama Gen2'sine geçir)* Azure SQL Veritabanı'ndaki bir dış denetim tablosuna bir bölüm listesi yazdığınızı varsayar. Bu nedenle, dış denetim tablosundan bölüm listesini almak, her bölüm üzerinde yinelemek ve her ADF kopya işi bir defada bir bölümü kopyalamak için bir *Arama* etkinliği kullanır. Herhangi bir kopyalama işi tamamlandıktan sonra, denetim tablosundaki her bölümü kopyalama durumunu güncelleştirmek için *Depolanan Yordam* etkinliğini kullanır.

Şablon beş etkinlik içerir:
- **Arama,** Azure Veri Gölü Depolama Gen2'ye kopyalanmamış bölümleri harici bir denetim tablosundan alır. Tablo adı *s3_partition_control_table* ve tablodan veri yüklemek için sorgu *"SELECT PartitionPrefix FROM s3_partition_control_table WHERE SuccessOrFailure = 0"*.
- **ForEach,** *Arama* etkinliğinden bölüm listesini alır ve her bölümü *TriggerCopy* etkinliğine yineler. *BatchCount'yi* aynı anda birden çok ADF kopya işi çalıştıracak şekilde ayarlayabilirsiniz. Bu şablonda 2 tane belirledik.
- **ExecutePipeline** *CopyFolderPartitionFromS3* ardışık yürütme. Her kopya işi bir bölüm kopyalamak için başka bir ardışık kaynak oluşturmamızın nedeni, bu belirli bölümü AWS S3'ten yeniden yüklemek için başarısız kopya işini yeniden çalıştırmanızı kolaylaştıracak olmasıdır. Diğer bölümleri yükleyen diğer tüm kopyalama işleri etkilenmez.
- AWS S3'ten Azure Veri Gölü Depolama Gen2'ye kadar her bölümü **kopyalayın.**
- **SqlServerStoredProcedure** denetim tablosunda her bölümü kopyalama durumunu güncelleştirin.

Şablon iki parametre içerir:
- **AWS_S3_bucketName,** verileri geçirmek istediğiniz AWS S3'teki kova adınızdır. AWS S3'teki birden çok kovadan veri geçirmek istiyorsanız, her bölüm için kova adını depolamak için dış denetim tablonuza bir sütun daha ekleyebilir ve bu sütundan verileri almak için ardışık adınızı güncelleştirebilirsiniz.
- **Azure_Storage_fileSystem,** verileri aktarmak istediğiniz Azure Veri Gölü Depolama Gen2'deki dosyaSistem adınızdır.

### <a name="for-the-template-to-copy-changed-files-only-from-amazon-s3-to-azure-data-lake-storage-gen2"></a>Şablonun yalnızca Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye kopyalanması için

Bu şablon *(şablon adı: AWS S3'ten Azure Veri Gölü Depolama Gen2'ye*kadar olan verileri kopyala ) yalnızca AWS S3'ten Azure'a kadar yeni veya güncelleştirilmiş dosyaları kopyalamak için her dosyanın LastModifiedTime'ını kullanır. Dosya veya klasörleriniz, AWS S3'teki dosya veya klasör adının bir parçası olarak zaman dilimi bilgileriyle zaten zaman dilimi bilgileriyle bölümlenmişse (örneğin, /yyyy/mm/dd/file.csv), yeni dosyaları kademeli olarak yüklemek için daha performant bir yaklaşım elde etmek için bu [öğreticiye](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md) gidebilirsiniz. Bu şablon, Azure SQL Veritabanı'ndaki bir dış denetim tablosuna bir bölüm listesi yazdığınızı varsayar. Bu nedenle, dış denetim tablosundan bölüm listesini almak, her bölüm üzerinde yinelemek ve her ADF kopya işi bir defada bir bölümü kopyalamak için bir *Arama* etkinliği kullanır. Her kopyalama işi Dosyaları AWS S3'ten kopyalamaya başladığında, yalnızca yeni veya güncelleştirilen dosyaları tanımlamak ve kopyalamak için LastModifiedTime özelliğine güvenir. Herhangi bir kopyalama işi tamamlandıktan sonra, denetim tablosundaki her bölümü kopyalama durumunu güncelleştirmek için *Depolanan Yordam* etkinliğini kullanır.

Şablon yedi etkinlik içerir:
- **Arama,** bölümleri harici bir denetim tablosundan alır. Tablo adı *s3_partition_delta_control_table* ve tablodan veri yüklemek için sorgu *"s3_partition_delta_control_table farklı BölümPrefix seçin"*.
- **ForEach,** *Arama* etkinliğinden bölüm listesini alır ve her bölümü *TriggerDeltaCopy* etkinliğine yineler. *BatchCount'yi* aynı anda birden çok ADF kopya işi çalıştıracak şekilde ayarlayabilirsiniz. Bu şablonda 2 tane belirledik.
- **ExecutePipeline** *DeltaCopyFolderPartitionFromS3* ardışık uygular. Her kopya işi bir bölüm kopyalamak için başka bir ardışık kaynak oluşturmamızın nedeni, bu belirli bölümü AWS S3'ten yeniden yüklemek için başarısız kopya işini yeniden çalıştırmanızı kolaylaştıracak olmasıdır. Diğer bölümleri yükleyen diğer tüm kopyalama işleri etkilenmez.
- **Arama,** yeni veya güncelleştirilmiş dosyaların LastModifiedTime üzerinden tanımlanabilmesi için dış denetim tablosundan son kopya iş çalıştırma süresini alır. Tablo adı *s3_partition_delta_control_table* ve tablodan veri yüklemek için sorgu *"select max (JobRunTime) s3_partition_delta_control_table'den LastModifiedTime olarak BölümPrefix = '@{pipeline().parameters.prefixStr}' ve SuccessOrFailure = 1"* dir.
- Yalnızca AWS S3'ten Azure Veri Gölü Depolama Gen2'ye kadar her bölüm için yeni veya değiştirilmiş dosyaları **kopyalayın.** *modifiedDatetimeStart* özelliği son kopya iş çalışma süresi ayarlanır. *modifiedDatetimeEnd* özelliği geçerli kopyalama iş çalışma süresi ayarlanır. Saatin UTC saat dilimine uygulandığını unutmayın.
- **SqlServerStoredProcedure,** başarılı olduğunda denetim tablosunda her bölümü kopyalama ve kopyalama çalışma süresini güncelleştirme. SuccessOrFailure sütunu 1 olarak ayarlanır.
- **SqlServerStoredProcedure,** başarısız olduğunda denetim tablosunda her bölümü kopyalama ve kopyalama çalışma süresini güncelleştirme. SuccessOrFailure sütunu 0 olarak ayarlanır.

Şablon iki parametre içerir:
- **AWS_S3_bucketName,** verileri geçirmek istediğiniz AWS S3'teki kova adınızdır. AWS S3'teki birden çok kovadan veri geçirmek istiyorsanız, her bölüm için kova adını depolamak için dış denetim tablonuza bir sütun daha ekleyebilir ve bu sütundan verileri almak için ardışık adınızı güncelleştirebilirsiniz.
- **Azure_Storage_fileSystem,** verileri aktarmak istediğiniz Azure Veri Gölü Depolama Gen2'deki dosyaSistem adınızdır.

## <a name="how-to-use-these-two-solution-templates"></a>Bu iki çözüm şablonu nasıl kullanılır?

### <a name="for-the-template-to-migrate-historical-data-from-amazon-s3-to-azure-data-lake-storage-gen2"></a>Şablonun geçmiş verileri Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye aktarması için 

1. AWS S3'ün bölüm listesini depolamak için Azure SQL Veritabanı'nda bir denetim tablosu oluşturun. 

    > [!NOTE]
    > Masa adı s3_partition_control_table.
    > Denetim tablosunun şeması PartitionPrefix and SuccessOrFailure'dır, PartitionPrefix, Amazon S3'teki klasör leri ve dosyaları ada göre filtrelemek için S3'teki önek ayarıdır ve SuccessOrFailure her bölümü kopyalama durumudur: 0 bu bölümün Azure'a kopyalanmadığı anlamına gelir ve 1 bu bölümün başarıyla Azure'a kopyalandığı anlamına gelir.
    > Denetim tablosunda tanımlanan 5 bölüm vardır ve her bölümü kopyalamanın varsayılan durumu 0'dır.

    ```sql
    CREATE TABLE [dbo].[s3_partition_control_table](
        [PartitionPrefix] [varchar](255) NULL,
        [SuccessOrFailure] [bit] NULL
    )

    INSERT INTO s3_partition_control_table (PartitionPrefix, SuccessOrFailure)
    VALUES
    ('a', 0),
    ('b', 0),
    ('c', 0),
    ('d', 0),
    ('e', 0);
    ```

2. Denetim tablosu için aynı Azure SQL Veritabanında Saklı Yordam oluşturun. 

    > [!NOTE]
    > Depolanan Yordamın adı sp_update_partition_success. ADF ardışık ardışık alanınızdaki SqlServerStoredProcedure etkinliği tarafından çağrılacaktır.

    ```sql
    CREATE PROCEDURE [dbo].[sp_update_partition_success] @PartPrefix varchar(255)
    AS
    BEGIN
    
        UPDATE s3_partition_control_table
        SET [SuccessOrFailure] = 1 WHERE [PartitionPrefix] = @PartPrefix
    END
    GO
    ```

3. **AWS S3'ten Azure Veri Gölü Depolama Gen2 şablonuna geçmiş verileri geçir'e** gidin. Dış denetim tablonuza bağlantıları, veri kaynağı deposu olarak AWS S3'ü ve hedef depo olarak Azure Veri Gölü Depolama Gen2'yi giriş yapın. Dış denetim tablosu ve depolanan yordamın aynı bağlantıya atıfta bulunduğuna dikkat edin.

    ![Yeni bağlantı oluşturma](media/solution-template-migration-s3-azure/historical-migration-s3-azure1.png)

4. **Bu şablonu kullan'ı**seçin.

    ![Bu şablonu kullan](media/solution-template-migration-s3-azure/historical-migration-s3-azure2.png)
    
5. Aşağıdaki örnekte gösterildiği gibi, 2 ardışık hatlar ve 3 veri kümesi oluşturulduğunu görürsünüz:

    ![Boru hattını gözden geçirin](media/solution-template-migration-s3-azure/historical-migration-s3-azure3.png)

6. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.

    ![**Hata Ayıklama** seçeneğini tıklatın](media/solution-template-migration-s3-azure/historical-migration-s3-azure4.png)

7. Aşağıdaki örneğe benzer sonuçlar görürsünüz:

    ![Sonucu gözden geçirin](media/solution-template-migration-s3-azure/historical-migration-s3-azure5.png)


### <a name="for-the-template-to-copy-changed-files-only-from-amazon-s3-to-azure-data-lake-storage-gen2"></a>Şablonun yalnızca Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye kopyalanması için

1. AWS S3'ün bölüm listesini depolamak için Azure SQL Veritabanı'nda bir denetim tablosu oluşturun. 

    > [!NOTE]
    > Tablo adı s3_partition_delta_control_table.
    > Denetim tablosunun şeması PartitionPrefix, JobRunTime ve SuccessOrFailure'dır, PartitionPrefix S3'teki klasörleri ve dosyaları Amazon S3'teki klasör ve dosyaları ada göre filtrelemek için önek ayardır, JobRunTime kopyalama işleri çalıştırıldığında tarih saati değeridir ve SuccessOrFailure her bölümü kopyalama durumudur: 0 bu bölüm Azure'a kopyalanmamıştır ve 1 bu bölüm Azure'a başarıyla kopyalandığı anlamına gelir.
    > Denetim tablosunda tanımlanan 5 bölüm vardır. JobRunTime için varsayılan değer, tek seferlik geçmiş veri geçişinin başladığı zaman olabilir. ADF kopyalama etkinliği, bu tarihten sonra en son değiştirilen AWS S3'teki dosyaları kopyalar. Her bölümü kopyalamanın varsayılan durumu 1'dir.

    ```sql
    CREATE TABLE [dbo].[s3_partition_delta_control_table](
        [PartitionPrefix] [varchar](255) NULL,
        [JobRunTime] [datetime] NULL,
        [SuccessOrFailure] [bit] NULL
        )

    INSERT INTO s3_partition_delta_control_table (PartitionPrefix, JobRunTime, SuccessOrFailure)
    VALUES
    ('a','1/1/2019 12:00:00 AM',1),
    ('b','1/1/2019 12:00:00 AM',1),
    ('c','1/1/2019 12:00:00 AM',1),
    ('d','1/1/2019 12:00:00 AM',1),
    ('e','1/1/2019 12:00:00 AM',1);
    ```

2. Denetim tablosu için aynı Azure SQL Veritabanında Saklı Yordam oluşturun. 

    > [!NOTE]
    > Depolanan Yordam'ın adı sp_insert_partition_JobRunTime_success. ADF ardışık ardışık alanınızdaki SqlServerStoredProcedure etkinliği tarafından çağrılacaktır.

    ```sql
        CREATE PROCEDURE [dbo].[sp_insert_partition_JobRunTime_success] @PartPrefix varchar(255), @JobRunTime datetime, @SuccessOrFailure bit
        AS
        BEGIN
            INSERT INTO s3_partition_delta_control_table (PartitionPrefix, JobRunTime, SuccessOrFailure)
            VALUES
            (@PartPrefix,@JobRunTime,@SuccessOrFailure)
        END
        GO
    ```


3. **AWS S3'ten Azure Veri Gölü Depolama Gen2 şablonuna kadar kopyala delta verisine** gidin. Dış denetim tablonuza bağlantıları, veri kaynağı deposu olarak AWS S3'ü ve hedef depo olarak Azure Veri Gölü Depolama Gen2'yi giriş yapın. Dış denetim tablosu ve depolanan yordamın aynı bağlantıya atıfta bulunduğuna dikkat edin.

    ![Yeni bağlantı oluşturma](media/solution-template-migration-s3-azure/delta-migration-s3-azure1.png)

4. **Bu şablonu kullan'ı**seçin.

    ![Bu şablonu kullan](media/solution-template-migration-s3-azure/delta-migration-s3-azure2.png)
    
5. Aşağıdaki örnekte gösterildiği gibi, 2 ardışık hatlar ve 3 veri kümesi oluşturulduğunu görürsünüz:

    ![Boru hattını gözden geçirin](media/solution-template-migration-s3-azure/delta-migration-s3-azure3.png)

6. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.

    ![**Hata Ayıklama** seçeneğini tıklatın](media/solution-template-migration-s3-azure/delta-migration-s3-azure4.png)

7. Aşağıdaki örneğe benzer sonuçlar görürsünüz:

    ![Sonucu gözden geçirin](media/solution-template-migration-s3-azure/delta-migration-s3-azure5.png)

8. Ayrıca bir sorgu *"select * from s3_partition_delta_control_table"* ile kontrol edebilirsiniz, aşağıdaki örneğe benzer çıktı göreceksiniz:

    ![Sonucu gözden geçirin](media/solution-template-migration-s3-azure/delta-migration-s3-azure6.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Birden çok kapsayıcıdan dosyaları kopyalama](solution-template-copy-files-multiple-containers.md)
- [Dosyaları taşıma](solution-template-move-files.md)