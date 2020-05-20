---
title: Kopyalama etkinliğinde veri tutarlılığı doğrulaması
description: Azure Data Factory ' de kopyalama etkinliğinde veri tutarlılığı doğrulamanın nasıl etkinleştirileceği hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: ''
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 3/27/2020
ms.author: yexu
ms.openlocfilehash: a386c7d44cf5ba7eda895006cda7ce1fa9b798ac
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83664980"
---
#  <a name="data-consistency-verification-in-copy-activity-preview"></a>Kopyalama etkinliğinde veri tutarlılığı doğrulama (Önizleme)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verileri kaynaktan hedef depoya taşıdığınızda, Azure Data Factory kopyalama etkinliği, verilerin kaynaktan yalnızca kaynak ve hedef depo arasında tutarlı bir şekilde kopyalanmadığından emin olmak için ek veri tutarlılığı doğrulaması yapmanız için bir seçenek sunar. Veri taşıma sırasında tutarsız veriler bulunduktan sonra, kopyalama etkinliğini durdurabilir veya tutarsız verileri atlamak için hata toleransı ayarını etkinleştirerek geri kalanı kopyalamaya devam edebilirsiniz. Kopyalama etkinliğinde oturum günlüğü ayarını etkinleştirerek atlanan nesne adlarını alabilirsiniz. 

> [!IMPORTANT]
> Bu özellik şu anda önizleme aşamasındadır ve üzerinde etkin olarak çalıştığımız şu sınırlamalara sahiptir:
>- Veri tutarlılığı doğrulaması yalnızca kopyalama etkinliğinde ' PreserveHierarchy ' davranışına sahip dosya tabanlı depolar arasında kopyalanan ikili dosyalarda kullanılabilir. Tablo verilerini kopyalamak için, veri tutarlılığı doğrulaması henüz kopyalama etkinliğinde kullanılamaz.
>- Kopyalama etkinliğinde oturum günlüğü ayarını, Atlanan dosyaları günlüğe kaydetmek üzere etkinleştirdiğinizde, kopyalama etkinliği başarısız olursa günlük dosyasının bütünlüğü %100 olamaz.
>- Oturum günlüğü, başarıyla kopyalanan dosyaların şu ana kadar günlüğe kaydedildiği, yalnızca tutarsız dosyalar içeriyor.

## <a name="supported-data-stores"></a>Desteklenen veri depoları

### <a name="source-data-stores"></a>Kaynak veri depoları

-   [Azure Blob depolama](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md)
-   [Azure Dosya Depolama](connector-azure-file-storage.md)
-   [Amazon S3](connector-amazon-simple-storage-service.md)
-   [Dosya sistemi](connector-file-system.md)
-   [HDFS](connector-hdfs.md)

### <a name="destination-data-stores"></a>Hedef veri depoları

-   [Azure Blob depolama](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md)
-   [Azure Dosya Depolama](connector-azure-file-storage.md)
-   [Dosya sistemi](connector-file-system.md)


## <a name="configuration"></a>Yapılandırma
Aşağıdaki örnek, kopyalama etkinliğinde veri tutarlılığı doğrulamasını etkinleştirmek için bir JSON tanımı sağlar: 

