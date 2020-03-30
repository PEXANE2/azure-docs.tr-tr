---
title: Azure SQL Database Edge ile SQL Veritabanı DAC paketlerini ve Akış Analizi işlerini kullanma | Microsoft Dokümanlar
description: SQL Database Edge'de Stream Analytics işlerini kullanma hakkında bilgi edinin
keywords: sql veritabanı kenarı, akış analitiği, sqlpackage
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 21a8bb6953fd879b17816361f536596571678697
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74384168"
---
# <a name="using-sql-database-dac-packages-and-stream-analytics-jobs-with-sql-database-edge"></a>SQL Database DAC paketlerini ve Stream Analytics işlerini SQL Database Edge ile kullanma

Azure SQL Veritabanı Kenarı Önizleme, IoT ve kenar dağıtımları için optimize edilmiş bir ilişkisel veritabanı altyapısıdır. Endüstri lideri performans, güvenlik ve sorgu işleme özellikleri sağlayan Microsoft SQL Server Database Engine'in en son sürümleriüzerine kurulmuştur. Azure SQL Database Edge, SQL Server'ın sektör lideri ilişkisel veritabanı yönetimi özelliklerinin yanı sıra, gerçek zamanlı analiz ve karmaşık olay işleme için yerleşik akış özelliği sağlar.

Azure SQL Database Edge ayrıca SQL Database Edge dağıtımı sırasında bir [SQL Database DAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications) paketi dağıtmanızı sağlayan SqlPackage.exe'nin yerel bir uygulamasını da sağlar.

Azure SQL Veritabanı Kenarı, IoT Edge modülü `module twin's desired properties` seçeneği ile iki isteğe bağlı parametre yi ortaya çıkarır:

```json
{
    "properties.desired":
    {
        "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
        "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
    }
}
```

