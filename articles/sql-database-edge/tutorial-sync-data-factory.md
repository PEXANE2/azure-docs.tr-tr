---
title: Azure Veri Fabrikası'nı kullanarak Azure SQL Veritabanı Kenarı'ndan verileri eşitleyin | Microsoft Dokümanlar
description: Azure SQL Veritabanı Kenarı ve Azure Blob depolama alanı arasında veri eşitleme hakkında bilgi edinin
keywords: sql veritabanı kenarı,sql veritabanı kenarından senkronizasyon verileri, sql veritabanı kenar veri fabrikası
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: e6fd9e6431137708ba93328a8ed1359b93b4ee1f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74851715"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-blob-storage-by-using-azure-data-factory"></a>Öğretici: Azure Veri Fabrikası'nı kullanarak verileri SQL Database Edge'den Azure Blob depolamasına senkronize etme

Bu öğreticide, Azure SQL Veritabanı Kenarı örneğindeki bir tablodaki verileri Azure Blob depolamasına artımlı olarak eşitlemek için Azure Veri Fabrikası'nı kullanırsınız.

## <a name="before-you-begin"></a>Başlamadan önce

Azure SQL Veritabanı Kenarı dağıtımınızda henüz bir veritabanı veya tablo oluşturmadıysanız, oluşturmak için şu yöntemlerden birini kullanın:

* SQL Database Edge'e bağlanmak için [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/) veya Azure Data [Studio'u](/sql/azure-data-studio/download/) kullanın. Veritabanı ve tablo oluşturmak için bir SQL komut dosyası çalıştırın.
* DOĞRUDAN SQL Veritabanı Edge modülüne bağlanarak [SQLCMD](/sql/tools/sqlcmd-utility/) kullanarak bir SQL veritabanı ve tablo oluşturun. Daha fazla bilgi için [sqlcmd kullanarak Veritabanı Motoruna Bağlan'a](/sql/ssms/scripting/sqlcmd-connect-to-the-database-engine/)bakın.
* DAC paket dosyasını SQL Database Edge kapsayıcısına dağıtmak için SQLPackage.exe'yi kullanın. Bu işlemi, modülün istenen özellikler yapılandırmasının bir parçası olarak SQLPackage dosyası URI'yi belirterek otomatikleştirebilirsiniz. Ayrıca, SQL Database Edge'e bir DAC paketi dağıtmak için doğrudan SqlPackage.exe istemci aracını kullanabilirsiniz.

    SqlPackage.exe'yi nasıl indirdiğiniz hakkında bilgi için [sqlpackage'ı indirin ve yükleyin.](/sql/tools/sqlpackage-download/) Aşağıda SqlPackage.exe için bazı örnek komutlar veremiştir. Daha fazla bilgi için SqlPackage.exe belgelerine bakın.

    **DAC paketi oluşturma**

    ```cmd
    sqlpackage /Action:Extract /SourceConnectionString:"Data Source=<Server_Name>,<port>;Initial Catalog=<DB_name>;User ID=<user>;Password=<password>" /TargetFile:<dacpac_file_name>
    ```

    **DAC paketi uygulama**

    ```cmd
    sqlpackage /Action:Publish /Sourcefile:<dacpac_file_name> /TargetServerName:<Server_Name>,<port> /TargetDatabaseName:<DB_Name> /TargetUser:<user> /TargetPassword:<password>
    ```

## <a name="create-a-sql-table-and-procedure-to-store-and-update-the-watermark-levels"></a>Filigran düzeylerini depolamak ve güncelleştirmek için bir SQL tablosu ve yordamı oluşturma

Filigran tablosu, verilerin Azure Depolama ile zaten eşitlenmiş olduğu son zaman damgasını depolamak için kullanılır. Her eşitlemeden sonra filigran tablosunu güncelleştirmek için Transact-SQL (T-SQL) depolanan yordam kullanılır.

Bu komutları SQL Database Edge örneğinde çalıştırın:

```sql
    Create table [dbo].[watermarktable]
    (
    TableName varchar(255),
    WatermarkValue datetime,
    )
    GO

    CREATE PROCEDURE usp_write_watermark @timestamp datetime, @TableName varchar(50)  
    AS  
    BEGIN
    UPDATE [dbo].[watermarktable]
    SET [WatermarkValue] = @timestamp WHERE [TableName] = @TableName
    END
    Go
```