```json
"typeProperties": { 
"source": { 
        "type": "BinarySource", 
        "storeSettings": { 
            "type": "AzureDataLakeStoreReadSettings", 
            "recursive": true 
        } 
    }, 
    "sink": { 
        "type": "BinarySink", 
        "storeSettings": { 
            "type": "AzureDataLakeStoreWriteSettings" 
        } 
}, 
    "validateDataConsistency": true, 
    "skipErrorFile": { 
        "dataInconsistency": true 
    }, 
    "logStorageSettings": { 
        "linkedServiceName": { 
            "referenceName": "ADLSGen2_storage", 
            "type": "LinkedServiceReference" 
        }, 
        "path": "/sessionlog/" 
} 
} 
```

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | -------- 
Validatedatatutarlılığı | Bu özellik için true ayarlarsanız, kopyalama etkinliği kaynak ve hedef depo arasında veri tutarlılığı sağlamak için kaynaktan hedef depoya kopyalanmış her bir nesne için dosya boyutunu, lastModifiedDate ve MD5 sağlama toplamını denetler. Bu seçeneği etkinleştirerek kopyalama performansının etkileneceğini unutmayın.  | True<br/>False (varsayılan) | Hayır
Veri tutarsızlığı | Tutarsız verileri atlamak istediğinizi öğrenmek için skipErrorFile özellik paketi içindeki anahtar-değer çiftlerinden biri.<br/> -True: tutarsız verileri atlayarak geri kalanı kopyalamak istiyorsunuz.<br/> -False: tutarsız veriler bulunduğunda kopyalama etkinliğini iptal etmek istiyorsunuz.<br/>Bu özelliğin yalnızca Validatedatatutarlılığı doğru olarak ayarladığınızda geçerli olduğunu unutmayın.  | True<br/>False (varsayılan) | Hayır
logStorageSettings | Atlanan nesneleri günlüğe kaydetmek için oturum günlüğünü etkinleştirmek üzere belirtilenebilir bir özellik grubu. | | Hayır
linkedServiceName | [Azure Blob depolama alanına](connector-azure-blob-storage.md#linked-service-properties) bağlı hizmet veya oturum günlüğü dosyalarını depolamak için [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md#linked-service-properties) . | `AzureBlobStorage` `AzureBlobFS` Günlük dosyalarını depolamak için kullandığınız örneğe başvuran bağlı hizmetin veya türlerin adları. | Hayır
yol | Günlük dosyalarının yolu. | Günlük dosyalarını depolamak istediğiniz yolu belirtin. Bir yol sağlamazsanız, hizmet sizin için bir kapsayıcı oluşturur. | Hayır

>[!NOTE]
>- Hazırlama kopyalama senaryosunda veri tutarlılığı desteklenmez. 
>- Herhangi bir depolama deposundan ikili dosyaları Azure Blob depolama veya Azure Data Lake Storage 2. kopyalama sırasında, kaynak ve hedef mağazalar arasında veri tutarlılığı sağlamak için kopyalama etkinliği dosya boyutu ve MD5 sağlama toplamı doğrulaması yapar. 
>- Herhangi bir depolama deposundan ikili dosyaları Azure Blob depolama veya Azure Data Lake Storage 2. dışındaki depolama depolarına kopyalarken, kaynak ve hedef depo arasında veri tutarlılığı sağlamak için kopyalama etkinliği dosya boyutu doğrulaması yapar.


## <a name="monitoring"></a>İzleme

### <a name="output-from-copy-activity"></a>Kopyalama etkinliğinden çıkış
Kopyalama etkinliği tamamen çalıştıktan sonra, her bir kopyalama etkinliği çalışmasının çıktısından veri tutarlılığı doğrulamasının sonucunu görebilirsiniz:

```json
"output": {
            "dataRead": 695,
            "dataWritten": 186,
            "filesRead": 3,  
            "filesWritten": 1, 
            "filesSkipped": 2, 
            "throughput": 297,
            "logPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "dataConsistencyVerification": 
           { 
                "VerificationResult": "Verified", 
                "InconsistentData": "Skipped" 
           } 
        }

```
"Dataımst doğrulama özelliği" kaynağından veri tutarlılığı doğrulama ayrıntılarını görebilirsiniz.

**Doğrulamaları Icationresult**değeri: 
-   **Doğrulandı**: kopyalanmış verilerinizin kaynak ve hedef depo arasında tutarlı olması doğrulandı. 
-   **Notdoğrulandı**: kopyalama etkinliğinde Validatedatatutarlılığını etkinleştirmediyseniz, kopyalanmış verileriniz tutarlı olarak doğrulanmadı. 
-   **Desteklenmiyor**: Bu belirli bir kopya çifti için veri tutarlılığı doğrulaması desteklenmediğinden, kopyalanmış verileriniz tutarlı olarak doğrulanmadı. 

**Inreferencetentdata**değeri: 
-   **Bulundu**: ADF kopyalama etkinliği tutarsız veriler buldu. 
-   **Atlandı**: ADF kopyalama etkinliği tutarsız veriler buldu ve atladı. 
-   **Hiçbiri**: ADF kopyalama etkinliği tutarsız veriler bulamadı. Verilerinizin kaynak ve hedef depo arasında tutarlı olarak doğrulanması veya kopyalama etkinliğinde Validatedatatutarlılığı devre dışı bırakılması nedeniyle olabilir. 

### <a name="session-log-from-copy-activity"></a>Kopyalama etkinliğinden oturum günlüğü

Tutarsız dosyayı günlüğe kaydetmek üzere yapılandırırsanız, günlük dosyasını şu yoldan bulabilirsiniz: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/copyactivity-logs/[copy-activity-name]/[copy-activity-run-id]/[auto-generated-GUID].csv` .  Günlük dosyaları CSV dosyaları olacaktır. 

Bir günlük dosyasının şeması aşağıdaki gibidir:

Sütun | Açıklama 
-------- | -----------  
Zaman damgası | ADF tutarsız dosyaları atlamazsa zaman damgası.
Düzey | Bu öğenin günlük düzeyi. Dosya atlama gösteren öğe için ' uyarı ' düzeyinde olacaktır.
ThrottledRequests | ADF kopyalama etkinliği işlem davranışı her dosyada. Atlanacak dosyayı belirtmek için ' FileSkip ' olur.
Operationıtem | Atlanacak dosya adı.
İleti | Dosyaların neden atlandığını gösteren daha fazla bilgi.

Bir günlük dosyası örneği aşağıdaki gibidir: 
```
Timestamp, Level, OperationName, OperationItem, Message
2020-02-26 06:22:56.3190846, Warning, FileSkip, "sample1.csv", "File is skipped after read 548000000 bytes: ErrorCode=DataConsistencySourceDataChanged,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Source file 'sample1.csv' is changed by other clients during the copy activity run.,Source=,'." 
```
Yukarıdaki günlük dosyasından, kaynak ve hedef depo arasında tutarlı bir şekilde doğrulanamadığı için SAMPLE1. csv dosyasının atlandığını görebilirsiniz. SAMPLE1. csv ' nin tutarsız hale geldiği hakkında daha fazla ayrıntı için, ADF kopyalama etkinliği aynı anda kopyalanırken diğer uygulamalar tarafından değiştirilmekte. 



## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği hata toleransı](copy-activity-fault-tolerance.md)


