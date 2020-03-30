---
title: Tanılama günlüklerini kullanarak Azure Veri Gezgini işlemlerine izleme
description: Azure Veri Gezgini için oluşturma işlemlerini izlemek için tanı lama günlüklerini nasıl ayarlayabilirsiniz öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 3e10979e26cacdc0c2071a6030c945adad21a51c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277419"
---
# <a name="monitor-azure-data-explorer-ingestion-operations-using-diagnostic-logs-preview"></a>Tanılama günlüklerini kullanarak Azure Veri Gezgini işlemlerini izleme (Önizleme)

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından, sorguları veritabanında bir tabloya veri yükler (yüklersiniz) böylece buna karşı sorguları çalıştırabilirsiniz. [Azure Monitor tanı günlükleri,](/azure/azure-monitor/platform/diagnostic-logs-overview) Azure kaynaklarının çalışması hakkında veri sağlar. Azure Veri Gezgini, yutma başarıları ve hataları hakkında öngörüler için tanılama günlüklerini kullanır. İşlem durumunu izlemek için işlem günlüklerini Azure Depolama, Etkinlik Hub'ı veya Log Analytics'e dışa aktarabilirsiniz. Azure Depolama ve Azure Etkinlik Hub'ından günlükler, daha fazla analiz için Azure Veri Gezgini kümenizdeki bir tabloya yönlendirilebilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa, ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/)oluşturun.
* Bir [küme ve veritabanı](create-cluster-database-portal.md)oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="set-up-diagnostic-logs-for-an-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi için tanıgünlükleri ayarlama

Tanılama günlükleri aşağıdaki günlük verilerinin toplanmasını yapılandırmak için kullanılabilir:
* Başarılı yutma işlemleri: Bu günlükler başarıyla tamamlanan yutma işlemleri hakkında bilgi sahibidir.
* Başarısız yutma işlemleri: Bu günlüklerde hata ayrıntıları da dahil olmak üzere başarısız yutma işlemleri hakkında ayrıntılı bilgiler vardır. 

Veriler daha sonra bir Depolama hesabına arşivlenir, bir Etkinlik Hub'ına aktarılır veya belirtimlerinize göre Log Analytics'e gönderilir.

### <a name="enable-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme

Tanılama günlükleri varsayılan olarak devre dışı bırakılır. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları yapın:

1. Azure [portalında,](https://portal.azure.com)izlemek istediğiniz Azure Veri Gezgini küme kaynağını seçin.
1. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.
  
    ![Tanılama günlükleri ekleme](media/using-diagnostic-logs/add-diagnostic-logs.png)

1. **Tanıayar ayarı ekle'yi**seçin.
1. **Tanılama ayarları** penceresinde:
 
    ![Tanılama ayarları yapılandırması](media/using-diagnostic-logs/configure-diagnostics-settings.png) 

    1. Tanılama ayarınız için **Ad'ı** seçin.
    1. Bir veya daha fazla hedef seçin: Bir Depolama hesabı, Olay Merkezi veya Günlük Analizi.
    1. Toplanacak günlükleri seçin: `SucceededIngestion` `FailedIngestion`veya .
    1. Toplanacak [ölçümleri](using-metrics.md#supported-azure-data-explorer-metrics) seçin (isteğe bağlı).  
    1. Yeni tanılama günlükleri ayarlarını ve ölçümlerini kaydetmek için **Kaydet'i** seçin.
    1. Tanılama günlüklerinin etkinleştirmesini istemek için Azure portalında yeni bir **destek isteği** oluşturun.

Yeni ayarlar birkaç dakika içinde ayarlanır. Günlükler daha sonra yapılandırılan arşiv hedefinde (Depolama hesabı, Olay Hub'ı veya Log Analytics) görünür. 

## <a name="diagnostic-logs-schema"></a>Tanılama günlükleri şema

Tüm [Azure Monitor tanılama günlükleri ortak bir üst düzey şema paylaşır.](/azure/azure-monitor/platform/diagnostic-logs-schema) Azure Veri Gezgini kendi etkinlikleri için benzersiz özelliklere sahiptir. Tüm günlükler JSON biçiminde depolanır.

### <a name="ingestion-logs-schema"></a>Yutma şema günlükleri

Log JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

|Adı               |Açıklama
|---                |---
|time               |Raporun zamanı
|resourceId         |Azure Kaynak Yöneticisi kaynak kimliği
|operationName      |İşlemin adı: 'MICROSOFT. KUSTO/KÜMELER/YUTMA/EYLEM'
|operationVersion   |Şema versiyonu: '1.0' 
|category           |İşlemin kategorisi. `SucceededIngestion` veya `FailedIngestion`. Özellikler [başarılı çalışma](#successful-ingestion-operation-log) veya [başarısız işlem](#failed-ingestion-operation-log)için farklılık gösterir.
|properties         |Operasyonun ayrıntılı bilgi.

#### <a name="successful-ingestion-operation-log"></a>Başarılı yutma işlemi günlüğü

**Örnek:**

```json
{
    "time": "",
    "resourceId": "",
    "operationName": "MICROSOFT.KUSTO/CLUSTERS/INGEST/ACTION",
    "operationVersion": "1.0",
    "category": "SucceededIngestion",
    "properties":
    {
        "succeededOn": "2019-05-27 07:55:05.3693628",
        "operationId": "b446c48f-6e2f-4884-b723-92eb6dc99cc9",
        "database": "Samples",
        "table": "StormEvents",
        "ingestionSourceId": "66a2959e-80de-4952-975d-b65072fc571d",
        "ingestionSourcePath": "https://kustoingestionlogs.blob.core.windows.net/sampledata/events8347293.json",
        "rootActivityId": "d0bd5dd3-c564-4647-953e-05670e22a81d"
    }
}
```
**Başarılı bir işlem tanı lama günlüğünün özellikleri**

|Adı               |Açıklama
|---                |---
|başarılıOn        |Yutma süresi nin tamamlanması
|operationId        |Azure Veri Gezgini alma işlemi kimliği
|database           |Hedef veritabanının adı
|tablo              |Hedef tablonun adı
|ingestionSourceId  |Yutma veri kaynağının kimliği
|yutmaKaynak Yolu|Yutma veri kaynağı veya blob URI yolu
|rootActivityId     |Etkinlik Kimliği

#### <a name="failed-ingestion-operation-log"></a>Başarısız alma işlemi günlüğü

**Örnek:**

```json
{
    "time": "",
    "resourceId": "",
    "operationName": "MICROSOFT.KUSTO/CLUSTERS/INGEST/ACTION",
    "operationVersion": "1.0",
    "category": "FailedIngestion",
    "properties":
    {
        "failedOn": "2019-05-27 08:57:05.4273524",
        "operationId": "5956515d-9a48-4544-a514-cf4656fe7f95",
        "database": "Samples",
        "table": "StormEvents",
        "ingestionSourceId": "eee56f8c-2211-4ea4-93a6-be556e853e5f",
        "ingestionSourcePath": "https://kustoingestionlogs.blob.core.windows.net/sampledata/events5725592.json",
        "rootActivityId": "52134905-947a-4231-afaf-13d9b7b184d5",
        "details": "Permanent failure downloading blob. URI: ..., permanentReason: Download_SourceNotFound, DownloadFailedException: 'Could not find file ...'",
        "errorCode": "Download_SourceNotFound",
        "failureStatus": "Permanent",
        "originatesFromUpdatePolicy": false,
        "shouldRetry": false
    }
}
```

**Başarısız bir işlem tanılama günlüğünün özellikleri**

|Adı               |Açıklama
|---                |---
|failedOn           |Yutma süresi nin tamamlanması
|operationId        |Azure Veri Gezgini alma işlemi kimliği
|database           |Hedef veritabanının adı
|tablo              |Hedef tablonun adı
|ingestionSourceId  |Yutma veri kaynağının kimliği
|yutmaKaynak Yolu|Yutma veri kaynağı veya blob URI yolu
|rootActivityId     |Etkinlik Kimliği
|Şey            |Hata ve hata iletisinin ayrıntılı açıklaması
|hataKodu          |Hata kodu 
|failureDurum      |`Permanent` veya `Transient`. Geçici bir hatanın yeniden denemesi başarılı olabilir.
|originatesFromUpdatePolicy|Hata bir güncelleştirme ilkesinden kaynaklanıyorsa true
|shouldRetry        |Retry başarılı olabilir eğer doğru

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure Veri Gezgini'nde izleme verilerini alma ve sorgulama](ingest-data-no-code.md)
* [Kümelerin sistem durumunu izlemek için ölçümleri kullanma](using-metrics.md)