## <a name="create-a-data-factory-pipeline"></a>Veri Fabrikası ardışık hattı oluşturma

Bu bölümde, verileri Azure SQL Veritabanı Kenarı'ndaki bir tablodan Azure Blob depolamasına senkronize etmek için bir Azure Veri Fabrikası ardışık hattı oluşturursunuz.

### <a name="create-a-data-factory-by-using-the-data-factory-ui"></a>Veri Fabrikası UI'sini kullanarak bir veri fabrikası oluşturma

[Bu öğreticideki](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory)yönergeleri izleyerek bir veri fabrikası oluşturun.

### <a name="create-a-data-factory-pipeline"></a>Veri Fabrikası ardışık hattı oluşturma

1. Veri Fabrikası **UI'nin başlangıç** **sayfasında, veri hattı oluştur'u**seçin.

    ![Veri Fabrikası ardışık hattı oluşturma](media/tutorial-sync-data-factory/data-factory-get-started.png)

2. Ardışık hatlar için **Özellikler** penceresinin **Genel** sayfasında, ad için **PeriodicSync'i** girin.

3. Eski filigran değerini almak için Arama etkinliğini ekleyin. **Etkinlikler** **bölmesinde, Genel'i** genişletin ve **Arama** etkinliğini boru hattı tasarımcısı yüzeyine sürükleyin. Etkinliğin adını **OldWatermark**olarak değiştirin.

    ![Eski filigran aramasını ekleyin](media/tutorial-sync-data-factory/create-old-watermark-lookup.png)

4. **Ayarlar** sekmesine geçin ve **Kaynak Veri Kümesi**için **Yeni'yi** seçin. Şimdi filigran tablosundaki verileri temsil edecek bir veri kümesi oluşturursunuz. Bu tablo, önceki kopyalama işleminde kullanılan eski filigranı içerir.

5. Yeni **Veri Kümesi** penceresinde **Azure SQL Server'ı**seçin ve ardından **Devam et'i**seçin.  

6. Veri kümesiiçin **Set özellikleri** penceresinde, **Ad**altında, **FiligranDataset**girin.

7. **Bağlantılı Hizmet**için **Yeni'yi**seçin ve ardından şu adımları tamamlayın:

    1. **Ad**altında, **SQLDBEdgeLinkedService**girin.

    2. **Sunucu adı altında,** SQL Database Edge sunucu bilgilerinizi girin.

    3. Listeden **Veritabanı adınızı** seçin.

    4. Kullanıcı **adınızı** ve **Şifrenizi**girin.

    5. SQL Database Edge örneğine bağlantıyı sınamak için **Test bağlantısını**seçin.

    6. **Oluştur'u**seçin.

    ![Bağlı hizmet oluşturma](media/tutorial-sync-data-factory/create-linked-service.png)

    7. **Tamam'ı**seçin.

8. **Ayarlar** sekmesinde, **Edit'i**seçin.

9. **Bağlantı** sekmesinde **[dbo]'yu seçin.[ ** **tablo**için filigran] . Tablodaki verileri önizlemek istiyorsanız, **Verileri Önizleme'yi**seçin.

10. Üstteki ardışık ardışık sekmesini seçerek veya soldaki ağaç görünümünde ardışık ardışık dizinin adını seçerek ardışık hat lar düzenleyicisine geçin. Arama etkinliğinin özellikler penceresinde, **Kaynak veri kümesi** listesinde **FiligranDataset'in** seçildiğini doğrulayın.

11. **Etkinlikler** bölmesinde, **Genel'i** genişletin ve başka bir **Arama** etkinliğini boru hattı tasarımcısı yüzeyine sürükleyin. Özellikler penceresinin **Genel** sekmesinde adı **NewWatermark** olarak ayarlayın. Bu Arama etkinliği, hedefe kopyalanabilmesi için kaynak verileri içeren tablodan yeni filigran değerini alır.

12. İkinci Arama etkinliği için özellikler penceresinde, **Ayarlar** sekmesine geçin ve yeni filigran değerini içeren kaynak tabloya işaret etmek için bir veri kümesi oluşturmak için **Yeni'yi** seçin.

