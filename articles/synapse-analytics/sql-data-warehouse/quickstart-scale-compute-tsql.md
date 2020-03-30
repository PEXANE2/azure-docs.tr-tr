---
title: Azure Synapse Analytics'te hesaplamayı ölçeklendirin - T-SQL
description: T-SQL ve SQL Server Management Studio (SSMS) kullanarak Azure Synapse Analytics'te hesaplamayı ölçeklendirin. Daha iyi performans için işlemin ölçeğini genişletin veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: ba0bcc61cbfbb16652021045a3b25bbcee72df2c
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350776"
---
# <a name="quickstart-scale-compute-in-azure-synapse-analytics-using-t-sql"></a>Quickstart: T-SQL kullanarak Azure Synapse Analytics'te hesaplamayı ölçeklendirin

T-SQL ve SQL Server Management Studio (SSMS) kullanarak Azure Synapse Analytics'te (eski adıyla SQL DW) hesaplamayı ölçeklendirin. Daha iyi performans için [işlemin ölçeğini genişletin](sql-data-warehouse-manage-compute-overview.md) veya maliyet tasarrufu sağlamak için işlemin ölçeğini geri daraltın. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.
 
## <a name="create-a-data-warehouse"></a>Veri ambarı oluşturma

[Hızlı Başlangıç: Oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanarak **mySampleDataWarehouse** adlı bir veri ambarı oluşturun. Bir güvenlik duvarı kuralına sahip olduğundan ve sql server management studio içinden veri ambarınıza bağlanabildiğinizden emin olmak için hızlı başlatmayı tamamlayın.

## <a name="connect-to-the-server-as-server-admin"></a>Sunucu yöneticisi olarak sunucuya bağlanma

Bu bölümde Azure SQL sunucunuzla bağlantı kurmak için [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) kullanılmaktadır.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Sunucu türü | Veritabanı altyapısı | Bu değer gereklidir |
   | Sunucu adı | Tam sunucu adı | İşte bir örnek: **mySampleDataWarehouseservername.database.windows.net**. |
   | Kimlik doğrulaması | SQL Server Kimlik Doğrulaması | Bu öğreticide yapılandırılan tek kimlik doğrulaması türü SQL Kimlik Doğrulamasıdır. |
   | Oturum Aç | Sunucu yöneticisi hesabı | Sunucuyu oluştururken belirttiğiniz hesap. |
   | Parola | Sunucu yöneticisi hesabınızın parolası | Sunucuyu oluşturduğunuzda belirttiğiniz parola. |

    ![Sunucuya bağlan](./media/quickstart-scale-compute-tsql/connect-to-server.png)

3. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır.

4. Nesne Gezgini’nde, **Veritabanları**’nı genişletin. Ardından **mySampleDataWarehouse'u** yeni veritabanınızdaki nesneleri görüntülemek için genişletin.

    ![Veritabanı nesneleri](./media/quickstart-scale-compute-tsql/connected.png)

## <a name="view-service-objective"></a>Hizmet hedefini görüntüleme
Hizmet hedefi ayarı, veri ambarı için veri ambarı birimleri sayısını içerir. 

Veri ambarınız için geçerli veri ambarı birimlerini görüntülemek için:

1. **mySampleDataWarehouseservername.database.windows.net**bağlantı altında, **Sistem Veritabanları**genişletmek.
2. **Ana** seçeneğine sağ tıklayıp **Yeni Sorgu**’yu seçin. Yeni bir sorgu penceresi açılır.
3. sys.database_service_objectives dinamik yönetim görünümünden seçim yapmak için aşağıdaki sorguyu çalıştırın. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. Aşağıdaki sonuçlar, **mySampleDataWarehouse** öğesinin DW400 hizmet hedefine sahip olduğunu gösterir. 

    ![iew-akım-dwu](./media/quickstart-scale-compute-tsql/view-current-dwu.png)

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme
Azure Synapse'de, veri ambarı birimlerini ayarlayarak bilgi işlem kaynaklarını artırabilir veya azaltabilirsiniz. [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünde **mySampleDataWarehouse** oluşturuldu ve 400 DWU ile başlatıldı. Aşağıdaki adımlar, **mySampleDataWarehouse** için DWU’ları ayarlar.

Veri ambarı birimlerini değiştirmek için:

1. **Ana** seçeneğine sağ tıklayıp **Yeni Sorgu**’yu seçin.
2. [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL deyimini kullanarak hizmet hedefini değiştirin. Hizmet hedefini için DW300 olarak değiştirmek için aşağıdaki sorguyu çalıştırın. 

    ```Sql
    ALTER DATABASE mySampleDataWarehouse
    MODIFY (SERVICE_OBJECTIVE = 'DW300c');
    ```

## <a name="monitor-scale-change-request"></a>Ölçek değişikliği isteğini izleme
Önceki değişiklik isteğinin ilerlemesini görmek için sys.dm_operation_status dinamik yönetim görünümünü (DMV) yoklamak üzere `WAITFORDELAY` T-SQL söz dizimini kullanabilirsiniz.

Hizmet nesnesi değişiklik durumunu yoklamak için:

1. **Ana** seçeneğine sağ tıklayıp **Yeni Sorgu**’yu seçin.
2. sys.dm_operation_status DMV’sini yoklamak için aşağıdaki sorguyu çalıştırın.

    ```sql
    WHILE 
    (
        SELECT TOP 1 state_desc
        FROM sys.dm_operation_status
        WHERE 
            1=1
            AND resource_type_desc = 'Database'
            AND major_resource_id = 'mySampleDataWarehouse'
            AND operation = 'ALTER DATABASE'
        ORDER BY
            start_time DESC
    ) = 'IN_PROGRESS'
    BEGIN
        RAISERROR('Scale operation in progress',0,0) WITH NOWAIT;
        WAITFOR DELAY '00:00:05';
    END
    PRINT 'Complete';
    ```
3. Elde edilen çıkış, durumu yoklama işleminin günlük kaydını gösterir.

    ![İşlem durumu](./media/quickstart-scale-compute-tsql/polling-output.png)

## <a name="check-data-warehouse-state"></a>Veri ambarı durumunu denetleme

Bir veri ambarı duraklatıldığında için T-SQL ile buna bağlanamazsınız. Veri ambarının geçerli durumunu görmek için bir PowerShell cmdlet’ini kullanabilirsiniz. Bir örnek için bkz. [Veri ambarı durumunu denetleme - Powershell](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>İşlem durumunu denetleme

Azure Sinaps'ınızdaki çeşitli yönetim işlemleri hakkında bilgi vermek için [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) DMV'de aşağıdaki sorguyu çalıştırın. Örneğin, işlemi ve işlemin IN_PROGRESS veya COMPLETED olan durumunu döndürür.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'mySampleDataWarehouse'
```


## <a name="next-steps"></a>Sonraki adımlar
Artık veri ambarınız için işlemin nasıl ölçeklendirileceğini öğrendiniz. Azure Synapse hakkında daha fazla bilgi edinmek için veri yükleme eğitimine devam edin.

> [!div class="nextstepaction"]
>[Verileri Azure Synapse Analytics'e yükleme](load-data-from-azure-blob-storage-using-polybase.md)
