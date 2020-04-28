---
title: Azure Data Lake Storage 1. için tanılama günlükleri görüntüleniyor | Microsoft Docs
description: 'Azure Data Lake Storage 1. için tanılama günlüklerine nasıl kurulum ve erişme hakkında bilgi edinin '
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: d200f72b3c0e5634c3dca8f60a4754a14351110a
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60878752"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. için tanılama günlüklerine erişme
Azure Data Lake Storage 1. hesabınız için tanılama günlüğünü etkinleştirmeyi ve hesabınız için toplanan günlüklerin nasıl görüntüleneceğini öğrenin.

Kuruluşlar, verilere erişen kullanıcıların listesi, verilere ne sıklıkta erişildiğine, hesapta ne kadar veri depolandığını vb. gibi bilgiler sağlayan veri erişim denetimi izlerini toplamak üzere Azure Data Lake Storage 1. hesabı için tanılama günlüğünü etkinleştirebilir. Etkinleştirildiğinde, tanılama ve/veya istekler en iyi çaba temelinde günlüğe kaydedilir. Hem Istekler hem de tanılama günlüğü girdileri yalnızca hizmet uç noktasına karşı yapılan istekler varsa oluşturulur.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Storage 1. hesabı**. [Azure portalını kullanarak Azure Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md)konusundaki yönergeleri izleyin.