13. Yeni **Veri Kümesi** penceresinde, **SQL Database Edge örneğini**seçin ve ardından **Devam et'i**seçin.

    1. Set **özellikleri** penceresinde, **Ad**altında, **SourceDataset**girin. **Bağlantılı hizmet**altında **SQLDBEdgeLinkedService'i**seçin.

    2. **Tablo'nun**altında, eşitlemek istediğiniz tabloyu seçin. Bu öğreticide daha sonra açıklandığı gibi, bu veri kümesi için bir sorgu da belirtebilirsiniz. Sorgu, bu adımda belirttiğiniz tablodan önce gelir.

    3. **Tamam'ı**seçin.

14. Üstteki ardışık ardışık sekmesini seçerek veya soldaki ağaç görünümünde ardışık ardışık dizinin adını seçerek ardışık hat lar düzenleyicisine geçin. Arama etkinliğinin özellikler penceresinde, **Kaynak veri kümesi** listesinde **SourceDataset'in** seçildiğini doğrulayın.

15. **Sorgu'nun**altında **Sorgula'yı** seçin. Aşağıdaki sorgudaki tablo adını güncelleştirin ve ardından sorguyu girin. `timestamp` Tablodan yalnızca maksimum değeri seçmiyoruz. Yalnızca İlk **satırı**seçtiğinizden emin olun.

    ```sql
    select MAX(timestamp) as NewWatermarkvalue from [TableName]
    ```

    ![sorguseçin](media/tutorial-sync-data-factory/select-query-data-factory.png)

16. **Etkinlikler** bölmesinde, **Taşı & Dönüştür'&** genişletin ve **Kopyalama** etkinliğini **Etkinlikler** bölmesinden tasarımcı yüzeyine sürükleyin. Etkinliğin adını **IncrementalCopy**olarak ayarlayın.

17. Arama etkinliklerine bağlı yeşil düğmeyi Kopyalama etkinliğine sürükleyerek her iki Arama etkinliğini Kopyalama etkinliğine bağlayın. Kopya etkinliği kenar renginin maviye değiştiğini gördüğünüzde fare düğmesini bırakın.

18. Kopyalama etkinliğini seçin ve **Özellikler** penceresinde etkinliğin özelliklerini gördüğünüzü onaylayın.

19. **Özellikler** penceresindeki **Kaynak** sekmesine geçin ve aşağıdaki adımları tamamlayın:

    1. Kaynak **veri kümesi** **kutusunda, SourceDataset'i**seçin.

    2. **Sorgukullan'ın**altında **Sorgu'yı**seçin.

    3. **Sorgu** kutusuna SQL sorgusunu girin. Örnek bir sorgu aşağıda veda edebilirsiniz:

    ```sql
    select * from TemperatureSensor where timestamp > '@{activity('OldWaterMark').output.firstRow.WatermarkValue}' and timestamp <= '@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}'
    ```

20. **Lavabo** sekmesinde, Lavabo **Veri Kümesi**altında **Yeni'yi** seçin.

21. Bu öğreticide, lavabo veri deposu bir Azure Blob depolama veri deposudur. **Azure Blob depolama alanını**seçin ve ardından Yeni Veri **Kümesi** penceresinde **Devam et'i** seçin.

22. Biçim **Seç** penceresinde, verilerinizin biçimini seçin ve sonra **Devam et'i**seçin.

23. Özellikleri **Ayarla** penceresinde, **Ad** **altında, SinkDataset'i**girin. **Bağlantılı hizmet**altında, **Yeni'yi**seçin. Artık Azure Blob depolama alanınıza bağlantı (bağlantılı bir hizmet) oluşturursunuz.

24. Yeni **Bağlantılı Hizmet (Azure Blob depolama)** penceresinde aşağıdaki adımları tamamlayın:

    1. **Ad** kutusuna **AzureStorageLinkedService'i**girin.

    2. **Depolama hesabı adı**altında, Azure aboneliğiniz için Azure depolama hesabını seçin.

    3. Bağlantıyı test edin ve **ardından Finish'i**seçin.

25. Özellikleri **Ayarla** penceresinde, **AzureStorageLinkedService'in** **Bağlantılı hizmet**altında seçildiğini onaylayın. **Oluştur** ve **Tamam'ı**seçin.

26. **Lavabo** sekmesinde, **Edit'i**seçin.

