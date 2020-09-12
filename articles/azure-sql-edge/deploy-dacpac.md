---
title: SQL veritabanı DACPAC ve BACPAC paketlerini kullanma-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de DACPACs ve BacPacs kullanma hakkında bilgi edinin (Önizleme)
keywords: SQL Edge, SqlPackage
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/03/2020
ms.openlocfilehash: 52c8e9586d8ee53cdaac28cb1c48d2927d82c2ed
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89462768"
---
# <a name="sql-database-dacpac-and-bacpac-packages-in-sql-edge"></a>SQL Edge 'de SQL veritabanı DACPAC ve BACPAC paketleri

Azure SQL Edge (Önizleme), IoT ve Edge dağıtımları için iyileştirilmiş bir ilişkisel veritabanı altyapısıdır. Bu, sektör lideri performans, güvenlik ve sorgu işleme özellikleri sağlayan Microsoft SQL Server veritabanı altyapısının en son sürümlerinde oluşturulmuştur. Azure SQL Edge, SQL Server sektör lideri ilişkisel veritabanı yönetim özelliklerinin yanı sıra gerçek zamanlı analiz ve karmaşık olay işleme için yerleşik akış özelliği sağlar.

Azure SQL Edge, SQL Edge dağıtımı sırasında bir [SQL veritabanı DACPAC ve BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications) paketi dağıtmanızı sağlayan SqlPackage.exe yerel bir uygulamasını da sağlar. SQL veritabanı DACPACs, SQL Edge modülünün seçeneği aracılığıyla ortaya çıkarılan SqlPackage parametresi kullanılarak SQL Edge 'e dağıtılabilir `module twin's desired properties` :

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
| SqlPackage | SQL veritabanı DAC veya BACPAC paketini içeren *. zip* dosyası Için Azure Blob depolama URI 'si. ZIP dosyası, hem birden çok dac paketi hem de bacpac dosyası içerebilir.
| Aşama Jobınfo | ASA Edge işi için Azure Blob depolama URI 'SI.

## <a name="use-a-sql-database-dac-package-with-sql-edge"></a>SQL Edge ile SQL veritabanı DAC paketi kullanma

SQL veritabanı DAC paketini `(*.dacpac)` veya BACPAC dosyasını `(*.bacpac)` SQL Edge ile kullanmak için şu adımları izleyin:

1. Bir DAC paketi oluşturun/ayıklayın veya aşağıda bahsedilen mekanizmayı kullanarak bacpac dosyasını dışarı aktarın. 
    - Bir SQL veritabanı DAC paketi oluşturun veya ayıklayın. Mevcut bir SQL Server veritabanı için DAC paketi oluşturma hakkında bilgi için bkz. [bir VERITABANıNDAN dac ayıklama](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/) .
    - Dağıtılan bir DAC paketini veya veritabanını dışarı aktarma. Mevcut bir SQL Server veritabanı için bacpac dosyası oluşturma hakkında bilgi için bkz. [veri katmanı uygulamasını dışarı aktarma](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application/) .

2. `*.dacpac`Veya `*.bacpac` dosyasını zip olarak bir Azure Blob depolama hesabına yükleyin. Azure Blob depolamaya dosya yükleme hakkında daha fazla bilgi için bkz. [Azure Portal blob 'Ları yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Azure portal kullanarak ZIP dosyası için paylaşılan erişim imzası oluşturun. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) ile erişim temsilcisi](../storage/common/storage-sas-overview.md).

4. SQL Edge modülü yapılandırmasını, DAC paketinin paylaşılan erişim URI 'sini içerecek şekilde güncelleştirin. SQL Edge modülünü güncelleştirmek için şu adımları uygulayın:

    1. Azure portal IoT Hub dağıtımınıza gidin.

    2. Sol bölmede **IoT Edge**' yi seçin.

    3. **IoT Edge** SAYFASıNDA, SQL Edge modülünün dağıtıldığı IoT Edge bulun ve seçin.

    4. Cihaz cihazı **IoT Edge** sayfasında, **modülü ayarla**' yı seçin.

    5. **Modülleri ayarla** sayfasında SQL Edge modülüne göre **Yapılandır** ' ı seçin.

    6. **IoT Edge özel modüller** bölmesinde, **module ikizi 'ın istenen özelliklerini ayarla**' yı seçin. `SQLPackage`Aşağıdaki örnekte gösterildiği gibi, SEÇENEĞININ URI 'sini dahil etmek için istenen özellikleri güncelleştirin.

        > [!NOTE]
        > Aşağıdaki JSON 'daki SAS URI 'SI yalnızca bir örnektir. URI 'yi dağıtımınızdaki gerçek URI ile değiştirin.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "<<<SAS URL for the *.zip file containing the dacpac and/or the bacpac files",
                }
            }
        ```

    7. **Kaydet**’i seçin.

    8. **Modülleri ayarla** sayfasında, **İleri**' yi seçin.

    9. **Modülleri ayarla** sayfasında, **İleri** ' yi ve ardından **Gönder**' i seçin.

5. Modül güncelleştirildikten sonra, paket dosyası indirilir, sıkıştırıldı ve SQL Edge örneğine göre dağıtılır.

Azure SQL Edge kapsayıcısının her yeniden başlatıldığında `*.dacpac` dosya paketi indirilir ve değişiklikler için değerlendirilir. Dacpac dosyasının yeni bir sürümüne karşılaşılırsa, değişiklikler veritabanına SQL Edge 'de dağıtılır. Bacpac dosyaları 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal ÜZERINDEN SQL Edge dağıtın](deploy-portal.md).
- [Akış verileri](stream-data.md)
- [SQL Edge 'de ONNX ile Machine Learning ve AI (Önizleme)](onnx-overview.md)
