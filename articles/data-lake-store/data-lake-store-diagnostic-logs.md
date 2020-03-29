---
title: Azure Veri Gölü Depolama Gen1 için tanı günlüklerini görüntüleme | Microsoft Dokümanlar
description: 'Azure Veri Gölü Depolama Gen1 için tanılama günlüklerinin nasıl kurulup erişileceğini anlama '
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60878752"
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1 için tanılama günlüklerine erişim
Azure Veri Gölü Depolama Gen1 hesabınız için tanısal günlüğe kaydetmeyi ve hesabınız için toplanan günlükleri nasıl görüntüleyebilirsiniz öğrenin.

Kuruluşlar, verilere erişen kullanıcıların listesi, verilere ne sıklıkta erişildi, hesapta ne kadar veri depolandığı vb. gibi bilgiler sağlayan veri erişim denetim izlerini toplamak için Azure Veri Gölü Depolama Gen1 hesapları için tanılama günlüğe kaydetmeyi etkinleştirebilir. Etkinleştirildiğinde, tanılama ve/veya istekler en iyi çaba temelinde günlüğe kaydedilir. Hem İstekler hem de Tanılama günlük girişleri yalnızca hizmet bitiş noktasına karşı yapılan istekler varsa oluşturulur.

## <a name="prerequisites"></a>Ön koşullar
* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Veri Gölü Depolama Gen1 hesabı.** [Azure Portalını kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-portal.md)yönergeleri izleyin.

