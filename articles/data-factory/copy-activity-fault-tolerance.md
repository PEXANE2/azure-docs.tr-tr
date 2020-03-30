---
title: Azure Data Factory’de kopyalama etkinliğinin hataya dayanıklılığı
description: Uyumsuz satırları atlayarak Azure Veri Fabrikası'ndaki kopyalama etkinliğine hata toleransı ekleme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: yexu
ms.openlocfilehash: 42c637839172dab09a8721a93a67785a748afd2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75708911"
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Azure Data Factory’de kopyalama etkinliğinin hataya dayanıklılığı
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Geçerli sürüm](copy-activity-fault-tolerance.md)

Azure Veri Fabrikası'ndaki kopyalama etkinliği, kaynak ve lavabo veri depoları arasında veri kopyalarken uyumsuz satırları işlemeniz için iki yol sunar:

- Uyumsuz verilerle (varsayılan davranış) karşılaşıldığında kopyalama etkinliğini iptal edebilir ve başarısız olabilirsiniz.
- Hata toleransı ekleyerek ve uyumsuz veri satırlarını atlayarak tüm verileri kopyalamaya devam edebilirsiniz. Ayrıca, uyumsuz satırları Azure Blob depolama veya Azure Veri Gölü Deposu'nda da günlüğe kaydedebilirsiniz. Daha sonra hatanın nedenini öğrenmek, veri kaynağındaki verileri düzeltmek ve kopyalama etkinliğini yeniden denemek için günlüğü inceleyebilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Kopyalama Etkinliği, uyumsuz verileri algılama, atlama ve günlüğe kaydetme için üç senaryoyu destekler:

- **Kaynak veri türü ile lavabo yerel türü arasındaki uyumsuzluk.** 

    Örneğin: Blob depolamasındaki bir CSV dosyasındaki verileri üç INT türü sütun içeren şema tanımına sahip bir SQL veritabanına kopyalayın. 123.456.789 gibi sayısal veri içeren CSV dosya satırları başarılı bir şekilde lavabo deposuna kopyalanır. Ancak, 123.456, abc gibi sayısal olmayan değerler içeren satırlar uyumsuz olarak algılanır ve atlanır.

- **Kaynak ve lavabo arasındaki sütun sayısında uyuşmazlık**.

    Örneğin: Blob depolamasındaki bir CSV dosyasındaki verileri altı sütun içeren şema tanımına sahip bir SQL veritabanına kopyalayın. Altı sütun içeren CSV dosya satırları lavabo deposuna başarıyla kopyalanır. Altıdan fazla sütun içeren CSV dosya satırları uyumsuz olarak algılanır ve atlanır.

- **SQL Server/Azure SQL Database/Azure Cosmos DB adresine yazarken birincil anahtar ihlali.**

    Örneğin: Verileri BIR SQL sunucusundan SQL veritabanına kopyalayın. Birincil anahtar lavabo SQL veritabanında tanımlanır, ancak kaynak SQL sunucusunda böyle bir birincil anahtar tanımlanmamıştır. Kaynakta bulunan yinelenen satırlar lavaboya kopyalanamaz. Etkinlik kopyalarını yalnızca kaynak verilerin ilk satırını lavaboya kopyalar. Yinelenen birincil anahtar değerini içeren sonraki kaynak satırları uyumsuz olarak algılanır ve atlanır.

>[!NOTE]
>- PolyBase kullanarak SQL Veri Ambarı'na veri yüklemek için, kopyalama etkinliğinde "[polyBaseSettings](connector-azure-sql-data-warehouse.md#azure-sql-data-warehouse-as-sink)" üzerinden reddetme ilkeleri belirterek PolyBase'in yerel hata toleransı ayarlarını yapılandırın. Yine de, PolyBase uyumsuz satırlarının aşağıda gösterildiği gibi normal şekilde Blob veya ADLS'e yönlendirilmesini etkinleştirebilirsiniz.
>- Bu özellik, kopyalama etkinliği [Amazon Redshift Unload](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift)çağırmak için yapılandırıldığınızda geçerli değildir.
>- Bu özellik, kopyalama etkinliği [SQL lavabosundan depolanan](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#invoke-a-stored-procedure-from-a-sql-sink)yordamı çağırmak için yapılandırıldığı zaman geçerli değildir.

## <a name="configuration"></a>Yapılandırma
Aşağıdaki örnek, Kopyalama Etkinliği'ndeki uyumsuz satırları atlayarak yapılandırmak için bir JSON tanımı sağlar:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },
    "enableSkipIncompatibleRow": true,
    "redirectIncompatibleRowSettings": {
         "linkedServiceName": {
              "referenceName": "<Azure Storage or Data Lake Store linked service>",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | -------- 
etkinleştirinSkipIncompatibleRow | Kopya sırasında uyumsuz satırları atlayıp atlamayacağımı belirtir. | True<br/>False (varsayılan) | Hayır
yeniden YönlendirmeUyumsuzSatır Ayarları | Uyumsuz satırları günlüğe kaydetmek istediğinizde belirtilebilen özellikler grubu. | &nbsp; | Hayır
linkedServiceName | Atlanan satırları içeren günlüğü depolamak için [Azure Depolama](connector-azure-blob-storage.md#linked-service-properties) veya Azure Veri [Gölü Deposu'nun](connector-azure-data-lake-store.md#linked-service-properties) bağlantılı hizmeti. | Günlük dosyasını `AzureStorage` `AzureDataLakeStore` depolamak için kullanmak istediğiniz örneğe başvuran bir veya türbağlantılı hizmetin adı. | Hayır
yol | Atlanan satırları içeren günlük dosyasının yolu. | Uyumsuz verileri günlüğe kaydetmek için kullanmak istediğiniz yolu belirtin. Bir yol sağlamazsanız, hizmet sizin için bir kapsayıcı oluşturur. | Hayır

## <a name="monitor-skipped-rows"></a>Atlanan satırları izleme
Kopyalama etkinliği tamamlandıktan sonra, kopya etkinliği çıktısında atlanan satır sayısını görebilirsiniz:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Uyumsuz satırları günlüğe kaydetmek için yapılandırAcak şekilde yapılandırAcak `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`şekilde yapılandıracak şekilde, günlük dosyasını bu yolda bulabilirsiniz: . 

Günlük dosyaları sadece csv dosyaları olabilir. Atlanan orijinal veriler gerekirse sütun sınırlayıcı olarak virgülle günlüğe kaydedilir. Uyumsuzluğun temel nedenini görebileceğiniz günlük dosyasındaki orijinal kaynak verilere ek olarak iki sütun daha "ErrorCode" ve "ErrorMessage" ekliyoruz. ErrorCode ve ErrorMessage çift tırnak ile alıntı olacaktır. 

Günlük dosyası içeriğinin bir örneği aşağıdaki gibidir:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Sonraki adımlar
Diğer Kopyalama Etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Etkinlik performansını kopyalama](copy-activity-performance.md)