27. SinkDataset'in **Bağlantı** sekmesine gidin ve aşağıdaki adımları tamamlayın:

    1. **Dosya yolu**altında , *asdedatasync/artımlı kopya*girin , *asdedatasync* blob konteyner adı ve *artımlı kopya* klasör adıdır. Yoksa kapsayıcıyı oluşturun veya varolan ın adını kullanın. Azure Veri Fabrikası, yoksa çıktı klasörünü otomatik olarak *artımlı olarak* oluşturur. Bir blob kapsayıcısındaki klasörlerden birine gitmek istiyorsanız **Dosya yolu** için **Gözat** düğmesini de kullanabilirsiniz.

    2. **Dosya yolunun** **Dosya** bölümü için **dinamik içerik ekle [Alt+P]** seçeneğini belirleyin ve ardından ** @CONCAT('Artımlı-', ardışık() girin. RunId, '.txt')** açılan pencerede. **Bitiş'i**seçin. Dosya adı ifade tarafından dinamik olarak oluşturulur. Her işlem hattı çalıştırması benzersiz bir kimliğe sahiptir. Kopyalama etkinliği, dosya adını oluşturmak için çalışma kimliğini kullanır.

28. Üstteki ardışık ardışık sekmesini seçerek veya soldaki ağaç görünümünde ardışık ardışık dizinin adını seçerek ardışık hat lar düzenleyicisine geçin.

29. **Etkinlikler** **bölmesinde, Genel'i** genişletin ve **Depolanan Yordam** etkinliğini **Etkinlikler** bölmesinden boru hattı tasarımcısı yüzeyine sürükleyin. Kopyalama etkinliğinin yeşil (başarı) çıktısını Depolanan Yordam etkinliğine bağlayın.

30. Ardışık hat tasarımcısında **Depolanan Yordam Etkinliği'ni** seçin ve adını **SPtoUpdateWatermarkActivity**olarak değiştirin.

31. **SQL Hesabı** sekmesine geçin ve **Bağlantılı hizmet**altında ***QLDBEdgeLinkedService'i** seçin.

32. **Depolanan Yordam** sekmesine geçin ve aşağıdaki adımları tamamlayın:

    1. **Depolanan yordam adı altında** **[dbo] seçeneğini belirleyin.[ usp_write_watermark]**.

    2. Depolanan yordam parametreleri için değerleri belirtmek için **Alma parametresini** seçin ve parametreler için şu değerleri girin:

    |Adı|Tür|Değer|
    |-----|----|-----|
    |LastModifiedtime|DateTime|@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}|
    |TableName|Dize|@{activity('OldWaterMark').output.firstRow.TableName}|

33. Boru hattı ayarlarını doğrulamak için araç çubuğunda **Doğrula'yı** seçin. Doğrulama hatası olmadığından emin olun. **Pipeline Doğrulama Raporu** penceresini kapatmak **>>** için .

34. **Tümünü Yayımla** düğmesini seçerek varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık hatlar) Azure Veri Fabrikası hizmetine yayımlayın. Yayımlama işleminin başarılı olduğunu onaylayan bir ileti görene kadar bekleyin.

## <a name="trigger-a-pipeline-based-on-a-schedule"></a>Zamanlamaya dayalı bir ardışık bilgi

1. Pipeline araç çubuğunda **Tetikleyici Ekle'yi**, **Yeni/Edit'i**seçin ve ardından **Yeni'yi**seçin.

2. Tetikleyicinize **HourlySync**adını verebinizi adlandırın. **Türü**altında , **Zamanlama'yı**seçin. **Yinelemeyi** her 1 saatte bir ayarlayın.

3. **Tamam'ı**seçin.

4. **Tümünü Yayımla**.

5. **Şimdi Tetikle'yi**seçin.

6. Soldaki **İzleyici** sekmesine geçin. El ile tetikleme işlemi tarafından tetiklenen işlem hattı çalıştırmasının durumunu görebilirsiniz. Listeyi yenilemek için **Yenile**’yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticideki Azure Veri Fabrikası ardışık hattı, sql veritabanı kenarı örneğindeki bir tablodaki verileri her saat bir kez Azure Blob depolamasındaki bir konuma kopyalar. Diğer senaryolarda Veri Fabrikası'nı kullanma hakkında bilgi edinmek için şu [öğreticilere](../data-factory/tutorial-copy-data-portal.md)bakın.