## <a name="enable-diagnostic-logging-for-your-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabınız için tanılama günlüğe kaydetmeyi etkinleştirin
1. Yeni [Azure portalında](https://portal.azure.com)oturum açın.
2. Veri Gölü Depolama Gen1 hesabınızı açın ve Data Lake Storage Gen1 hesap çubuğunuzdan **Tanılama ayarlarını**tıklatın.
3. **Tanılama ayarları** bıçak, **tanılama aç'ı**tıklatın.

    ![Tanılama günlüğünü etkinleştirme](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "Tanılama günlüklerini etkinleştirme")

3. **Tanılama ayarları** bıçak, tanılama günlüğe yapılandırmak için aşağıdaki değişiklikleri yapın.
   
    ![Tanılama günlüğünü etkinleştirme](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Tanılama günlüklerini etkinleştirme")
   
   * **Ad**için, tanılama günlüğü yapılandırması için bir değer girin.
   * Verileri farklı şekillerde depolamayı/işlemeyi seçebilirsiniz.
     
        * Günlükleri bir Azure Depolama hesabında depolamak için **depolama hesabına Arşivleme** seçeneğini seçin. Toplu işlenecek verileri daha sonraki bir tarihte arşivlemek istiyorsanız bu seçeneği kullanırsınız. Bu seçeneği belirlerseniz, günlükleri kaydetmek için bir Azure Depolama hesabı sağlamanız gerekir.
        
        * Günlük verilerini **bir** Azure Etkinlik Hub'ına aktarmak için bir olay merkezine Akış seçeneğini seçin. Gelen günlükleri gerçek zamanlı olarak çözümlemek için bir akış aşağı işleme ardışık hattınız varsa, büyük olasılıkla bu seçeneği kullanırsınız. Bu seçeneği belirlerseniz, kullanmak istediğiniz Azure Etkinlik Hub'ı için ayrıntıları sağlamanız gerekir.

        * Oluşturulan günlük verilerini analiz etmek için Azure Monitor hizmetini kullanmak için **Günlük Analitiğine Gönder** seçeneğini seçin. Bu seçeneği belirlerseniz, günlük çözümlemesini kullanacağınız Log Analytics çalışma alanının ayrıntılarını sağlamanız gerekir. Bkz. [Azure Monitor günlükleri ile toplanan verileri Görüntüle veya analiz edin,](../azure-monitor/learn/tutorial-viewdata.md) Azure Monitor günlüklerini kullanmayla ilgili ayrıntıları arayın.
     
   * Denetim günlükleri almak mı yoksa istek günlükleri mi yoksa her ikisini birden mi almak istediğinizi belirtin.
   * Verilerin korunması gereken gün sayısını belirtin. Bekletme yalnızca günlük verilerini arşivlemek için Azure depolama hesabını kullanıyorsanız uygulanabilir.
   * **Kaydet**'e tıklayın.

Tanılama ayarlarını etkinleştirdikten sonra, **Tanılama Günlükleri** sekmesindeki günlükleri izleyebilirsiniz.

## <a name="view-diagnostic-logs-for-your-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabınız için tanılama günlüklerini görüntüleme
Veri Gölü Depolama Gen1 hesabınız için günlük verilerini görüntülemenin iki yolu vardır.

* Data Lake Storage Gen1 hesap ayarlarından görünüm
* Verilerin depolandığı Azure Depolama hesabından

### <a name="using-the-data-lake-storage-gen1-settings-view"></a>Veri Gölü Depolama Gen1 Ayarları görünümünü kullanma
1. Veri Gölü Depolama Gen1 hesap **Ayarları** nızdan **Tanı Günlükleri'ni**tıklatın.
   
    ![Tanılama günlüklerini görüntüleme](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Tanılama günlüklerini görüntüleme") 
2. **Tanılama Günlükleri** çubuğunda, Denetim Günlükleri ve **İstek Günlükleri**tarafından kategorize edilen **günlükleri** görmeniz gerekir.
   
   * İstek günlükleri, Veri Gölü Depolama Gen1 hesabında yapılan tüm API isteğini yakalar.
   * Denetim Günlükleri, İstek Günlükleri'ne benzer, ancak Veri Gölü Depolama Gen1 hesabında gerçekleştirilen işlemlerin çok daha ayrıntılı bir dökümünü sağlar. Örneğin, istek günlüklerinde tek bir yükleme API çağrısı, denetim günlüklerinde birden çok "Ek" işlemiyle sonuçlanabilir.
3. Günlükleri indirmek için, her günlük girişine karşı **İndir** bağlantısını tıklatın.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Günlük verilerini içeren Azure Depolama hesabından
1. Günlüğe kaydetmek için Veri Gölü Depolama Gen1 ile ilişkili Azure Depolama hesap bıçağını açın ve ardından Blobs'u tıklatın. **Blob servis** bıçağı iki kapsayıcıyı listeler.
   
    ![Tanıgünlüğü görüntüleme](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Tanılama günlüklerini görüntüleme")
   
   * Kapsayıcı **öngörüleri-günlükleri-denetimi** denetim günlüklerini içerir.
   * Kapsayıcı **öngörüler-günlükleri-istekleri** istek günlükleri içerir.
2. Bu kapların içinde, günlükler aşağıdaki yapı altında depolanır.
   
    ![Tanıgünlüğü görüntüleme](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Tanılama günlüklerini görüntüleme")
   
    Örnek olarak, denetim günlüğüne giden tam yol`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Benzer şekilde, istek günlüğüne giden tam yol`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestorage/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Günlük verilerinin yapısını anlama
Denetim ve istek günlükleri JSON biçimindedir. Bu bölümde, istek ve denetim günlükleri için JSON yapısına bakAcağız.

### <a name="request-logs"></a>İstek günlükleri
JSON biçimlendirilmiş istek günlüğündeki örnek giriş burada. Her blob günlük nesneleri bir dizi içeren **kayıtları** denilen bir kök nesne vardır.

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
| time |Dize |Günlüğün zaman damgası (UTC'de) |
| resourceId |Dize |İşlemin gerçekleştiği kaynağın kimliği |
| category |Dize |Günlük kategorisi. Örneğin, **İstekler**. |
| operationName |Dize |Günlüğe kaydedilen işlemin adı. Örneğin, getfilestatus. |
| resultType |Dize |Örneğin, operasyonun durumu 200. |
| callerIpAddress |Dize |İstekte bulunduran müşterinin IP adresi |
| correlationId |Dize |İlgili günlük girişlerini gruplandırmak için kullanılabilen günlüğün kimliği |
| identity |Nesne |Günlüğü oluşturan kimlik |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="request-log-properties-schema"></a>İstek günlük özellikleri şema
| Adı | Tür | Açıklama |
| --- | --- | --- |
| Http Yöntemi |Dize |İşlem için kullanılan HTTP Yöntemi. Örneğin, GET. |
| Yol |Dize |İşlemin gerçekleştirildiği yol |
| İstekİçerik Uzunluğu |int |HTTP isteğinin içerik uzunluğu |
| MüşteriİstekId |Dize |Bu isteği benzersiz olarak tanımlayan kimlik |
| StartTime |Dize |Sunucunun isteği aldığı saat |
| EndTime |Dize |Sunucunun yanıt gönderdiği saat |

### <a name="audit-logs"></a>Denetim günlükleri
JSON biçimlendirilmiş denetim günlüğündeki örnek giriş burada. Her blob günlük nesneleri bir dizi içeren **kayıtları** denilen bir kök nesnesi vardır

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
| time |Dize |Günlüğün zaman damgası (UTC'de) |
| resourceId |Dize |İşlemin gerçekleştiği kaynağın kimliği |
| category |Dize |Günlük kategorisi. Örneğin, **Denetim**. |
| operationName |Dize |Günlüğe kaydedilen işlemin adı. Örneğin, getfilestatus. |
| resultType |Dize |Örneğin, operasyonun durumu 200. |
| resultSignature |Dize |İşlemle ilgili ek ayrıntılar. |
| correlationId |Dize |İlgili günlük girişlerini gruplandırmak için kullanılabilen günlüğün kimliği |
| identity |Nesne |Günlüğü oluşturan kimlik |
| properties |JSON |Ayrıntılar için aşağıya bakın |

#### <a name="audit-log-properties-schema"></a>Denetim günlüğü özellikleri şema
| Adı | Tür | Açıklama |
| --- | --- | --- |
| Akış Adı |Dize |İşlemin gerçekleştirildiği yol |

## <a name="samples-to-process-the-log-data"></a>Günlük verilerini işlemek için örnekler
Azure Veri Gölü Deposu Gen1'den Azure Monitor günlüklerine günlükler gönderirken (bkz. Azure [Monitor günlüklerini](../azure-monitor/learn/tutorial-viewdata.md) kullanma yla ilgili ayrıntıları arayın, aşağıdaki sorgu, kullanıcı ekran adlarının listesini, olayların saatini ve görsel bir grafikle birlikte olay zamanı için olay sayısını içeren bir tabloyu döndürür. Kullanıcı GUID veya diğer öznitelikleri göstermek için kolayca değiştirilebilir:

```
search *
| where ( Type == "AzureDiagnostics" )
| summarize count(TimeGenerated) by identity_s, TimeGenerated
```


Azure Veri Gölü Depolama Gen1, günlük verilerinin nasıl işlendiği ve analiz edileceklerine ilişkin bir örnek sağlar. Örneği [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

