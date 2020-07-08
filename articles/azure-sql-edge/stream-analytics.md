---
title: Azure SQL Edge ile Azure Stream Analytics Edge işleri kullanma (Önizleme)
description: Azure SQL Edge 'de Stream Analytics işleri kullanma hakkında bilgi edinin (Önizleme)
keywords: SQL Edge, Stream Analytics,
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e323c56f22c3a7d32bda5afe03a3462d3263f0d0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85254081"
---
# <a name="using-azure-stream-analytics-jobs-with-sql-edge"></a>SQL Edge ile Azure Stream Analytics işleri kullanma

Azure SQL Edge (Önizleme), IoT ve Edge dağıtımları için iyileştirilmiş bir ilişkisel veritabanı altyapısıdır. Bu, sektör lideri performans, güvenlik ve sorgu işleme özellikleri sağlayan Microsoft SQL Server veritabanı altyapısının en son sürümlerinde oluşturulmuştur. Azure SQL Edge, SQL Server sektör lideri ilişkisel veritabanı yönetim özelliklerinin yanı sıra gerçek zamanlı analiz ve karmaşık olay işleme için yerleşik akış özelliği sağlar.

Azure SQL Edge, Stream Analytics çalışma zamanının yerel bir uygulamasına sahiptir. Bu uygulama, bir Azure Stream Analytics Edge işi oluşturmanızı ve bu işi SQL Edge akış işi olarak dağıtmanızı sağlar. Azure Stream Analytics işleri, SQL Edge modülünün seçeneği aracılığıyla ortaya çıkarılan aşama Jobınfo parametresi kullanılarak SQL Edge 'e dağıtılabilir `module twin's desired properties` :

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
| Aşama Jobınfo | ASA Edge işi için Azure Blob depolama URI 'SI.

## <a name="create-an-azure-stream-analytics-edge-job"></a>Azure Stream Analytics Edge Işi oluşturma

1. Azure portalına gidin.

2. Yeni bir **IoT Edge üzerinde Azure Stream Analytics** işi oluşturun. **Kenarı**hedefleyen barındırma ortamını seçin.

3. Azure Stream Analytics işi için bir giriş ve çıkış tanımlayın. Burada ayarlanacak her SQL çıktısı, veritabanındaki tek bir tabloya bağlıdır. Verileri birden çok tabloya akışı gerekiyorsa, birden çok SQL veritabanı çıkışı oluşturmanız gerekir. SQL çıktılarını farklı veritabanlarına işaret etmek için yapılandırabilirsiniz.

    **Giriş**. Edge işinin girişi olarak EdgeHub ' ı seçin ve kaynak bilgilerini sağlayın.

    **Çıktı**. Çıkış olarak SQL veritabanı ' nı seçin. **SQL veritabanı ayarlarını el Ile sağla**' yı seçin. Veritabanı ve tablo için yapılandırma ayrıntılarını sağlayın.

    |Alan      | Açıklama |
    |---------------|-------------|
    |Çıktı diğer adı | Çıkış diğer adının adı.|
    |Veritabanı | Veritabanının adı. SQL Edge örneğinde bulunan bir veritabanının geçerli bir adı olması gerekir.|
    |Sunucu adı | SQL örneği için ad (veya IP adresi) ve bağlantı noktası numarası ayrıntıları. Bir SQL Edge dağıtımı için sunucu adı için **TCP:., 1433** kullanabilirsiniz.|
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

6. **Yapılandır**altında **Yayımla**' yı seçin ve ardından **Yayınla** düğmesini seçin. SAS URI 'sini SQL Edge modülüyle kullanılmak üzere kaydedin.

## <a name="deploy-azure-stream-analytics-edge-job-to-sql-edge"></a>Azure Stream Analytics Edge işini SQL Edge 'e dağıtma

Akış işini SQL Edge modülüne dağıtmak için SQL Edge modülü yapılandırmasını, önceki adımdan akış işi için SAS URI 'sini içerecek şekilde güncelleştirin. SQL Edge modülünü güncelleştirmek için:

1. Azure portal IoT Hub dağıtımınıza gidin.

2. Sol bölmede **IoT Edge**' yi seçin.

3. **IoT Edge** SAYFASıNDA, SQL Edge modülünün dağıtıldığı IoT Edge bulun ve seçin.

4. Cihaz cihazı **IoT Edge** sayfasında, **modülü ayarla**' yı seçin.

5. **Modülleri ayarla** sayfasında SQL Edge modülüne göre **Yapılandır** ' ı seçin.

6. **IoT Edge özel modüller** bölmesinde, **module ikizi 'ın istenen özelliklerini ayarla**' yı seçin. `ASAJobInfo`Aşağıdaki örnekte gösterildiği gibi, SEÇENEĞININ URI 'sini dahil etmek için istenen özellikleri güncelleştirin.

    > [!NOTE]
    > Aşağıdaki JSON 'daki SAS URI 'SI yalnızca bir örnektir. URI 'yi dağıtımınızdaki gerçek URI ile değiştirin.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "<<<SAS URL For the published ASA Edge Job>>>>"
            }
        }
    ```

7. **Kaydet**'i seçin.

8. **Modülleri ayarla** sayfasında, **İleri**' yi seçin.

9. **Modülleri ayarla** sayfasında, **İleri** ' yi ve ardından **Gönder**' i seçin.

10. Modül güncelleştirmesinden sonra Stream Analytics iş dosyası indirilir, sıkıştırıldı ve SQL Edge örneğine göre dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal ÜZERINDEN SQL Edge dağıtın](deploy-portal.md).

- [Akış verileri](stream-data.md)

- [SQL Edge 'de ONNX ile Machine Learning ve AI (Önizleme)](onnx-overview.md)
