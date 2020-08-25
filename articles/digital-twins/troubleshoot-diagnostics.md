---
title: Tanılamayı ayarla
titleSuffix: Azure Digital Twins
description: Bkz. tanılama ayarlarıyla günlüğü etkinleştirme.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: fc397b6d6beb719e11dc3959bbcf4d75c08a8dda
ms.sourcegitcommit: 5b6acff3d1d0603904929cc529ecbcfcde90d88b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88723937"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Azure dijital TWINS sorunlarını giderme: tanılama günlüğü

Azure dijital TWINS, kaynaklarınızın durumu hakkında bilgi veren hizmet örneğiniz için [ölçümleri](troubleshoot-metrics.md) toplar. Bu ölçümleri, Azure Digital TWINS hizmetinin genel sistem durumunu ve ona bağlı kaynakları değerlendirmek için kullanabilirsiniz. Bu kullanıcıya yönelik istatistikler, Azure dijital Tağınızda neler olduğunu görmenizi ve Azure desteği ile iletişim kurmaya gerek kalmadan sorunlar için kök neden analizi gerçekleştirmeye yardımcı olur.

Bu makalede, Azure dijital TWINS örneğinden ölçüm verileriniz için **tanılama günlüğü açma işleminin** nasıl yapılacağı gösterilir. Hizmet sorunlarını gidermenize yardımcı olması için bu günlükleri kullanabilir ve Azure dijital TWINS ölçümlerini farklı hedeflere göndermek için tanılama ayarlarını yapılandırabilirsiniz. Bu ayarlar hakkında daha fazla bilgi [*için Tanılama ayarları oluşturma ' da, farklı hedeflere platform günlükleri ve ölçümleri gönderebilirsiniz*](../azure-monitor/platform/diagnostic-settings.md).

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Tanılama ayarlarını Azure portal açın

Azure dijital TWINS örneğiniz için tanılama ayarlarının nasıl etkinleştirileceği aşağıda verilmiştir:

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure dijital TWINS örneğinize gidin. Bunu, adını Portal arama çubuğuna yazarak bulabilirsiniz. 

2. Menüden **Tanılama ayarları** ' nı seçin ve ardından **Tanılama ayarı ekleyin**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

3. Aşağıdaki sayfada aşağıdaki değerleri girin:
     * **Tanılama ayarı adı**: Tanılama ayarlarına bir ad verin.
     * **Kategori ayrıntıları**: hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için tanılamayı etkinleştirmek üzere kutulara göz atın. Tanılama ayarlarının rapor aldığı işlemler şunlardır:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - Queryoperation başvurusu destekleniyor
        - Tüm Ölçümler
        
        Bu seçenekler hakkında daha fazla ayrıntı için aşağıdaki [*Kategori ayrıntıları*](#category-details) bölümüne bakın.
     * **Hedef ayrıntıları**: günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
        - Log Analytics’e gönderme
        - Bir depolama hesabına arşivle
        - Bir olay hub'ına akış yap

        Hedef seçiminiz için gerekliyse ek ayrıntılar doldurmanız istenebilir.  
    
4. Yeni ayarları kaydedin. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Kullanıcının bir tanılama ayarı adı doldurduğu ve kategori ayrıntıları ve hedef ayrıntıları için bazı onay kutusu seçimleri yaptığı tanılama ayarı sayfasını gösteren ekran görüntüsü. Kaydet düğmesi vurgulanır.":::

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, olay, örneğiniz için **Tanılama ayarları** sayfasında yapılandırılan hedefte geri görüntülenir. 

## <a name="category-details"></a>Kategori ayrıntıları

Tanılama ayarlarını ayarlarken **Kategori ayrıntıları** altında seçilebilecekleri günlük kategorileri hakkında daha fazla ayrıntı aşağıda verilmiştir.

| Günlük kategorisi | Açıklama |
| --- | --- |
| ADTModelsOperation | Modellerle ilgili tüm API çağrılarını günlüğe kaydet |
| ADTQueryOperation | Sorgularla ilgili tüm API çağrılarını günlüğe kaydet |
| ADTEventRoutesOperation | Olay rotalarıyla ilgili tüm API çağrılarını ve Azure dijital TWINS 'den olayların çıkış durumunu Event Grid, Event Hubs ve Service Bus gibi bir uç nokta hizmetine kaydedin |
| ADTDigitalTwinsOperation | Azure dijital TWINS ile ilgili tüm API çağrılarını günlüğe kaydet |

Her günlük kategorisi yazma, okuma, silme ve eylem işlemlerinden oluşur.  Bu haritanın REST API çağrılar aşağıdaki gibidir:

| Olay türü | REST API işlemler |
| --- | --- |
| Yazma | YERLEŞTIRME ve düzeltme eki uygulama |
| Okuma | GET |
| Sil | DELETE |
| Eylem | POST |

Her kategoride günlüğe kaydedilen işlemlerin ve karşılık gelen [Azure dijital TWINS REST API çağrılarının](https://docs.microsoft.com/rest/api/azure-digitaltwins/) kapsamlı bir listesi aşağıda verilmiştir. 

>[!NOTE]
> Her günlük kategorisi birkaç işlem/REST API çağrısı içerir. Aşağıdaki tabloda, her günlük kategorisi, sonraki günlük kategorisi listelenene kadar tüm işlemler/REST API çağrılarıyla eşlenir. 

| Günlük kategorisi | İşlem | Çağrılar ve diğer olaylar REST API |
| --- | --- | --- |
| ADTModelsOperation | Microsoft. DigitalTwins/modeller/yazma | Digital Ikizi modelleri güncelleştirme API 'SI |
|  | Microsoft. DigitalTwins/modeller/okuma | KIMLIK ve liste API 'Lerine göre dijital Ikizi modelleri al |
|  | Microsoft. DigitalTwins/modeller/silme | Dijital Ikizi modelleri API 'YI Sil |
|  | Microsoft. DigitalTwins/modeller/eylem | Digital Ikizi modelleri API ekleme |
| ADTQueryOperation | Microsoft. DigitalTwins/sorgu/eylem | Sorgu TWINS API 'SI |
| ADTEventRoutesOperation | Microsoft. DigitalTwins/eventroutes/Write | Olay rotaları API ekleme |
|  | Microsoft. DigitalTwins/eventroutes/okuma | KIMLIĞE ve liste API 'Lerine göre olay yolları |
|  | Microsoft. DigitalTwins/eventrotalar/Sil | Olay rotaları API silme |
|  | Microsoft. DigitalTwins/eventroutes/eylem | Bir uç nokta hizmetine olay yayımlamaya çalışırken hata oluştu (API çağrısı değil) |
| ADTDigitalTwinsOperation | Microsoft. DigitalTwins/digitaltwıns/Write | Dijital TWINS ekleme, Ilişki ekleme, güncelleştirme, güncelleştirme bileşeni |
|  | Microsoft. DigitalTwins/digitaltwıns/okuma | KIMLIĞE göre dijital TWINS alma, bileşen alma, KIMLIĞE göre Ilişki alma, gelen Ilişkileri listeleme, Ilişkileri listeleme |
|  | Microsoft. DigitalTwins/digitaltwıns/Delete | Dijital TWINS silme, Ilişkiyi silme |
|  | Microsoft. DigitalTwins/digitaltwıns/eylem | Dijital TWINS bileşen telemetrisi gönderme, telemetri gönderme |

## <a name="log-schemas"></a>Günlük şemaları 

Her günlük kategorisinin, bu kategorideki olayların nasıl raporlandığını tanımlayan bir şeması vardır. Her bir günlük girişi metin olarak depolanır ve JSON blobu olarak biçimlendirilir. Günlükteki ve örnek JSON gövdelerinin alanları, aşağıdaki her günlük türü için verilmiştir. 

`ADTDigitalTwinsOperation`, `ADTModelsOperation` ve `ADTQueryOperation` TUTARLı bir API günlüğü şeması kullanın; `ADTEventRoutesOperation` kendi ayrı şemasına sahiptir.

### <a name="api-log-schemas"></a>API günlük şemaları

Bu günlük şeması,, ve için tutarlıdır `ADTDigitalTwinsOperation` `ADTModelsOperation` `ADTQueryOperation` . Bir Azure dijital TWINS örneğine yönelik API çağrılarına yönelik bilgiler içerir.

API günlüklerinin alan ve özellik açıklamaları aşağıda verilmiştir.

| Alan adı | Veri türü | Açıklama |
|-----|------|-------------|
| `Time` | DateTime | Bu olayın gerçekleştiği tarih ve saat (UTC) |
| `ResourceID` | Dize | Olayın gerçekleştiği kaynak için Azure Resource Manager kaynak KIMLIĞI |
| `OperationName` | Dize  | Olay sırasında gerçekleştirilen eylemin türü |
| `OperationVersion` | Dize | Olay sırasında kullanılan API sürümü |
| `Category` | Dize | Yayılmakta olan kaynağın türü |
| `ResultType` | Dize | Etkinliğin sonucu |
| `ResultSignature` | Dize | Olay için http durum kodu |
| `ResultDescription` | Dize | Olayla ilgili ek ayrıntılar |
| `DurationMs` | Dize | Olayın milisaniye cinsinden ne kadar sürdüğü |
| `CallerIpAddress` | Dize | Olay için maskelenmiş kaynak IP adresi |
| `CorrelationId` | Guid | Olay için müşteri tarafından sunulan benzersiz tanımlayıcı |
| `Level` | Dize | Etkinliğin günlüğe kaydetme önem derecesi |
| `Location` | Dize | Olayın gerçekleştiği bölge |
| `RequestUri` | Kullanılmamışsa | Olay sırasında kullanılan uç nokta |

Aşağıda bu tür Günlükler için örnek JSON gövdeleri verilmiştir.

#### <a name="adtdigitaltwinsoperation"></a>ADTDigitalTwinsOperation

```json
{
  "time": "2020-03-14T21:11:14.9918922Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/digitaltwins/write",
  "operationVersion": "2020-05-31-preview",
  "category": "DigitalTwinOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "314",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "2f6a8e64-94aa-492a-bc31-16b9f0b16ab3",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/digitaltwins/factory-58d81613-2e54-4faa-a930-d980e6e2a884?api-version=2020-05-31-preview"
}
```

#### <a name="adtmodelsoperation"></a>ADTModelsOperation

```json
{
  "time": "2020-10-29T21:12:24.2337302Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/models/write",
  "operationVersion": "2020-05-31-preview",
  "category": "ModelsOperation",
  "resultType": "Success",
  "resultSignature": "201",
  "resultDescription": "",
  "durationMs": "935",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "9dcb71ea-bb6f-46f2-ab70-78b80db76882",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/Models?api-version=2020-05-31-preview",
}
```

#### <a name="adtqueryoperation"></a>ADTQueryOperation

```json
{
  "time": "2020-12-04T21:11:44.1690031Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/query/action",
  "operationVersion": "2020-05-31-preview",
  "category": "QueryOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "255",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "1ee2b6e9-3af4-4873-8c7c-1a698b9ac334",
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/query?api-version=2020-05-31-preview",
}
```

### <a name="egress-log-schemas"></a>Çıkış günlüğü şemaları

Bu, Günlükler için şemadır `ADTEventRoutesOperation` . Bunlar, özel durumlarla ilgili ayrıntıları ve çıkış uç noktaları etrafında bir Azure dijital TWINS örneğine bağlı API işlemlerini içerir.

|Alan adı | Veri türü | Açıklama |
|-----|------|-------------|
| `Time` | DateTime | Bu olayın gerçekleştiği tarih ve saat (UTC) |
| `ResourceId` | Dize | Olayın gerçekleştiği kaynak için Azure Resource Manager kaynak KIMLIĞI |
| `OperationName` | Dize  | Olay sırasında gerçekleştirilen eylemin türü |
| `Category` | Dize | Yayılmakta olan kaynağın türü |
| `ResultDescription` | Dize | Olayla ilgili ek ayrıntılar |
| `Level` | Dize | Etkinliğin günlüğe kaydetme önem derecesi |
| `Location` | Dize | Olayın gerçekleştiği bölge |
| `EndpointName` | Dize | Azure dijital TWINS 'te oluşturulan çıkış uç noktası adı |

Aşağıda bu tür Günlükler için örnek JSON gövdeleri verilmiştir.

#### <a name="adteventroutesoperation"></a>ADTEventRoutesOperation

```json
{
  "time": "2020-11-05T22:18:38.0708705Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/eventroutes/action",
  "category": "EventRoutesOperation",
  "resultDescription": "Unable to send EventGrid message to [my-event-grid.westus-1.eventgrid.azure.net] for event Id [f6f45831-55d0-408b-8366-058e81ca6089].",
  "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
  "level": "3",
  "location": "southcentralus",
  "properties": {
    "endpointName": "endpointEventGridInvalidKey"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [*Azure kaynaklarınızdan günlük verilerini toplama ve*](../azure-monitor/platform/platform-logs-overview.md)kullanma.
* Azure dijital TWINS ölçümleri hakkında daha fazla bilgi için bkz. [*sorun giderme: Azure izleyici ile ölçümleri görüntüleme*](troubleshoot-metrics.md).
* Ölçümleriniz için uyarıların nasıl etkinleştirileceği hakkında bilgi için bkz. [*sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md).
