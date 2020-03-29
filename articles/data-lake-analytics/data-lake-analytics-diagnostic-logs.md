---
title: Azure Veri Gölü Analitiği için tanılama günlüklerini etkinleştirme ve görüntüleme
description: Azure Veri Gölü Analitiği için tanılama günlüklerinin nasıl ayarlanıp eriştiğinizi anlama
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.topic: conceptual
ms.date: 02/12/2018
ms.openlocfilehash: 7fd88383e909ebd6be64c22721b813946e37179e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60616526"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics’te tanılama günlüklerine erişim

Tanılama günlüğü, veri erişimi denetim yollarını toplamanıza olanak tanır. Bu günlükler gibi bilgiler sağlar:

* Verilere erişen kullanıcıların listesi.
* Verilere ne sıklıkta erişilir.
* Hesapta ne kadar veri depolanır.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Veri Gölü Analizi hesabınızı açın ve __Monitör__ bölümünden **Tanılama günlüklerini** seçin. Ardından, __tanılamayı aç'ı__seçin.

    ![Denetim ve istek günlüklerini toplamak için tanılamayı açın](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. __Tanılama ayarlarından,__ bu günlük yapılandırması için bir __Ad__ girin ve ardından günlüğe kaydetme seçeneklerini seçin.

    ![Denetim ve istek günlüklerini toplamak için tanılamayı açın](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Tanılama günlüklerini etkinleştirme")

   * Verileri üç farklı şekilde depolamayı/işlemeyi seçebilirsiniz.

     * Günlükleri bir Azure depolama hesabında depolamak için __depolama hesabına Arşiv'i__ seçin. Verileri arşivlemek istiyorsanız bu seçeneği kullanın. Bu seçeneği belirlerseniz, günlükleri kaydetmek için bir Azure depolama hesabı sağlamanız gerekir.

     * Günlük verilerini bir Azure Etkinlik Hub'ına aktarmak için Etkinlik Hub'ına **Akış'ı** seçin. Gelen günlükleri gerçek zamanlı olarak çözümleyen bir akış aşağı işleme ardışık hattınız varsa bu seçeneği kullanın. Bu seçeneği belirlerseniz, kullanmak istediğiniz Azure Etkinlik Hub'ı için ayrıntıları sağlamanız gerekir.

     * Verileri Azure Monitor hizmetine göndermek için __Günlük Analizi'ne Gönder'i__ seçin. Günlükleri toplamak ve analiz etmek için Azure Monitor günlüklerini kullanmak istiyorsanız bu seçeneği kullanın.
   * Denetim günlükleri almak mı yoksa istek günlükleri mi yoksa her ikisini birden mi almak istediğinizi belirtin.  İstek günlüğü her API isteğini yakalar. Denetim günlüğü, bu API isteği tarafından tetiklenen tüm işlemleri kaydeder.

   * __Arşiv'den depolama hesabına,__ verileri korumak için gün sayısını belirtin.

   * __Kaydet__'e tıklayın.

        > [!NOTE]
        > __Kaydet__ düğmesini tıklatmadan önce __bir depolama hesabına Arşivle,__ __Olay Hub'ına Akış__ veya Günlük __Analitiğine Gönder'i__ seçmeniz gerekir.

### <a name="use-the-azure-storage-account-that-contains-log-data"></a>Günlük verilerini içeren Azure Depolama hesabını kullanma

1. Günlüğe kaydetme verilerini tutan blob kapsayıcılarını görüntülemek için, günlüğe kaydetme için Veri Gölü Analitiği için kullanılan Azure Depolama hesabını açın ve ardından __Blobs'ı__tıklatın.

   * Kapsayıcı **öngörüleri-günlükleri-denetimi** denetim günlüklerini içerir.
   * Kapsayıcı **öngörüler-günlükleri-istekleri** istek günlükleri içerir.

2. Kapsayıcılar içinde, günlükleri aşağıdaki dosya yapısı altında saklanır:

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > Yoldaki girişler `##` günlüğün oluşturulduğu yılı, ayı, günü ve saati içerir. Data Lake Analytics her saat başı `m=` bir dosya `00`oluşturur, bu nedenle her zaman bir değer içerir.

    Örnek olarak, denetim günlüğüne giden tam yol şu olabilir:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Benzer şekilde, istek günlüğüne giden tam yol şu şekilde olabilir:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Günlük yapısı

Denetim ve istek günlükleri yapılandırılmış json biçimindedir.

### <a name="request-logs"></a>İstek günlükleri

JSON biçimlendirilmiş istek günlüğündeki örnek giriş burada. Her blob günlük nesneleri bir dizi içeren **kayıtları** denilen bir kök nesne vardır.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>İstek günlüğü şeması

| Adı | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Günlüğün zaman damgası (UTC'de) |
| resourceId |Dize |İşlemin gerçekleştiği kaynağın tanımlayıcısı |
| category |Dize |Günlük kategorisi. Örneğin, **İstekler**. |
| operationName |Dize |Günlüğe kaydedilen işlemin adı. Örneğin, GetAggregatedJobHistory. |
| resultType |Dize |Örneğin, operasyonun durumu 200. |
| callerIpAddress |Dize |İstekte bulunduran müşterinin IP adresi |
| correlationId |Dize |Günlüğün tanımlayıcısı. Bu değer, ilgili günlük girişleri kümesini gruplandırmak için kullanılabilir. |
| identity |Nesne |Günlüğü oluşturan kimlik |
| properties |JSON |Ayrıntılar için sonraki bölüme bakın (İstek günlük özellikleri şeması) |

#### <a name="request-log-properties-schema"></a>İstek günlük özellikleri şema

| Adı | Tür | Açıklama |
| --- | --- | --- |
| Http Yöntemi |Dize |İşlem için kullanılan HTTP Yöntemi. Örneğin, GET. |
| Yol |Dize |İşlemin gerçekleştirildiği yol |
| İstekİçerik Uzunluğu |int |HTTP isteğinin içerik uzunluğu |
| MüşteriİstekId |Dize |Bu isteği benzersiz olarak tanımlayan tanımlayıcı |
| StartTime |Dize |Sunucunun isteği aldığı saat |
| EndTime |Dize |Sunucunun yanıt gönderdiği saat |

### <a name="audit-logs"></a>Denetim günlükleri

JSON biçimlendirilmiş denetim günlüğündeki örnek giriş burada. Her blob günlük nesneleri bir dizi içeren **kayıtları** denilen bir kök nesne vardır.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Denetim günlüğü şeması

| Adı | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Günlüğün zaman damgası (UTC'de) |
| resourceId |Dize |İşlemin gerçekleştiği kaynağın tanımlayıcısı |
| category |Dize |Günlük kategorisi. Örneğin, **Denetim**. |
| operationName |Dize |Günlüğe kaydedilen işlemin adı. Örneğin, JobSubmitted. |
| resultType |Dize |İş durumu (operationName) için bir alt durum. |
| resultSignature |Dize |İş durumu (operationName) hakkında ek ayrıntılar. |
| identity |Dize |İşlemi isteyen kullanıcı. Örneğin, susan@contoso.com. |
| properties |JSON |Ayrıntılar için sonraki bölüme bakın (Denetim günlüğü özellikleri şeması) |

> [!NOTE]
> **resultType** ve **resultSignature** bir işlemin sonucu hakkında bilgi sağlar ve yalnızca bir işlem tamamlandığında bir değer içerir. Örneğin, yalnızca **operationName** **JobStarted** veya **JobEnded'in**bir değerini içerdiğinde bir değer içerirler.
>
>

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şema

| Adı | Tür | Açıklama |
| --- | --- | --- |
| JobId |Dize |İşe atanan kimlik |
| İş Adı |Dize |İş için sağlanan ad |
| JobRunTime |Dize |İşi işlemek için kullanılan çalışma süresi |
| Gönderme Süresi |Dize |İşin gönderildiği saat (UTC'de) |
| StartTime |Dize |Teslimden sonra işin çalışmaya başladığı saat (UTC'de) |
| EndTime |Dize |İşin sona erme zamanı |
| Parallelism |Dize |Gönderim sırasında bu iş için istenen Data Lake Analytics birimlerinin sayısı |

> [!NOTE]
> **SubmitTime,** **StartTime,** **EndTime**ve **Parallelism** bir işlem hakkında bilgi sağlar. Bu girişler yalnızca bu işlem başlatıldığında veya tamamlandıysa bir değer içerir. Örneğin, **SubmitTime** yalnızca **operationName'den** sonra bir değer **içerir.**

## <a name="process-the-log-data"></a>Günlük verilerini işleme

Azure Veri Gölü Analizi, günlük verilerinin nasıl işlenir ve analiz edilemeye ilişkin bir örnek sağlar. Örneği [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Analytics’e Genel Bakış](data-lake-analytics-overview.md)
