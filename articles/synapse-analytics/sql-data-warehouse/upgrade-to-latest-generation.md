---
title: En son nesle yükseltin
description: Azure Synapse Analytics SQL havuzunun en son nesil Azure donanım ve depolama mimarisine yükseltin.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/19/2019
ms.author: martinle
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 637e377e469eeb1a82b6c0ad3a845d94ac09c7db
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351208"
---
# <a name="optimize-performance-by-upgrading-azure-synapse-analytics-sql-pool"></a>Azure Synapse Analytics SQL havuzuna yükselterek performansı optimize edin

SQL havuzunu en son nesil Azure donanım ve depolama mimarisine yükseltin.

## <a name="why-upgrade"></a>Neden yükseltme?

Artık [desteklenen bölgeler](gen2-migration-schedule.md#automated-schedule-and-region-availability-table)için Azure portalındaki SQL havuzu Compute Optimize Gen2 katmanına sorunsuz bir şekilde yükseltebilirsiniz. Bölgeniz kendi kendini yükseltmeyi desteklemiyorsa, desteklenen bir bölgeye yükseltebilir veya kendi kendine yükseltmenin bölgenizde kullanılabilir olmasını bekleyebilirsiniz. Daha hızlı performans, daha yüksek ölçeklenebilirlik ve sınırsız sütun depolama gibi en son nesil Azure donanımı ve gelişmiş depolama mimarisinden yararlanmak için şimdi yükseltin. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Uygulandığı öğe:

Bu yükseltme, [desteklenen bölgelerdeki](gen2-migration-schedule.md#automated-schedule-and-region-availability-table)En İyi Duruma Getirilmiş Gen1 tier SQL havuzları için geçerlidir.

## <a name="before-you-begin"></a>Başlamadan önce

1. [BÖLGEnizin](gen2-migration-schedule.md#automated-schedule-and-region-availability-table) GEN1-GEN2 geçişi için desteklenilip desteklenmeyip desteklenmeyolup olmadığını kontrol edin. Otomatik geçiş tarihlerine dikkat edin. Otomatik işlemle çakışmayı önlemek için, el ile geçişinizi otomatik işlem başlangıç tarihinden önce planlayın.
2. Henüz desteklenmeyen bir bölgedeyseniz, desteklenen bölgeye [geri yükleme kullanarak](#upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal) bölgenizin eklenmesini veya yükseltilmeyeceğini denetlemeye devam edin.
3. Bölgeniz [desteklenirse, Azure portalı üzerinden yükseltme](#upgrade-in-a-supported-region-using-the-azure-portal)
4. Aşağıdaki eşlemi kullanarak, İşlem Optimize Edilmiş Gen1 katmanındaki geçerli performans düzeyini temel alarak SQL havuzu için **önerilen performans düzeyini seçin:**

   | İşlem Optimize Edilmiş Gen1 katmanı | İşlem Optimize Gen2 katmanı |
   | :-------------------------: | :-------------------------: |
   |            DW100            |           DW100c            |
   |            DW200            |           DW200c            |
   |            DW300            |           DW300c            |
   |            DW400            |           DW400c            |
   |            DW500            |           DW500c'yi seçin            |
   |            DW600            |           DW500c'yi seçin            |
   |           DW1000            |           DW1000c           |
   |           DW1200            |           DW1000c           |
   |           DW1500            |           DW1500c           |
   |           DW2000            |           DW2000c           |
   |           DW3000            |           DW3000c           |
   |           DW6000            |           DW6000c           |

> [!Note]
> Önerilen performans düzeyleri doğrudan dönüştürme değildir. Örneğin, DW600'den DW500c'ye gitmenizi öneririz.

## <a name="upgrade-in-a-supported-region-using-the-azure-portal"></a>Azure portalını kullanarak desteklenen bir bölgede yükseltme

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!NOTE]
> Azure portalı üzerinden Gen1'den Gen2'ye geçiş kalıcıdır. Gen1'e dönmek için bir süreç yoktur.  

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

1. Yükseltilecek İşlem Optimize Edilen Gen1 tier SQL havuzu duraklatılırsa, [SQL havuzunu devam ettirin.](pause-and-resume-compute-portal.md)

   > [!NOTE]
   > SQL havuzu Gen2'ye geçiş yapmak için çalışıyor olmalıdır.

2. Birkaç dakikalık kapalı kalma süresine hazırlıklı olun. 

3. İşlem Optimize Edilmiş Gen1 performans düzeylerine yapılan kod başvurularını belirleyin ve bunları eşdeğer İşlem Optimize Edilmiş Gen2 performans düzeyine değiştirin. Yükseltmeden önce kod başvurularını güncelleştirmeniz gereken iki örnek aşağıda verilmiştir:

   Orijinal Gen1 PowerShell komutu:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
   ```

   Şu şekilde değiştirilmiştir:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300c"
   ```

   > [!NOTE] 
   > -RequestedServiceObjectiveName "DW300" olarak değiştirilir - RequestedServiceObjectiveName "DW300**c**"
   >

   Orijinal Gen1 T-SQL komutu:

   ```SQL
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300') ;
   ```

   Şu şekilde değiştirilmiştir:

   ```sql
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300c') ; 
   ```
   > [!NOTE] 
   > SERVICE_OBJECTIVE = 'DW300' SERVICE_OBJECTIVE = 'DW300**c**' olarak değiştirilir

## <a name="start-the-upgrade"></a>Yükseltmeyi başlatın

1. Azure portalındaki İşlem Optimize Gen1 SQL havuzuna gidin. Yükseltilecek İşlem Optimize Edilen Gen1 tier SQL havuzu duraklatılırsa, [SQL havuzunu devam ettirin.](pause-and-resume-compute-portal.md) 
2. Görevler sekmesi altında **Gen2 kartına Yükseltme'yi** seçin: ![Upgrade_1](./media/upgrade-to-latest-generation/upgrade-to-gen2-1.png)
    
    > [!NOTE]
    > Görevler sekmesi altında **Gen2 kartına Yükseltme'yi** görmüyorsanız, abonelik türünüz geçerli bölgede sınırlıdır.
    > Aboneliğinizin beyaz listeye alınması için [bir destek bileti gönderin.](sql-data-warehouse-get-started-create-support-ticket.md)

3. Yükseltmeden önce iş yükünüzün çalışmasını ve titremeyi tamamladığından emin olun. SQL havuzunuz Compute Optimize Gen2 tier SQL pool olarak tekrar çevrimiçi olmadan önce birkaç dakika kapalı kalma süresi yaşayacaksınız. **Yükseltme'yi seçin**:

   ![Upgrade_2](./media/upgrade-to-latest-generation/upgrade-to-gen2-2.png)

4. Azure portalındaki durumu kontrol ederek **yükseltmenizi izleyin:**

   ![Yükseltme3](./media/upgrade-to-latest-generation/upgrade-to-gen2-3.png)

   Yükseltme işleminin ilk adımı, tüm oturumların öldürüleceği ve bağlantıların bırakılabildiği ölçek işleminden ("Yükseltme - Çevrimdışı") geçer. 

   Yükseltme işleminin ikinci adımı veri geçişidir ("Yükseltme - Çevrimiçi"). Veri geçişi çevrimiçi bir arka plan işlemidir. Bu işlem, sütun verilerini yerel bir SSD önbelleği kullanarak eski depolama mimarisinden yeni depolama mimarisine yavaşça taşır. Bu süre zarfında, SQL havuzunuz sorgulama ve yükleme için çevrimiçi olacaktır. Verileriniz, geçirilip geçirilmediğine bakılmaksızın sorgulanabilir. Veri geçişi, veri boyutunuza, performans düzeyinize ve sütun mağazanızın segmentlerinin sayısına bağlı olarak değişen oranlarda gerçekleşir. 

5. **İsteğe Bağlı Öneri:** Ölçekleme işlemi tamamlandıktan sonra, veri geçişi arka plan işlemini hızlandırabilirsiniz. Daha büyük bir SLO ve kaynak sınıfında sorgulayacağınız tüm birincil sütun deposu tablolarında [Alter Index yeniden oluşturmayı](sql-data-warehouse-tables-index.md) çalıştırarak veri hareketini zorlayabilirsiniz. Bu işlem, tablolarınızın sayısına ve boyutlarına bağlı olarak tamamlanması saatler süren damla arka plan işlemiyle karşılaştırıldığında **çevrimdışıdır.** Ancak, tamamlandığında, yüksek kaliteli satır gruplarına sahip yeni gelişmiş depolama mimarisi sayesinde veri geçişi çok daha hızlı olacaktır.
 
> [!NOTE]
> Alter Index yeniden oluşturma çevrimdışı bir işlemdir ve yeniden oluşturma tamamlanana kadar tablolar kullanılamaz.

Aşağıdaki sorgu, veri geçişini hızlandırmak için gerekli Alter Index Rebuild komutlarını oluşturur:

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```

## <a name="upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal"></a>Azure portalı üzerinden geri yükleme yi kullanarak Azure coğrafi bölgesinden yükseltme

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>Azure portalını kullanarak kullanıcı tanımlı bir geri yükleme noktası oluşturma

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Geri yükleme noktası oluşturmak istediğiniz SQL havuzuna gidin.

3. Genel Bakış bölümünün üst kısmında **+Yeni Geri Yükleme Noktası'nı**seçin.

    ![Yeni Geri Yükleme Noktası](./media/upgrade-to-latest-generation/creating_restore_point_0.png)

4. Geri yükleme noktanız için bir ad belirtin.

    ![Geri Yükleme Noktasının Adı](./media/upgrade-to-latest-generation/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>Azure portalını kullanarak etkin veya duraklatılmış bir veritabanını geri yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Geri yüklemek istediğiniz SQL havuzuna gidin.
3. Genel Bakış bölümünün üst kısmında Geri **Yükle'yi**seçin.

    ![ Geri Yüklemeye Genel Bakış](./media/upgrade-to-latest-generation/restoring_0.png)

4. Otomatik **geri yükleme noktalarını** veya **kullanıcı tanımlı geri yükleme noktalarını**seçin. Kullanıcı tanımlı geri yükleme noktaları **için, kullanıcı tanımlı bir geri yükleme noktası seçin** veya kullanıcı **tanımlı yeni bir geri yükleme noktası oluşturun.** Sunucu için **yeni oluştur'u** seçin ve Gen2 destekli coğrafi bölgede bir sunucu seçin. 

    ![Otomatik Geri Yükleme Noktaları](./media/upgrade-to-latest-generation/restoring_1.png)

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>PowerShell'i kullanarak Azure coğrafi bölgesinden geri yükleme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bir veritabanını kurtarmak için [Geri Yükleme-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) cmdlet'ini kullanın.

> [!NOTE]
> Gen2'ye coğrafi geri yükleme yapabilirsiniz! Bunu yapmak için isteğe bağlı bir parametre olarak Gen2 ServiceObjectiveName (örneğin DW1000**c)** belirtin.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listele.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Kurtarmak istediğiniz veritabanını alın.
5. Gen2 ServiceObjectiveName belirterek veritabanı için kurtarma isteğini oluşturun.
6. Coğrafi olarak geri yüklenen veritabanının durumunu doğrulayın.

```Powershell
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>" -RequestedServiceObjectiveName "DW300c"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veritabanınızı yapılandırmak için [bkz.](../../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery)

Kurtarılan veritabanı, kaynak veritabanı TDE etkinse TDE etkin olur.


SQL havuzunuzla ilgili herhangi bir sorun la karşılaşırsanız, olası neden olarak bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) oluşturun ve "Gen2 yükseltmesi" referansı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Yükseltilmiş SQL havuzunuz çevrimiçi. Gelişmiş mimariden yararlanmak için [İş Yükü Yönetimi için Kaynak sınıflarına](resource-classes-for-workload-management.md)bakın.
