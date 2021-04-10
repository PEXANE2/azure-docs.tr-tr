---
title: Tanılama günlüklerini etkinleştirme ve sorgulama
titleSuffix: Azure Digital Twins
description: Tanılama ayarlarıyla günlüğe yazmayı etkinleştirme ve günlükleri anında görüntülemek için sorgulama bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 2/24/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 08db4d92da5213b1ce1b79867650da9df8c38ee4
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2021
ms.locfileid: "106385088"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Azure dijital TWINS sorunlarını giderme: tanılama günlüğü

Azure dijital TWINS, performansını, erişimini ve diğer verileri izlemek için hizmet örneğiniz için günlük toplayabilirler. Azure dijital TWINS Örneğinizde neler olduğunu görmek için bu günlükleri kullanabilir ve Azure desteği ile iletişim kurmaya gerek kalmadan sorunlar için kök neden analizi gerçekleştirebilirsiniz.

Bu makalede, Azure dijital TWINS örneğinden günlükleri toplamaya başlamak için [Azure Portal](https://portal.azure.com) [**Tanılama ayarlarının nasıl yapılandırılacağı**](#turn-on-diagnostic-settings) gösterilir. Günlüklerin nerede depolanması gerektiğini de belirtebilirsiniz (örneğin, Log Analytics veya tercih ettiğiniz bir depolama hesabı).

Bu makalede, Azure Digital TWINS 'in topladığı tüm [günlük kategorilerinin](#log-categories) ve [günlük şemalarının](#log-schemas) listeleri de bulunur.

Günlükleri ayarladıktan sonra, [**günlükleri sorgulayabilirsiniz ve**](#view-and-query-logs) hızlı bir şekilde özel Öngörüler toplayabilirsiniz.

## <a name="turn-on-diagnostic-settings"></a>Tanılama ayarlarını aç 

Azure dijital TWINS örneğiniz üzerinde günlük toplamaya başlamak için tanılama ayarlarını açın. Ayrıca, dışarıya kaydedilen günlüklerin depolanacağı hedefi de seçebilirsiniz. Azure dijital TWINS örneğiniz için tanılama ayarlarının nasıl etkinleştirileceği aşağıda verilmiştir.

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure dijital TWINS örneğinize gidin. Bunu, adını Portal arama çubuğuna yazarak bulabilirsiniz. 

2. Menüden **Tanılama ayarları** ' nı seçin ve ardından **Tanılama ayarı ekleyin**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü" lightbox="media/troubleshoot-diagnostics/diagnostic-settings.png":::

3. Aşağıdaki sayfada aşağıdaki değerleri girin:
     * **Tanılama ayarı adı**: Tanılama ayarlarına bir ad verin.
     * **Kategori ayrıntıları**: hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için tanılamayı etkinleştirmek üzere kutulara göz atın. Tanılama ayarlarının rapor aldığı işlemler şunlardır:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - Queryoperation başvurusu destekleniyor
        - Tüm Ölçümler
        
        Bu kategoriler ve içerdikleri bilgiler hakkında daha fazla ayrıntı için aşağıdaki [*günlük kategorileri*](#log-categories) bölümüne bakın.
     * **Hedef ayrıntıları**: günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
        - Log Analytics’e gönderme
        - Bir depolama hesabına arşivle
        - Bir olay hub'ına akış yap

        Hedef seçiminiz için gerekliyse ek ayrıntılar doldurmanız istenebilir.  
    
4. Yeni ayarları kaydedin. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Kullanıcının bir tanılama ayarı adı doldurduğu ve kategori ayrıntıları ve hedef ayrıntıları için bazı onay kutusu seçimleri yaptığı tanılama ayarı sayfasını gösteren ekran görüntüsü. Kaydet düğmesi vurgulanır." lightbox="media/troubleshoot-diagnostics/diagnostic-settings-details.png":::

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, olay, örneğiniz için **Tanılama ayarları** sayfasında yapılandırılan hedefte geri görüntülenir. 

Tanılama ayarları ve kurulum seçenekleri hakkında daha ayrıntılı bilgi için, [*Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek üzere tanılama ayarlarını oluştur*](../azure-monitor/essentials/diagnostic-settings.md)' u ziyaret edebilirsiniz.

## <a name="log-categories"></a>Günlük kategorileri

Azure Digital TWINS 'in topladığı günlük kategorileri hakkında daha fazla ayrıntı aşağıda verilmiştir.

| Günlük kategorisi | Açıklama |
| --- | --- |
| ADTModelsOperation | Modellerle ilgili tüm API çağrılarını günlüğe kaydet |
| ADTQueryOperation | Sorgularla ilgili tüm API çağrılarını günlüğe kaydet |
| ADTEventRoutesOperation | Olay rotalarıyla ilgili tüm API çağrılarını ve Azure dijital TWINS 'den olayların çıkış durumunu Event Grid, Event Hubs ve Service Bus gibi bir uç nokta hizmetine kaydedin |
| ADTDigitalTwinsOperation | Tek tek tüm API çağrılarını günlüğe kaydet |

Her günlük kategorisi yazma, okuma, silme ve eylem işlemlerinden oluşur.  Bu haritanın REST API çağrılar aşağıdaki gibidir:

| Olay türü | REST API işlemler |
| --- | --- |
| Yazma | YERLEŞTIRME ve düzeltme eki uygulama |
| Okuma | GET |
| Sil | DELETE |
| Eylem | POST |

Her kategoride günlüğe kaydedilen işlemlerin ve karşılık gelen [Azure dijital TWINS REST API çağrılarının](/rest/api/azure-digitaltwins/) kapsamlı bir listesi aşağıda verilmiştir. 

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

`ADTDigitalTwinsOperation`, `ADTModelsOperation` ve `ADTQueryOperation` TUTARLı bir API günlüğü şeması kullanır. `ADTEventRoutesOperation` Özellikleri, özelliklerde bir alan içerecek şekilde genişletir `endpointName` .

### <a name="api-log-schemas"></a>API günlük şemaları

Bu günlük şeması `ADTDigitalTwinsOperation` ,,, için `ADTModelsOperation` tutarlıdır `ADTQueryOperation` . Aynı şema, `ADTEventRoutesOperation` işlem adı **özel durumu** ile için de kullanılır `Microsoft.DigitalTwins/eventroutes/action` (Bu şema hakkında daha fazla bilgi için, sonraki bölüm, [*Çıkış günlüğü şemaları*](#egress-log-schemas)).

Şema, bir Azure dijital TWINS örneğine yönelik API çağrılarına yönelik bilgiler içerir.

API günlüklerinin alan ve özellik açıklamaları aşağıda verilmiştir.

| Alan adı | Veri türü | Açıklama |
|-----|------|-------------|
| `Time` | DateTime | Bu olayın gerçekleştiği tarih ve saat (UTC) |
| `ResourceId` | Dize | Olayın gerçekleştiği kaynak için Azure Resource Manager kaynak KIMLIĞI |
| `OperationName` | Dize  | Olay sırasında gerçekleştirilen eylemin türü |
| `OperationVersion` | Dize | Olay sırasında kullanılan API sürümü |
| `Category` | Dize | Yayılmakta olan kaynağın türü |
| `ResultType` | Dize | Etkinliğin sonucu |
| `ResultSignature` | Dize | Olay için http durum kodu |
| `ResultDescription` | Dize | Olayla ilgili ek ayrıntılar |
| `DurationMs` | Dize | Olayın milisaniye cinsinden ne kadar sürdüğü |
| `CallerIpAddress` | Dize | Olay için maskelenmiş kaynak IP adresi |
| `CorrelationId` | Guid | Olay için müşteri tarafından sunulan benzersiz tanımlayıcı |
| `ApplicationId` | Guid | Taşıyıcı yetkilendirmede kullanılan uygulama KIMLIĞI |
| `Level` | int | Etkinliğin günlüğe kaydetme önem derecesi |
| `Location` | Dize | Olayın gerçekleştiği bölge |
| `RequestUri` | Kullanılmamışsa | Olay sırasında kullanılan uç nokta |
| `TraceId` | Dize | `TraceId`, [W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Sistemler genelinde dağıtılmış bir izlemeyi benzersiz şekilde tanımlamak için kullanılan tüm izlemenin KIMLIĞI. |
| `SpanId` | Dize | `SpanId`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. İzlem içindeki bu isteğin KIMLIĞI. |
| `ParentId` | Dize | `ParentId`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Üst KIMLIĞI olmayan bir istek, izlemenin köküdür. |
| `TraceFlags` | Dize | `TraceFlags`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Örnekleme, izleme düzeyi vb. gibi izleme bayraklarını denetler. |
| `TraceState` | Dize | `TraceState`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Farklı dağıtılmış izleme sistemlerinde yaymak üzere satıcıya özgü ek izleme tanımlama bilgileri. |

Aşağıda bu tür Günlükler için örnek JSON gövdeleri verilmiştir.

#### <a name="adtdigitaltwinsoperation"></a>ADTDigitalTwinsOperation

```json
{
  "time": "2020-03-14T21:11:14.9918922Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/digitaltwins/write",
  "operationVersion": "2020-10-31",
  "category": "DigitalTwinOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": 8,
  "callerIpAddress": "13.68.244.*",
  "correlationId": "2f6a8e64-94aa-492a-bc31-16b9f0b16ab3",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/digitaltwins/factory-58d81613-2e54-4faa-a930-d980e6e2a884?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adtmodelsoperation"></a>ADTModelsOperation

```json
{
  "time": "2020-10-29T21:12:24.2337302Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/models/write",
  "operationVersion": "2020-10-31",
  "category": "ModelsOperation",
  "resultType": "Success",
  "resultSignature": "201",
  "resultDescription": "",
  "durationMs": "80",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "9dcb71ea-bb6f-46f2-ab70-78b80db76882",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/Models?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adtqueryoperation"></a>ADTQueryOperation

```json
{
  "time": "2020-12-04T21:11:44.1690031Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/query/action",
  "operationVersion": "2020-10-31",
  "category": "QueryOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "314",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "1ee2b6e9-3af4-4873-8c7c-1a698b9ac334",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/query?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adteventroutesoperation"></a>ADTEventRoutesOperation

Bu tür olmayan bir örnek JSON gövdesi aşağıda verilmiştir `ADTEventRoutesOperation`  `Microsoft.DigitalTwins/eventroutes/action` (Bu şema hakkında daha fazla bilgi için, [*Çıkış günlüğü şemaları*](#egress-log-schemas)sonraki bölümüne bakın).

```json
  {
    "time": "2020-10-30T22:18:38.0708705Z",
    "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
    "operationName": "Microsoft.DigitalTwins/eventroutes/write",
    "operationVersion": "2020-10-31",
    "category": "EventRoutesOperation",
    "resultType": "Success",
    "resultSignature": "204",
    "resultDescription": "",
    "durationMs": 42,
    "callerIpAddress": "212.100.32.*",
    "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
    "identity": {
      "claims": {
        "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
      }
    },
    "level": "4",
    "location": "southcentralus",
    "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/EventRoutes/egressRouteForEventHub?api-version=2020-10-31",
    "properties": {},
    "traceContext": {
      "traceId": "95ff77cfb300b04f80d83e64d13831e7",
      "spanId": "b630da57026dd046",
      "parentId": "9f0de6dadae85945",
      "traceFlags": "01",
      "tracestate": "k1=v1,k2=v2"
    }
  },
```

### <a name="egress-log-schemas"></a>Çıkış günlüğü şemaları

Bu, `ADTEventRoutesOperation` işlem adına özgü günlüklere yönelik şemadır `Microsoft.DigitalTwins/eventroutes/action` . Bunlar, özel durumlarla ilgili ayrıntıları ve çıkış uç noktaları etrafında bir Azure dijital TWINS örneğine bağlı API işlemlerini içerir.

|Alan adı | Veri türü | Açıklama |
|-----|------|-------------|
| `Time` | DateTime | Bu olayın gerçekleştiği tarih ve saat (UTC) |
| `ResourceId` | Dize | Olayın gerçekleştiği kaynak için Azure Resource Manager kaynak KIMLIĞI |
| `OperationName` | Dize  | Olay sırasında gerçekleştirilen eylemin türü |
| `Category` | Dize | Yayılmakta olan kaynağın türü |
| `ResultDescription` | Dize | Olayla ilgili ek ayrıntılar |
| `CorrelationId` | Guid | Olay için müşteri tarafından sunulan benzersiz tanımlayıcı |
| `ApplicationId` | Guid | Taşıyıcı yetkilendirmede kullanılan uygulama KIMLIĞI |
| `Level` | int | Etkinliğin günlüğe kaydetme önem derecesi |
| `Location` | Dize | Olayın gerçekleştiği bölge |
| `TraceId` | Dize | `TraceId`, [W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Sistemler genelinde dağıtılmış bir izlemeyi benzersiz şekilde tanımlamak için kullanılan tüm izlemenin KIMLIĞI. |
| `SpanId` | Dize | `SpanId`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. İzlem içindeki bu isteğin KIMLIĞI. |
| `ParentId` | Dize | `ParentId`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Üst KIMLIĞI olmayan bir istek, izlemenin köküdür. |
| `TraceFlags` | Dize | `TraceFlags`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Örnekleme, izleme düzeyi vb. gibi izleme bayraklarını denetler. |
| `TraceState` | Dize | `TraceState`[W3C's Trace bağlamının](https://www.w3.org/TR/trace-context/)bir parçası olarak. Farklı dağıtılmış izleme sistemlerinde yaymak üzere satıcıya özgü ek izleme tanımlama bilgileri. |
| `EndpointName` | Dize | Azure dijital TWINS 'te oluşturulan çıkış uç noktası adı |

Aşağıda bu tür Günlükler için örnek JSON gövdeleri verilmiştir.

#### <a name="adteventroutesoperation-for-microsoftdigitaltwinseventroutesaction"></a>Microsoft. DigitalTwins/eventrotalar/eylem için ADTEventRoutesOperation

Bu tür için örnek bir JSON gövdesi aşağıda verilmiştir `ADTEventRoutesOperation` `Microsoft.DigitalTwins/eventroutes/action` .

```json
{
  "time": "2020-11-05T22:18:38.0708705Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/eventroutes/action",
  "operationVersion": "",
  "category": "EventRoutesOperation",
  "resultType": "",
  "resultSignature": "",
  "resultDescription": "Unable to send EventHub message to [myPath] for event Id [f6f45831-55d0-408b-8366-058e81ca6089].",
  "durationMs": -1,
  "callerIpAddress": "",
  "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "",
  "properties": {
    "endpointName": "myEventHub"
  },
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
},
```

## <a name="view-and-query-logs"></a>Günlükleri görüntüleme ve sorgulama

Bu makalenin önceki kısımlarında, depolanacak günlük türlerini ve bunların depolama konumunu belirttiğini yapılandırdınız.

Sorunu gidermek ve bu günlüklerden Öngörüler oluşturmak için **özel sorgular** oluşturabilirsiniz. Başlamak için, hizmet tarafından sunulan ve müşterilerin örnekleri hakkında sahip olabileceği yaygın soruların ele aldığı birkaç örnek sorgudan yararlanabilirsiniz.

Örneğiniz için günlüklerin nasıl sorgulanacağını burada bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure dijital TWINS örneğinize gidin. Bunu, adını Portal arama çubuğuna yazarak bulabilirsiniz. 

2. Günlük sorgusu sayfasını açmak için menüden **Günlükler** ' i seçin. Sayfa, *sorgular* adlı bir pencerede açılır.

    :::image type="content" source="media/troubleshoot-diagnostics/logs.png" alt-text="Azure dijital TWINS örneği için Günlükler sayfasını gösteren ekran görüntüsü. DigitalTwin API gecikmesi ve model API gecikme süresi gibi farklı günlük seçeneklerinden sonra adlı önceden oluşturulmuş sorguları gösteren bir sorgular penceresi vardır." lightbox="media/troubleshoot-diagnostics/logs.png":::

    Bunlar, çeşitli Günlükler için yazılan önceden oluşturulmuş örnek sorgulardır. Sorgu düzenleyicisine yüklemek için sorgulardan birini seçebilir ve örneğiniz için bu günlükleri görmek üzere çalıştırabilirsiniz.

    Ayrıca sorgu Düzenleyicisi sayfasına herhangi bir şey yapmadan, özel sorgu kodu yazabileceğiniz veya düzenleyebileceğiniz sorgu Düzenleyicisi sayfasına doğrudan gidebileceğiniz *sorgular* penceresini kapatabilirsiniz.

3. *Sorgular* penceresinden çıktıktan sonra ana sorgu Düzenleyicisi sayfasını görürsünüz. Burada örnek sorguların metnini görüntüleyebilir ve düzenleyebilir ya da kendi sorgularınızı sıfırdan yazabilirsiniz.
    :::image type="content" source="media/troubleshoot-diagnostics/logs-query.png" alt-text="Azure dijital TWINS örneği için Günlükler sayfasını gösteren ekran görüntüsü. Sorgular penceresi kayboluyor ve bunun yerine farklı günlüklerin bir listesi, düzenlenebilir sorgu kodunu gösteren bir düzenleme bölmesi ve sorgu geçmişini gösteren bir bölme bulunur." lightbox="media/troubleshoot-diagnostics/logs-query.png":::

    Sol bölmede, 
    - *Tablolar* sekmesi, sorgularda kullanılabilecek farklı Azure dijital TWINS [günlük kategorilerini](#log-categories) gösterir. 
    - *Sorgular* sekmesi, düzenleyiciye yükleyebilmeniz için örnek sorguları içerir.
    - *Filtre* sekmesi, sorgunun döndürdüğü verilerin filtrelenmiş bir görünümünü özelleştirmenize olanak sağlar.

Günlük sorguları ve bunların nasıl yazılacağı hakkında daha ayrıntılı bilgi için [*Azure izleyici 'de günlük sorgularına genel bakış*](../azure-monitor/logs/log-query-overview.md)sayfasını ziyaret edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [*Azure kaynaklarınızdan günlük verilerini toplama ve*](../azure-monitor/essentials/platform-logs-overview.md)kullanma.
* Azure dijital TWINS ölçümleri hakkında daha fazla bilgi için bkz. [*sorun giderme: Azure izleyici ile ölçümleri görüntüleme*](troubleshoot-metrics.md).
* Ölçümleriniz için uyarıların nasıl etkinleştirileceği hakkında bilgi için bkz. [*sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md).