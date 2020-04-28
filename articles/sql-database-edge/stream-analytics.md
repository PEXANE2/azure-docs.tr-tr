---
title: Azure SQL veritabanı Edge ile SQL veritabanı DAC paketlerini ve Stream Analytics işlerini kullanma | Microsoft Docs
description: SQL veritabanı Edge 'de Stream Analytics işlerini kullanma hakkında bilgi edinin
keywords: SQL veritabanı Edge, Stream Analytics, SqlPackage
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 21a8bb6953fd879b17816361f536596571678697
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74384168"
---
# <a name="using-sql-database-dac-packages-and-stream-analytics-jobs-with-sql-database-edge"></a>SQL veritabanı ile SQL veritabanı DAC paketlerini ve Stream Analytics işlerini kullanma

Azure SQL veritabanı Edge önizlemesi, IoT ve Edge dağıtımları için iyileştirilmiş bir ilişkisel veritabanı altyapısıdır. Bu, sektör lideri performans, güvenlik ve sorgu işleme özellikleri sağlayan Microsoft SQL Server veritabanı altyapısının en son sürümlerinde oluşturulmuştur. Azure SQL veritabanı Edge, SQL Server sektör lideri ilişkisel veritabanı yönetim özelliklerinin yanı sıra gerçek zamanlı analiz ve karmaşık olay işleme için yerleşik akış özelliği sağlar.

Azure SQL veritabanı Edge, SQL veritabanı kenarının dağıtımı sırasında bir [SQL VERITABANı dac](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications) paketi dağıtmanıza olanak sağlayan, SqlPackage. exe ' nin yerel bir uygulamasını da sağlar.

Azure SQL veritabanı Edge, IoT Edge modülünün `module twin's desired properties` seçeneği aracılığıyla iki isteğe bağlı parametre kullanıma sunar:

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
| SqlPackage | SQL veritabanı DAC paketini içeren *. zip dosyası için Azure Blob depolama URI 'SI.
| Aşama Jobınfo | ASA Edge işi için Azure Blob depolama URI 'SI. Daha fazla bilgi için bkz. [SQL veritabanı Edge IÇIN asa Edge Işi yayımlama](/azure/sql-database-edge/stream-analytics#using-streaming-jobs-with-sql-database-edge).

## <a name="using-sql-database-dac-packages-with-sql-database-edge"></a>SQL veritabanı kenarıyla SQL veritabanı DAC paketlerini kullanma

SQL veritabanı kenarıyla SQL veritabanı DAC paketi (*. dacpac) kullanmak için şu adımları uygulayın:

1. Bir SQL veritabanı DAC paketi oluşturun veya ayıklayın. Mevcut bir SQL Server veritabanı için DAC paketi oluşturma hakkında bilgi için bkz. [bir VERITABANıNDAN dac ayıklama](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/) .

