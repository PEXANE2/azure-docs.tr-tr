---
title: 'Öğretici: Power BI Desktop & rapor oluşturmak için SQL isteğe bağlı (Önizleme) bağlama'
description: Bu öğreticide, Azure SYNAPSE Analytics 'teki isteğe bağlı SQL (Önizleme) öğesini Power BI masaüstüne bağlamayı ve bir görünümü temel alan bir demo raporu oluşturmayı öğrenin.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 649c9a2e0dd9df21a9a59140d9f2999768aab555
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745412"
---
# <a name="tutorial-use-sql-on-demand-preview-with-power-bi-desktop--create-a-report"></a>Öğretici: Power BI Desktop & bir rapor oluşturmak için SQL isteğe bağlı (Önizleme) kullanın

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
>
> - Demo veritabanı oluştur
> - Rapor için kullanılan görünüm oluştur
> - İsteğe bağlı SQL 'e Power BI Desktop bağlama
> - Görünümü temel alan rapor oluştur

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki yazılıma ihtiyacınız vardır:

- [Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio)veya [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms)gibi bir SQL sorgu aracı.
- [Power BI Desktop](https://powerbi.microsoft.com/downloads/).

Aşağıdaki parametrelerin değerleri:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet uç noktası adresi    | Sunucu adı olarak kullanılır                                   |
| SQL isteğe bağlı hizmet uç noktası bölgesi     | Örneklerde kullanılan depolamayı belirlemede kullanılır |
| Uç nokta erişimi için Kullanıcı adı ve parola | Uç noktaya erişmek için kullanılır                               |
| Görünümler oluşturmak için kullanacağınız veritabanı     | Örneklerde başlangıç noktası olarak kullanılan veritabanı       |

## <a name="1---create-database"></a>1-veritabanı oluşturma

Tanıtım ortamı için kendi demo veritabanınızı oluşturun. Gerçek verileri depolamak için değil, meta verileri görüntülemek için bu veritabanını kullanırsınız.

Aşağıdaki Transact-SQL (T-SQL) betiğini çalıştırarak demo veritabanını oluşturun (ve gerekirse var olan bir veritabanını bırakın):

```sql
-- Drop database if it exists
DROP DATABASE IF EXISTS Demo
GO

-- Create new database
CREATE DATABASE [Demo];
GO
```

## <a name="2---create-data-source"></a>2-veri kaynağı oluşturma

İsteğe bağlı SQL hizmeti için depolama alanındaki dosyalara erişmek üzere bir veri kaynağı gereklidir. Uç noktanız ile aynı bölgede bulunan bir depolama hesabı için veri kaynağını oluşturun. İsteğe bağlı SQL, farklı bölgelerdeki depolama hesaplarına erişebilse de depolama ve uç noktanın aynı bölgede bulunması daha iyi performans sağlar.

Aşağıdaki Transact-SQL (T-SQL) betiğini çalıştırarak veri kaynağını oluşturun:

```sql
-- There is no credential in data surce. We are using public storage account which doesn't need a secret.
CREATE EXTERNAL DATA SOURCE AzureOpenData
WITH ( LOCATION = 'https://azureopendatastorage.blob.core.windows.net/')
```

## <a name="3---prepare-view"></a>3-görünümü hazırlama

Aşağıdaki Transact-SQL (T-SQL) betiğini çalıştırarak Power BI için dış tanıtım verilerine göre görünüm oluşturun:

`usPopulationView`Aşağıdaki sorguyla veritabanı içinde görünüm oluşturun `Demo` :

```sql
DROP VIEW IF EXISTS usPopulationView;
GO

CREATE VIEW usPopulationView AS
SELECT
    *
FROM
    OPENROWSET(
        BULK 'censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        DATA_SOURCE = 'AzureOpenData',
        FORMAT='PARQUET'
    ) AS uspv;
```

Demo verileri aşağıdaki veri kümelerini içerir:

2000 ve 2010 ' den kaynaklanan her bir ABD İlçesi için cinsiyet ve yarış tarafından popülasyon, Parquet biçiminde ABD 'de Cennial

| Klasör yolu                                                  | Açıklama                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| /Release                                                    | Tanıtım depolama hesabındaki veriler için üst klasör               |
| /Release/us_population_county/                               | Veri dosyalarını, Hive/Hadoop bölümlendirme şeması kullanılarak yıla göre bölümlenen Parquet biçiminde doldurma. |

## <a name="4---create-power-bi-report"></a>4-Power BI raporu oluşturma

Aşağıdaki adımları kullanarak Power BI Desktop raporu oluşturun:

1. Power BI Desktop uygulamasını açın ve **veri al**' ı seçin.

   ![Power BI Masaüstü uygulamasını açın ve veri al ' ı seçin.](./media/tutorial-connect-power-bi-desktop/step-0-open-powerbi.png)

2. **Azure**  >  **Azure SQL veritabanı**' nı seçin. 

   ![Veri kaynağını seçin.](./media/tutorial-connect-power-bi-desktop/step-1-select-data-source.png)

3. Veritabanının **sunucu** alanında bulunduğu sunucunun adını yazın ve ardından `Demo` veritabanı adını yazın. **Içeri aktarma** seçeneğini belirleyip **Tamam**' ı seçin. 

   ![Uç noktada veritabanı ' nı seçin.](./media/tutorial-connect-power-bi-desktop/step-2-db.png)

4. Tercih edilen kimlik doğrulama yöntemini seçin:

    - AAD için örnek 
  
        ![Oturum aç ' a tıklayın.](./media/tutorial-connect-power-bi-desktop/step-2.1-select-aad-auth.png)

    - SQL oturum açma örneği-Kullanıcı adınızı ve parolanızı yazın.

        ![SQL oturum açma kullanın.](./media/tutorial-connect-power-bi-desktop/step-2.2-select-sql-auth.png)


5. Görünümü seçin `usPopulationView` ve ardından **Yükle**' yi seçin. 

   ![Seçili veritabanında bir görünüm seçin.](./media/tutorial-connect-power-bi-desktop/step-3-select-view.png)

6. İşlemin tamamlanmasını bekleyin ve ardından bir açılır pencere görüntülenir `There are pending changes in your queries that haven't been applied` . **Değişiklikleri Uygula**' yı seçin. 

   ![Değişiklikleri Uygula ' ya tıklayın.](./media/tutorial-connect-power-bi-desktop/step-4-apply-changes.png)

7. **Sorgu değişikliklerini Uygula** iletişim kutusunun kaybolması için bekleyin, bu işlem birkaç dakika sürebilir. 

   ![Sorgunun bitmesini bekleyin.](./media/tutorial-connect-power-bi-desktop/step-5-wait-for-query-to-finish.png)

8. Yükleme tamamlandıktan sonra, raporu oluşturmak için bu sırayla aşağıdaki sütunları seçin:
   - countyName
   - usu
   - stateName

   ![Bir harita raporu oluşturmak için ilgilendiğiniz sütunları seçin.](./media/tutorial-connect-power-bi-desktop/step-6-select-columns-of-interest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu raporu kullanarak işiniz bittiğinde, aşağıdaki adımlarla kaynakları silin:

1. Depolama hesabı için kimlik bilgilerini sil

   ```sql
   DROP EXTENAL DATA SOURCE AzureOpenData
   ```

2. Görünümü Sil

   ```sql
   DROP VIEW usPopulationView;
   ```

3. Veritabanını bırak

   ```sql
   DROP DATABASE Demo;
   ```

## <a name="next-steps"></a>Sonraki adımlar

SYNAPSE SQL kullanarak depolama dosyalarının nasıl sorgulanalınacağını öğrenmek için [sorgu depolama dosyalarına](develop-storage-files-overview.md) ilerleyin.