## <a name="enable-diagnostic-logging-for-your-data-lake-storage-gen1-account"></a>Data Lake Storage 1. hesabınız için tanılama günlüğünü etkinleştirme
1. Yeni [Azure Portal](https://portal.azure.com)oturum açın.
2. Data Lake Storage 1. hesabınızı açın ve Data Lake Storage 1. hesabı dikey penceresinde **Tanılama ayarları**' na tıklayın.
3. **Tanılama ayarları** dikey penceresinde **tanılamayı aç**' a tıklayın.

    ![Tanılama günlüğünü etkinleştirme](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Tanılama günlüklerini etkinleştirme")

3. Tanılama **ayarları** dikey penceresinde, tanılama günlüğünü yapılandırmak için aşağıdaki değişiklikleri yapın.
   
    ![Tanılama günlüğünü etkinleştirme](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Tanılama günlüklerini etkinleştirme")
   
   * **Ad**için, tanılama günlüğü yapılandırması için bir değer girin.
   * Verileri farklı yollarla depolamayı/işlemeyi tercih edebilirsiniz.
     
        * Günlükleri bir Azure depolama hesabına depolamak için **bir depolama hesabına arşivleme** seçeneğini belirleyin. Daha sonraki bir tarihte toplu olarak işlenecek verileri arşivlemek istiyorsanız bu seçeneği kullanın. Bu seçeneği belirlerseniz, günlüklerin kaydedileceği bir Azure depolama hesabı sağlamanız gerekir.
        
        * Günlük verilerinin bir Azure Olay Hub 'ına akışını sağlamak için **bir olay hub 'ına akış** seçeneğini belirleyin. Büyük olasılıkla, gelen günlükleri gerçek zamanlı olarak analiz etmek için bir aşağı akış işleme işlem hattına sahipseniz bu seçeneği kullanabilirsiniz. Bu seçeneği belirlerseniz, kullanmak istediğiniz Azure Olay Hub 'ının ayrıntılarını sağlamanız gerekir.

        * Oluşturulan günlük verilerini çözümlemek için Azure Izleyici hizmetini kullanmak üzere **Log Analytics gönderme** seçeneğini belirleyin. Bu seçeneği belirlerseniz, günlük analizini gerçekleştirmek için kullanacağınız Log Analytics çalışma alanının ayrıntılarını sağlamanız gerekir. Bkz. Azure izleyici [günlükleri ile toplanan verileri görüntüleme veya çözümleme](../azure-monitor/learn/tutorial-viewdata.md) Azure izleyici günlüklerini kullanma hakkında ayrıntılı bilgi için arama.
     
   * Denetim günlüklerini veya istek günlüklerini mi yoksa her ikisini mi almak istediğinizi belirtin.
   * Verilerin korunması gereken gün sayısını belirtin. Bekletme yalnızca günlük verilerini arşivlemek için Azure Storage hesabı kullanıyorsanız geçerlidir.
   * **Kaydet**’e tıklayın.

Tanılama ayarlarını etkinleştirdikten sonra, günlükleri **tanılama günlükleri** sekmesinde izleyebilirsiniz.

## <a name="view-diagnostic-logs-for-your-data-lake-storage-gen1-account"></a>Data Lake Storage 1. hesabınız için tanılama günlüklerini görüntüleyin
Data Lake Storage 1. hesabınızın günlük verilerini görüntülemenin iki yolu vardır.

* Data Lake Storage 1. hesap ayarları görünümünden
* Verilerin depolandığı Azure Storage hesabından

### <a name="using-the-data-lake-storage-gen1-settings-view"></a>Data Lake Storage 1. ayarları görünümünü kullanma
1. Data Lake Storage 1. hesap **ayarları** dikey penceresinde **tanılama günlükleri**' ne tıklayın.
   
    ![Tanılama günlüklerini görüntüle](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Tanılama günlüklerini görüntüleme") 
2. **Tanılama günlükleri** dikey penceresinde, günlükleri **Denetim günlüklerine** ve **istek günlüklerine**göre kategorilere ayırarak görmeniz gerekir.
   
   * İstek günlükleri Data Lake Storage 1. hesapta yapılan her API isteğini yakalar.
   * Denetim günlükleri, istek günlüklerine benzerdir ancak Data Lake Storage 1. hesapta gerçekleştirilen işlemlerin daha ayrıntılı bir dökümünü sağlar. Örneğin, istek günlüklerinde tek bir karşıya yükleme API çağrısı, denetim günlüklerinde birden çok "Append" işlemine neden olabilirler.
3. Günlükleri indirmek için her günlük girdisinde **karşıdan yükleme** bağlantısına tıklayın.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Günlük verilerini içeren Azure depolama hesabından
1. Günlük kaydı için Data Lake Storage 1. ilişkili Azure depolama hesabı dikey penceresini açın ve sonra Bloblar ' a tıklayın. **BLOB hizmeti** dikey penceresi iki kapsayıcıyı listeler.
   
    ![Tanılama günlüğünü görüntüle](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Tanılama günlüklerini görüntüleme")
   
   * Kapsayıcı **öngörüleri-Günlükler-denetim** denetim günlüklerini içerir.
   * Kapsayıcı **öngörüleri-Günlükler-istekler** istek günlüklerini içerir.
2. Bu kapsayıcılar içinde Günlükler aşağıdaki yapının altında depolanır.
   
    ![Tanılama günlüğünü görüntüle](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Tanılama günlüklerini görüntüleme")
   
    Örnek olarak, bir denetim günlüğüne yönelik yolun tamamı`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Benzer şekilde, bir istek günlüğüne yönelik yolun tamamı`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Günlük verilerinin yapısını anlama
Denetim ve istek günlükleri JSON biçimindedir. Bu bölümde, istek ve denetim günlükleri için JSON yapısına bakacağız.

### <a name="request-logs"></a>İstek günlükleri
JSON biçimli istek günlüğünde örnek bir giriş aşağıda verilmiştir. Her Blobun, bir dizi günlük nesnesi içeren **kayıtlar** adlı bir kök nesnesi vardır.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>İstek günlüğü şeması
| Adı | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Günlüğün zaman damgası (UTC) |
| resourceId |Dize |İşlemin gerçekleştiği kaynağın KIMLIĞI |
| category |Dize |Günlük kategorisi. Örneğin, **istekler**. |
| operationName |Dize |Günlüğe kaydedilen işlemin adı. Örneğin, getfilestatus. |
| resultType |Dize |İşlemin durumu, örneğin, 200. |
| callerIpAddress |Dize |İsteği yapan istemcinin IP adresi |
| correlationId |Dize |Bir dizi ilgili günlük girişi gruplamak için kullanılan günlüğün KIMLIĞI |
| identity |Nesne |Günlüğü oluşturan kimlik |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="request-log-properties-schema"></a>İstek günlüğü özellikleri şeması
| Adı | Tür | Açıklama |
| --- | --- | --- |
| HttpMethod |Dize |İşlem için kullanılan HTTP yöntemi. Örneğin, GET. |
| Yol |Dize |İşlemin gerçekleştirildiği yol |
| RequestContentLength |int |HTTP isteğinin içerik uzunluğu |
| Clientrequestıd 'ye sahip |Dize |Bu isteği benzersiz bir şekilde tanımlayan KIMLIK |
| StartTime |Dize |Sunucunun isteği aldığı zaman |
| EndTime |Dize |Sunucunun yanıt gönderdiği zaman |

### <a name="audit-logs"></a>Denetim günlükleri
JSON biçimli denetim günlüğünde örnek bir giriş aşağıda verilmiştir. Her Blobun, bir dizi günlük nesneleri içeren **kayıtlar** adlı bir kök nesnesi vardır

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_storage_gen1_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "resultSignature": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_storage_gen1_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Denetim günlüğü şeması
| Adı | Tür | Açıklama |
| --- | --- | --- |
| time |Dize |Günlüğün zaman damgası (UTC) |
| resourceId |Dize |İşlemin gerçekleştiği kaynağın KIMLIĞI |
| category |Dize |Günlük kategorisi. Örneğin, **Denetim**. |
| operationName |Dize |Günlüğe kaydedilen işlemin adı. Örneğin, getfilestatus. |
| resultType |Dize |İşlemin durumu, örneğin, 200. |
| resultSignature |Dize |İşlemle ilgili ek ayrıntılar. |
| correlationId |Dize |Bir dizi ilgili günlük girişi gruplamak için kullanılan günlüğün KIMLIĞI |
| identity |Nesne |Günlüğü oluşturan kimlik |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şeması
| Adı | Tür | Açıklama |
| --- | --- | --- |
| StreamName |Dize |İşlemin gerçekleştirildiği yol |

## <a name="samples-to-process-the-log-data"></a>Günlük verilerini işlemek için örnekler
Azure Data Lake Storage 1. Azure Izleyici günlüklerine Günlükler gönderirken (bkz. Azure izleyici günlükleri ile ilgili ayrıntılar için bkz. Azure izleyici günlükleri [ile toplanan verileri görüntüleme veya çözümleme](../azure-monitor/learn/tutorial-viewdata.md) ) aşağıdaki sorgu, Kullanıcı görünen adları, olayların zamanını ve bir görsel grafik ile olay zaman içindeki olayların sayısını içeren bir tablo döndürür. Kullanıcı GUID veya diğer öznitelikleri göstermek için kolayca değiştirilebilir:

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Azure Data Lake Storage 1. günlük verilerini işleme ve çözümleme hakkında bir örnek sağlar. Örneği adresinde [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)bulabilirsiniz. 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Storage 1. genel bakış](data-lake-store-overview.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

