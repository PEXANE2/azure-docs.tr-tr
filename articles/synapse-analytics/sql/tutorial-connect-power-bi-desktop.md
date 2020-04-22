---
title: "Öğretici: SQL isteğe bağlı (önizleme) Power BI Desktop'a bağlayın & rapor oluşturun"
description: Bu eğitimde, Azure Synapse Analytics'teki ISTEĞe bağlı SQL'i (önizleme) Power BI masaüstüne nasıl bağlayabileceğinizi ve görünüme dayalı bir demo raporu oluşturmayı öğrenin.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: e0ac6ccde2443a7b374d9eb85f6f960af79c69dc
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769490"
---
# <a name="tutorial-connect-sql-on-demand-preview-to-power-bi-desktop--create-report"></a>Öğretici: SQL isteğe bağlı (önizleme) Power BI Desktop'a bağlayın & rapor oluşturun

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> - Demo veritabanı oluşturma
> - Rapor için kullanılan görünüm oluşturma
> - Power BI Desktop’a bağlanma
> - Görünüme dayalı rapor oluşturma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki yazılıma ihtiyacınız var:

- [Azure Veri Stüdyosu](/sql/azure-data-studio/download-azure-data-studio)veya SQL Server [Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms)gibi bir SQL sorgu aracı.
- [Güç BI Masaüstü](https://powerbi.microsoft.com/downloads/).

Aşağıdaki parametreler için değerler:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet bitiş noktası adresi    | Sunucu adı olarak kullanılır                                   |
| SQL isteğe bağlı hizmet bitiş noktası bölgesi     | Numunelerde kullanılan depolamayı belirlemek için kullanılır |
| Uç nokta erişimi için kullanıcı adı ve parola | Uç noktaya erişmek için kullanılır                               |
| Görünüm oluşturmak için kullanacağınız veritabanı     | Örneklerde başlangıç noktası olarak kullanılan veritabanı       |

## <a name="1---create-database"></a>1 - Veritabanı oluşturma

Demo ortamı için kendi demo veritabanınızı oluşturun. Bu veritabanını meta verileri görüntülemek için kullanırsınız, gerçek verileri depolamak için değil.

Aşağıdaki Transact-SQL (T-SQL) komut dosyasını çalıştırarak demo veritabanını oluşturun (ve gerekirse varolan bir veritabanını bırakın:

```sql
-- Drop database if it exists
IF EXISTS (SELECT * FROM sys.databases WHERE name = 'Demo')
BEGIN
    DROP DATABASE Demo
END;
GO

-- Create new database
CREATE DATABASE [Demo];
GO
```

## <a name="2---create-credential"></a>2 - Kimlik bilgisi oluşturma

SQL on-demand hizmetinin depolama daki dosyalara erişebilmek için bir kimlik bilgisi gereklidir. Bitiş noktanızla aynı bölgede bulunan bir depolama hesabının kimlik belgesini oluşturun. İsteğe bağlı SQL farklı bölgelerden depolama hesaplarına erişebilse de, depolama ve bitiş noktasının aynı bölgede olması daha iyi performans sağlar.

Aşağıdaki Transact-SQL (T-SQL) komut dosyasını çalıştırarak kimlik bilgisini oluşturun:

```sql
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer];
GO

-- Create credentials for Census Data container which resides in a azure open data storage account
-- There is no secret. We are using public storage account which doesn't need a secret.
CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = '';
GO
```

## <a name="3---prepare-view"></a>3 - Görünüm hazırlama

Aşağıdaki Transact-SQL (T-SQL) komut dosyasını çalıştırarak Power BI'nin tüketire çalışması için harici demo verilerini temel alan görünümü oluşturun:

Aşağıdaki sorguyla `usPopulationView` veritabanı `Demo` içindeki görünümü oluşturun:

```sql
DROP VIEW IF EXISTS usPopulationView;
GO

CREATE VIEW usPopulationView AS
SELECT
    *
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS uspv;
```

Demo verileri aşağıdaki veri kümelerini içerir:

2000 ve 2010 Decennial Census parke formatında kaynaklı her ABD ilçe için cinsiyet ve ırk ait ABD nüfusu.

| Klasör yolu                                                  | Açıklama                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /sürüm/                                                    | Demo depolama hesabındaki veriler için üst klasör               |
| /release/us_population_county/                               | Parke formatında ABD nüfus veri dosyaları, Hive / Hadoop bölümleme düzeni kullanılarak yıl bölümlenmiş. |

## <a name="4---create-power-bi-report"></a>4 - Power BI raporu oluşturun

Aşağıdaki adımları kullanarak Power BI Desktop için rapor oluşturun:

1. Power BI Desktop uygulamasını açın ve **veri al'ı**seçin.

   ![Power BI masaüstü uygulamasını açın ve veri almayı seçin.](./media/tutorial-connect-power-bi-desktop/step-0-open-powerbi.png)

2. **Azure Azure** > **SQL Veritabanı'nı**seçin. 

   ![Veri kaynağını seçin.](./media/tutorial-connect-power-bi-desktop/step-1-select-data-source.png)

3. Veritabanının **Sunucu** alanında bulunduğu sunucunun adını yazın ve ardından `Demo` veritabanı adını yazın. **İçe Aktarma** seçeneğini seçin ve ardından **Tamam'ı**seçin. 

   ![Bitiş noktasında veritabanını seçin.](./media/tutorial-connect-power-bi-desktop/step-2-db.png)

4. Tercih edilen kimlik doğrulama yöntemini seçin:

    - AAD örneği 
  
    ![Oturum Aç'ı tıklatın.](./media/tutorial-connect-power-bi-desktop/step-2.1-select-aad-auth.png)

    - SQL Giriş Için Örnek - Kullanıcı adınızı ve şifrenizi yazın.

    ![SQL girişi kullanın.](./media/tutorial-connect-power-bi-desktop/step-2.2-select-sql-auth.png)


5. Görünümü `usPopulationView`seçin ve sonra **Yükle'yi**seçin. 

   ![Seçilen veritabanında görünüm'i seçin.](./media/tutorial-connect-power-bi-desktop/step-3-select-view.png)

6. İşlemin tamamlanmasını bekleyin ve ardından bir açılır `There are pending changes in your queries that haven't been applied`pencere görüntülenir. **Değişiklikleri Uygula'yı**seçin. 

   ![Değişiklikleri uygula'yı tıklatın.](./media/tutorial-connect-power-bi-desktop/step-4-apply-changes.png)

7. Sorgu yu toynatmama iletişim kutusunun kaybolmasını bekleyin ve bu birkaç dakika sürebilir. **Apply query changes** 

   ![Bir sorgunun tamamlanmasını bekleyin.](./media/tutorial-connect-power-bi-desktop/step-5-wait-for-query-to-finish.png)

8. Yük tamamlandıktan sonra, raporu oluşturmak için aşağıdaki sütunları seçin:
   - countyName
   - Nüfus
   - stateName

   ![Harita raporu oluşturmak için ilgi alanları sütunlarını seçin.](./media/tutorial-connect-power-bi-desktop/step-6-select-columns-of-interest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu raporu kullanmayı bitirdikten sonra, kaynakları aşağıdaki adımlarla silin:

1. Depolama hesabının kimlik bilgilerini silme

   ```sql
   DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer];
   ```

2. Görünümü silme

   ```sql
   DROP VIEW usPopulationView;
   ```

3. Veritabanını bırak

   ```sql
   DROP DATABASE Demo;
   ```

## <a name="next-steps"></a>Sonraki adımlar

Synapse SQL kullanarak depolama dosyalarını nasıl sorgulayabilirsiniz öğrenmek için [Sorgu depolama dosyalarına](develop-storage-files-overview.md) ilerleyin.