|Alan | Açıklama |
|------|-------------|
| SqlPackage | SQL Database DAC paketini içeren *.zip dosyası için Azure Blob depolama URI.
| ASAJobInfo | ASA Edge işi için Azure Blob depolama URI. Daha fazla bilgi için, [SQL Database Edge için asa edge işi yayımlama'ya](/azure/sql-database-edge/stream-analytics#using-streaming-jobs-with-sql-database-edge)bakın.

## <a name="using-sql-database-dac-packages-with-sql-database-edge"></a>SQL Database DAC paketlerini SQL Database Edge ile kullanma

SQL Database Edge ile SQL Database DAC paketini (*.dacpac) kullanmak için aşağıdaki adımları izleyin:

1. Bir SQL Veritabanı DAC paketi oluşturun veya ayıklayın. Bkz. Varolan bir SQL Server veritabanı için DAC paketinin nasıl oluşturacağı hakkında bilgi almak için [veritabanından bir DAC çıkarma.](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/)

2. *.dacpac'ı zip'ten çıkarın ve Azure Blob depolama hesabına yükleyin. Azure Blob depolama alanına dosya yükleme hakkında daha fazla bilgi için Azure [portalıyla yükleme, indirme ve liste bloblarına](../storage/blobs/storage-quickstart-blobs-portal.md)bakın.

3. Azure portalını kullanarak zip dosyası için paylaşılan bir erişim imzası oluşturun. Daha fazla bilgi için, [paylaşılan erişim imzaları (SAS) ile Temsilci erişimine](../storage/common/storage-sas-overview.md)bakın.

4. DAC paketi için paylaşılan erişim URI'yi içerecek şekilde SQL Database Edge modül yapılandırmasını güncelleştirin. SQL Database Edge modüllerini güncelleştirmek için aşağıdaki adımları izleyin:

    1. Azure portalında IoT Hub dağıtımınıza gidin.

    2. Sol bölmede **IoT Edge'i**seçin.

    3. **IoT Edge** sayfasında, SQL Database Edge modülünün dağıtıldığı IoT kenarını bulun ve seçin.

    4. **IoT Edge Device** aygıt sayfasında **Modülü Ayarla'yı**seçin.

    5. **Modülleri Ayarla** sayfasında, SQL Veritabanı Kenarı modülüne göre **Yapılandır'ı** seçin.

    6. **IoT Edge Özel Modüller** bölmesinde, **modül ikizinin istenilen özelliklerini belirleyin.** Aşağıdaki örnekte gösterildiği gibi, `SQLPackage` seçenek için URI'yi içerecek şekilde istenen özellikleri güncelleştirin.

        > [!NOTE]
        > Aşağıdaki JSON SAS URI sadece bir örnektir. Uri'yi dağıtımınızdan gerçek URI ile değiştirin.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "https://StorageAccountName.blob.core.windows.net/SQLpackageFiles/MeasurementsDB.zip?sp=r&st=2019-09-01T00:00:00Z&se=2019-09-30T00:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=Uh8X0E50qzlxkiKVBJKU3ccVQYwF235qTz7AXp4R3gI%3D",
                }
            }
        ```

    7. **Kaydet'i**seçin.

    8. **Modülleri Ayarla** sayfasında **İleri'yi**seçin.

    9. **Modülleri Ayarla** sayfasında **İleri'yi** seçin ve sonra **Gönder'i**seçin.

5. Modül güncelleştirmeden sonra, DAC paket dosyası indirilir, fermuarsız ve SQL Database Edge örneğine karşı dağıtılır.

## <a name="using-streaming-jobs-with-sql-database-edge"></a>SQL Database Edge ile akış işlerini kullanma

Azure SQL Database Edge akış analizi çalışma zamanı nın yerel bir uygulamasına sahiptir. Bu uygulama, bir Azure Akışı Analizi kenar işi oluşturmanıza ve bu işi SQL Database Edge akış işi olarak dağıtmanıza olanak tanır. Akış Analizi kenar işi oluşturmak için şu adımları tamamlayın:

1. Önizleme [URL'sini](https://portal.azure.com/?microsoft_azure_streamanalytics_edgeadapterspreview=true)kullanarak Azure portalına gidin. Bu önizleme URL'si, Akış Analizi kenar işi için SQL Veritabanı çıktısını yapılandırmanızı sağlar.

2. **IoT Edge** işinde yeni bir Azure Akışı Analizi oluşturun. **Edge'i**hedefleyen barındırma ortamını seçin.

3. Azure Akış Analizi işi için bir giriş ve çıktı tanımlayın. Burada ayarladığınız her SQL çıkışı veritabanındaki tek bir tabloya bağlıdır. Verileri birden çok tabloya akışınız gerekiyorsa, birden çok SQL Veritabanı çıkışı oluşturmanız gerekir. SQL çıktılarını farklı veritabanlarına işaret etmek için yapılandırabilirsiniz.

    **Giriş**. Kenar işi için giriş olarak EdgeHub'ı seçin ve kaynak bilgilerini sağlayın.

    **Çıktı**. Sql Veritabanı'nı çıktı olarak seçin. **SQL Veritabanı ayarlarını el ile sağla**seçeneğini belirleyin. Veritabanı ve tablo için yapılandırma ayrıntılarını sağlayın.

    |Alan      | Açıklama |
    |---------------|-------------|
    |Çıktı diğer adı | Çıktı diğer adının adı.|
    |Database | SQL veritabanının adı. SQL Database Edge örneğinde bulunan bir veritabanının geçerli bir adı olması gerekir.|
    |Sunucu adı | SQL örneğinin adı (veya IP adresi) ve bağlantı noktası numarası ayrıntıları. SQL Database Edge dağıtımı için sunucu adı için **tcp:.,1433'u** kullanabilirsiniz.|
    |Kullanıcı adı | Veri okuyucu ve veri yazarının daha önce belirttiğiniz veritabanına erişimi olan SQL oturum açma hesabı.|
    |Parola | Daha önce belirttiğiniz SQL oturum açma hesabının parolası.|
    |Tablo | Akış işi için çıktı olacak tablonun adı.|
    |Devralma Bölümleme| Önceki sorgu adımınızın veya girişinizin bölümleme düzeninin devralılmasını sağlar. Bu seçenek etkinleştirildiğinde, disk tabanlı bir tabloya yazarken ve işiniz için tamamen paralel bir topolojiye sahip olduğunuzda daha iyi iş elde etme yöntemini görmeyi bekleyebilirsiniz.|
    |Toplu İş Boyutu| Her toplu ekleme hareketiyle gönderilen maksimum kayıt sayısı.|

    Örnek giriş/çıkış yapılandırması aşağıda veda edinebilirsiniz:

    ```txt
        Input:
            Input from EdgeHub
            Input alias: input
            Event serialization format: JSON
            Encoding: UTF-8
            Event compression type: None

        Output:
            Output alias: output
            Database:  MeasurementsDB
            Server name: tcp:.,1433
            Username: sa
            Password: <Password>
            Table: TemperatureMeasurements
            Inherit Partitioning: Merge all input partitions into a single writer
            Max batch count: 10000
    ```

    > [!NOTE]
    > Azure Akış Analizi için SQL çıkış bağdaştırıcısı hakkında daha fazla bilgi için Azure [Akış Analizi çıktısı için Azure SQL Veritabanı'na](../stream-analytics/stream-analytics-sql-output-perf.md)bakın.

4. Kenar işi için ASA iş sorgusunu tanımlayın. Bu sorgu, sorgudaki giriş ve çıktı adları olarak tanımlanan girdi/çıktı diğer adlarını kullanmalıdır. Daha fazla bilgi için [Bkz. Akış Analizi Sorgu Dili başvurusu.](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)

5. Kenar işi için depolama hesabı ayarlarını ayarlayın. Depolama hesabı kenar işi için yayımlama hedefi olarak kullanılır.

6. **Yapıla'nın**altında **Yayımla'yı**seçin ve ardından **Yayımla** düğmesini seçin. SAS URI'yi SQL Database Edge modülünde kullanmak üzere kaydedin.

### <a name="deploy-the-stream-analytics-edge-job-to-sql-database-edge"></a>Akış Analizi kenar işini SQL Veritabanı Kenarına dağıtma

Akış işini SQL Database Edge modülüne dağıtmak için, önceki adımdan akış işi için SAS URI'yi içerecek şekilde SQL Database Edge modülü yapılandırmasını güncelleştirin. SQL Database Edge modüllerini güncelleştirmek için:

1. Azure portalında IoT Hub dağıtımınıza gidin.

2. Sol bölmede **IoT Edge'i**seçin.

3. **IoT Edge** sayfasında, SQL Database Edge modülünün dağıtıldığı IoT kenarını bulun ve seçin.

4. **IoT Edge Device** aygıt sayfasında **Modülü Ayarla'yı**seçin.

5. **Modülleri Ayarla** sayfasında, SQL Veritabanı Kenarı modülüne göre **Yapılandır'ı** seçin.

6. **IoT Edge Özel Modüller** bölmesinde, **modül ikizinin istenilen özelliklerini belirleyin.** Aşağıdaki örnekte gösterildiği gibi, `ASAJobInfo` seçenek için URI'yi içerecek şekilde istenen özellikleri güncelleştirin.

    > [!NOTE]
    > Aşağıdaki JSON SAS URI sadece bir örnektir. Uri'yi dağıtımınızdan gerçek URI ile değiştirin.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "https://storageAccountName.blob.core.windows.net/asajobs/ASAEdgeJobs/5a9b34db-7a5c-4606-adae-4c8609eaa1c7/d85b5aa6-4d38-4703-bb34-af7f0bd7916d/ASAEdgeJobDefinition.zip?sv=2018-03-28&sr=b&sig=HH%2BFMsEy378RaTxIy2Xo6rM6wDaqoBaZ5zFDBqdZiS0%3D&st=2019-09-01T22%3A24%3A34Z&se=2019-09-30T22%3A34%3A34Z&sp=r"   
            }
        }
    ```

7. **Kaydet'i**seçin.

8. **Modülleri Ayarla** sayfasında **İleri'yi**seçin.

9. **Modülleri Ayarla** sayfasında **İleri'yi** seçin ve sonra **Gönder'i**seçin.

10. Modül güncelleştirmesi sonrasında akış analizi iş dosyası indirilir, günaçmaz ve SQL Database Edge örneğine karşı dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

- Fiyatlandırma ve kullanılabilirlik ayrıntıları için [Azure SQL Veritabanı Kenarı'na](https://azure.microsoft.com/services/sql-database-edge/)bakın.
- Aboneliğiniz için Azure SQL Database Edge'i etkinleştirme isteğinde bulunun.
- Başlamak için Azure [portalı üzerinden SQL Veritabanı Kenarı Dağıt'a](deploy-portal.md)bakın.