2. *. Dacpac ZIP ve bir Azure Blob depolama hesabına yükleyin. Azure Blob depolamaya dosya yükleme hakkında daha fazla bilgi için bkz. [Azure Portal blob 'Ları yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Azure portal kullanarak ZIP dosyası için paylaşılan erişim imzası oluşturun. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) ile erişim temsilcisi](../storage/common/storage-sas-overview.md).

4. SQL veritabanı Edge modülü yapılandırmasını, DAC paketinin paylaşılan erişim URI 'sini içerecek şekilde güncelleştirin. SQL veritabanı Edge modülünü güncelleştirmek için şu adımları uygulayın:

    1. Azure portal IoT Hub dağıtımınıza gidin.

    2. Sol bölmede **IoT Edge**' yi seçin.

    3. **IoT Edge** sayfasında SQL veritabanı Edge modülünün dağıtıldığı IoT Edge ' i bulun ve seçin.

    4. Cihaz cihazı **IoT Edge** sayfasında, **modülü ayarla**' yı seçin.

    5. **Modülleri ayarla** sayfasında SQL veritabanı Edge modülüne göre **Yapılandır** ' ı seçin.

    6. **IoT Edge özel modüller** bölmesinde, **module ikizi 'ın istenen özelliklerini ayarla**' yı seçin. Aşağıdaki örnekte gösterildiği gibi, `SQLPackage` seçeneğinin URI 'sini dahil etmek için istenen özellikleri güncelleştirin.

        > [!NOTE]
        > Aşağıdaki JSON 'daki SAS URI 'SI yalnızca bir örnektir. URI 'yi dağıtımınızdaki gerçek URI ile değiştirin.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "https://StorageAccountName.blob.core.windows.net/SQLpackageFiles/MeasurementsDB.zip?sp=r&st=2019-09-01T00:00:00Z&se=2019-09-30T00:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=Uh8X0E50qzlxkiKVBJKU3ccVQYwF235qTz7AXp4R3gI%3D",
                }
            }
        ```

    7. **Kaydet**’i seçin.

    8. **Modülleri ayarla** sayfasında, **İleri**' yi seçin.

    9. **Modülleri ayarla** sayfasında, **İleri** ' yi ve ardından **Gönder**' i seçin.

5. Modül güncelleştirildikten sonra, DAC paket dosyası indirilir, sıkıştırıldı ve SQL veritabanı Edge örneğine göre dağıtılır.

## <a name="using-streaming-jobs-with-sql-database-edge"></a>SQL veritabanı kenarıyla akış işlerini kullanma

Azure SQL veritabanı Edge, Stream Analytics çalışma zamanının yerel bir uygulamasına sahiptir. Bu uygulama, bir Azure Stream Analytics Edge işi oluşturmanızı ve bu işi SQL veritabanı Edge akış işi olarak dağıtmanızı sağlar. Stream Analytics Edge işi oluşturmak için aşağıdaki adımları uygulayın:

1. Önizleme [URL 'sini](https://portal.azure.com/?microsoft_azure_streamanalytics_edgeadapterspreview=true)kullanarak Azure Portal gidin. Bu önizleme URL 'SI, Stream Analytics Edge işi için SQL veritabanı çıktısını yapılandırmanızı sağlar.

2. Yeni bir **IoT Edge üzerinde Azure Stream Analytics** işi oluşturun. **Kenarı**hedefleyen barındırma ortamını seçin.

3. Azure Stream Analytics işi için bir giriş ve çıkış tanımlayın. Burada ayarlanacak her SQL çıktısı, veritabanındaki tek bir tabloya bağlıdır. Verileri birden çok tabloya akışı gerekiyorsa, birden çok SQL veritabanı çıkışı oluşturmanız gerekir. SQL çıktılarını farklı veritabanlarına işaret etmek için yapılandırabilirsiniz.

    **Giriş**. Edge işinin girişi olarak EdgeHub ' ı seçin ve kaynak bilgilerini sağlayın.

    **Çıktı**. Çıkış olarak SQL veritabanı ' nı seçin. **SQL veritabanı ayarlarını el Ile sağla**' yı seçin. Veritabanı ve tablo için yapılandırma ayrıntılarını sağlayın.

    |Alan      | Açıklama |
    |---------------|-------------|
    |Çıktı diğer adı | Çıkış diğer adının adı.|
    |Veritabanı | SQL veritabanının adı. SQL veritabanı Edge örneğinde bulunan bir veritabanının geçerli bir adı olması gerekir.|
    |Sunucu adı | SQL örneği için ad (veya IP adresi) ve bağlantı noktası numarası ayrıntıları. SQL veritabanı Edge dağıtımı için sunucu adı için **TCP:., 1433** kullanabilirsiniz.|
    |Kullanıcı adı | Daha önce belirttiğiniz veritabanına veri okuyucu ve veri yazıcısı erişimi olan SQL oturum açma hesabı.|
    |Parola | Daha önce belirttiğiniz SQL oturum açma hesabının parolası.|
    |Tablo | Akış işi için çıkış olacak tablonun adı.|
    |Bölümlendirmeyi devralma| Önceki sorgu adımlarınızın veya girişinin bölümleme düzeninin devralınmasını mümkün. Bu seçenek etkinleştirildiğinde, disk tabanlı bir tabloya yazdığınızda ve işiniz için tamamen paralel topolojiniz olduğunda daha iyi üretilen iş miktarını görmeyi bekleyebilir.|
    |Toplu İş Boyutu| Her toplu ekleme işlemi ile gönderilen en fazla kayıt sayısı.|

    Örnek bir giriş/çıkış yapılandırması aşağıda verilmiştir:

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
    > Azure Stream Analytics için SQL çıkış bağdaştırıcısı hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'na Azure Stream Analytics çıktısı](../stream-analytics/stream-analytics-sql-output-perf.md).

4. Edge işi için ASA iş sorgusunu tanımlayın. Bu sorgu, sorguda giriş ve çıkış adları olarak tanımlanmış giriş/çıkış diğer adlarını kullanmalıdır. Daha fazla bilgi için bkz. [Stream Analytics sorgu dili başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference).

5. Edge işi için depolama hesabı ayarlarını belirleyin. Depolama hesabı, kenar işi için yayımlama hedefi olarak kullanılır.

6. **Yapılandır**altında **Yayımla**' yı seçin ve ardından **Yayınla** düğmesini seçin. SAS URI 'sini SQL veritabanı Edge modülüyle kullanılmak üzere kaydedin.

### <a name="deploy-the-stream-analytics-edge-job-to-sql-database-edge"></a>Stream Analytics Edge işini SQL veritabanı kenarına dağıtma

Akış işini SQL veritabanı Edge modülüne dağıtmak için SQL veritabanı Edge modülü yapılandırmasını, önceki adımdan akış işi için SAS URI 'sini içerecek şekilde güncelleştirin. SQL veritabanı Edge modülünü güncelleştirmek için:

1. Azure portal IoT Hub dağıtımınıza gidin.

2. Sol bölmede **IoT Edge**' yi seçin.

3. **IoT Edge** sayfasında SQL veritabanı Edge modülünün dağıtıldığı IoT Edge ' i bulun ve seçin.

4. Cihaz cihazı **IoT Edge** sayfasında, **modülü ayarla**' yı seçin.

5. **Modülleri ayarla** sayfasında SQL veritabanı Edge modülüne göre **Yapılandır** ' ı seçin.

6. **IoT Edge özel modüller** bölmesinde, **module ikizi 'ın istenen özelliklerini ayarla**' yı seçin. Aşağıdaki örnekte gösterildiği gibi, `ASAJobInfo` seçeneğinin URI 'sini dahil etmek için istenen özellikleri güncelleştirin.

    > [!NOTE]
    > Aşağıdaki JSON 'daki SAS URI 'SI yalnızca bir örnektir. URI 'yi dağıtımınızdaki gerçek URI ile değiştirin.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "https://storageAccountName.blob.core.windows.net/asajobs/ASAEdgeJobs/5a9b34db-7a5c-4606-adae-4c8609eaa1c7/d85b5aa6-4d38-4703-bb34-af7f0bd7916d/ASAEdgeJobDefinition.zip?sv=2018-03-28&sr=b&sig=HH%2BFMsEy378RaTxIy2Xo6rM6wDaqoBaZ5zFDBqdZiS0%3D&st=2019-09-01T22%3A24%3A34Z&se=2019-09-30T22%3A34%3A34Z&sp=r"   
            }
        }
    ```

7. **Kaydet**’i seçin.

8. **Modülleri ayarla** sayfasında, **İleri**' yi seçin.

9. **Modülleri ayarla** sayfasında, **İleri** ' yi ve ardından **Gönder**' i seçin.

10. Modül güncelleştirmesinden sonra Stream Analytics iş dosyası indirilir, sıkıştırıldı ve SQL veritabanı Edge örneğine göre dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

- Fiyatlandırma ve kullanılabilirlik ayrıntıları için bkz. [Azure SQL veritabanı Edge](https://azure.microsoft.com/services/sql-database-edge/).
- Aboneliğiniz için Azure SQL veritabanı Edge 'i etkinleştirme isteği.
- Başlamak için bkz. [SQL veritabanı Edge 'i Azure Portal Ile dağıtma](deploy-portal.md).
