---
title: İş akışlarında hataları ve özel durumları işleme
description: Azure Mantık Uygulamaları kullanılarak oluşturulan otomatik görevlerde ve iş akışlarında meydana gelen hataları ve özel durumları nasıl ele alacağınızı öğrenin
services: logic-apps
ms.suite: integration
author: dereklee
ms.author: deli
ms.reviewer: klam, estfan, logicappspm
ms.date: 01/11/2020
ms.topic: article
ms.openlocfilehash: 73b116117530e5a2103b604efbf757d691006508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284037"
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Azure Mantık Uygulamaları'ndaki hataları ve özel durumları işleme

Herhangi bir tümleştirme mimarisinin kapalı kalma süresini uygun şekilde işleme sayılişi veya bağımlı sistemlerden kaynaklanan sorunlar sorun yaratabilir. Logic Apps, sorunları ve hataları incelikle işleyen sağlam ve esnek tümleştirmeler oluşturmanıza yardımcı olmak için hataları ve özel durumları işlemek için birinci sınıf bir deneyim sağlar.

<a name="retry-policies"></a>

## <a name="retry-policies"></a>Yeniden deneme ilkeleri

En temel özel durum ve hata işleme için, desteklenen herhangi bir eylem de yeniden *deneme ilkesi* kullanabilirsiniz, örneğin, [HTTP eylem](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)bakın. Yeniden deneme ilkesi, özgün istek zamanlandığında veya başarısız olduğunda eylemin veya tetikleyicinin isteği yeniden deneip yeniden denemediğini ve nasıl yeniden denediğini belirtir, bu da 408, 429 veya 5xx yanıtıyla sonuçlanan herhangi bir istektir. Başka bir yeniden deneme ilkesi kullanılmazsa, varsayılan ilke kullanılır.

Yeniden deneme ilkesi türleri şunlardır:

| Tür | Açıklama |
|------|-------------|
| **Varsayılan** | Bu ilke, 7,5 saniye ölçeklendirilir ancak 5 ile 45 saniye arasında kapaklı olan *katlanarak artan* aralıklarla en fazla dört yeniden deneme gönderir. |
| **Üstel aralık**  | Bu ilke, bir sonraki isteği göndermeden önce katlanarak büyüyen bir aralıktan seçilen rasgele bir aralık bekler. |
| **Sabit aralık**  | Bu ilke, bir sonraki isteği göndermeden önce belirtilen aralığı bekler. |
| **Yok**  | İsteği yeniden göndermeyin. |
|||

Yeniden deneme ilkesi sınırları hakkında daha fazla bilgi [için, Logic Apps sınırları ve](../logic-apps/logic-apps-limits-and-config.md#request-limits)yapılandırması'na bakın.

### <a name="change-retry-policy"></a>Yeniden deneme ilkesini değiştirme

Farklı bir yeniden deneme ilkesi seçmek için aşağıdaki adımları izleyin:

1. Mantık uygulamanızı Logic App Designer'da açın.

1. Bir eylem veya tetikleyici için **Ayarlar'ı** açın.

1. Eylem veya tetikleyici, **Yeniden Deneme İlkesi**altında yeniden deneme ilkelerini destekliyorsa, istediğiniz türü seçin.

Veya, yeniden deneme ilkelerini destekleyen bir `inputs` eylem veya tetikleyici için bölümdeki yeniden deneme ilkesini el ile belirtebilirsiniz. Yeniden deneme ilkesi belirtmezseniz, eylem varsayılan ilkeyi kullanır.

```json
"<action-name>": {
   "type": "<action-type>",
   "inputs": {
      "<action-specific-inputs>",
      "retryPolicy": {
         "type": "<retry-policy-type>",
         "interval": "<retry-interval>",
         "count": <retry-attempts>,
         "minimumInterval": "<minimum-interval>",
         "maximumInterval": "<maximum-interval>"
      },
      "<other-action-specific-inputs>"
   },
   "runAfter": {}
}
```

*Gerekli*

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*yeniden deneme-ilke türü*> | Dize | Kullanmak istediğiniz yeniden deneme ilkesi `default`türü: `fixed`, , `none`, veya`exponential` |
| <*yeniden deneme aralığı*> | Dize | Değerin [ISO 8601 biçimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)kullanması gereken yeniden deneme aralığı. Varsayılan minimum aralık `PT5S` ve maksimum `PT1D`aralık. Üstel aralık ilkesini kullandığınızda, farklı minimum ve maksimum değerler belirtebilirsiniz. |
| <*yeniden deneme denemeleri*> | Tamsayı | 1 ile 90 arasında olması gereken yeniden deneme denemelerinin sayısı |
||||

*Isteğe bağlı*

| Değer | Tür | Açıklama |
|-------|------|-------------|
| <*minimum aralık*> | Dize | Üstel aralık ilkesi için, [ISO 8601 formatında](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) rasgele seçilen aralık için en küçük aralık |
| <*maksimum aralık*> | Dize | Üstel aralık ilkesi [için, ISO 8601 formatında](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) rasgele seçilen aralık için en büyük aralık |
||||

Burada farklı ilke türleri hakkında daha fazla bilgi.

<a name="default-retry"></a>

### <a name="default"></a>Varsayılan

Yeniden deneme ilkesi belirtmezseniz, eylem varsayılan ilkeyi kullanır, bu aslında 7,5 saniye ölçeklendirilir katlanarak artan aralıklarda dört yeniden deneme gönderen bir [üstel aralık ilkesidir.](#exponential-interval) Aralık 5 ile 45 saniye arasında kapatılır.

Eyleminiz veya tetikleyicinizde açıkça tanımlanmamış olsa da, varsayılan ilke nin bir örnek HTTP eyleminde nasıl olduğu aşağıda açıklanmıştır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "http://myAPIendpoint/api/action",
      "retryPolicy" : {
         "type": "exponential",
         "interval": "PT7S",
         "count": 4,
         "minimumInterval": "PT5S",
         "maximumInterval": "PT1H"
      }
   },
   "runAfter": {}
}
```

### <a name="none"></a>None

Eylemin veya tetikleyicinin başarısız istekleri yeniden denemediğini belirtmek için, <yeniden deneme `none`ilkesi *türü*>' olarak ayarlayın.

### <a name="fixed-interval"></a>Sabit aralık

Eylem veya tetikleyicinin bir sonraki isteği göndermeden önce belirtilen aralığı beklediğini belirtmek için, `fixed`<yeniden deneme ilkesi *türü*>' olarak ayarlayın.

*Örnek*

Bu yeniden deneme ilkesi, her deneme arasında 30 saniyelik bir gecikmeyle ilk başarısız istekten sonra en son haberleri iki kez daha almaya çalışır:

```json
"Get_latest_news": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest",
      "retryPolicy": {
         "type": "fixed",
         "interval": "PT30S",
         "count": 2
      }
   }
}
```

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Üstel aralık

Eylem veya tetikleyicinin bir sonraki isteği göndermeden önce rasgele bir aralık beklediğini belirtmek için, `exponential`<yeniden deneme ilkesi *türü*>. Rasgele aralık, katlanarak büyüyen bir aralıktan seçilir. İsteğe bağlı olarak, kendi minimum ve maksimum aralıklarınızı belirterek varsayılan minimum ve maksimum aralıkları geçersiz kılabilirsiniz.

**Rasgele değişken aralıkları**

Bu tablo, Logic Apps'ın her yeniden deneme için belirtilen aralıkta nasıl tek tip rasgele değişken oluşturduğunu ve yeniden deneme sayısını nasıl oluşturduğunu gösterir:

| Yeniden deneme numarası | Minimum aralık | Maksimum aralık |
|--------------|------------------|------------------|
| 1 | max(0, <*minimum aralıklı*>) | min(aralık, <*maksimum aralıklı*>) |
| 2 | max(aralık, <*minimum aralıklı*>) | min(2 * aralığı, <*maksimum aralıklı*>) |
| 3 | max(2 * aralığı, <*minimum aralıklı*>) | min(4 * aralığı, <*maksimum aralıklı*>) |
| 4 | max(4 * aralığı, <*minimum aralıklı*>) | min(8 * aralığı, <*maksimum aralıklı*>) |
| .... | .... | .... |
||||

<a name="control-run-after-behavior"></a>

## <a name="catch-and-handle-failures-by-changing-run-after-behavior"></a>"Sonra koş" davranışını değiştirerek hataları yakalayın ve işleme

Mantık Uygulama Tasarımcısı'na eylemler eklediğinizde, bu eylemleri çalıştırmak için kullanılacak sırayı dolaylı olarak beyan emre esiniz. Bir eylem çalışma bittikten sonra, bu eylem `Succeeded`, `Failed` `Skipped`, `TimedOut`, veya . Her eylem tanımında, `runAfter` özellik, ardıl eylemin çalıştırılabilmesi için önce tamamlanması gereken öncül eylemi ve bu öncül için izin verilen durumları belirtir. Varsayılan olarak, tasarımcıya eklediğiniz bir eylem yalnızca öncül `Succeeded` durumla tamamlandıktan sonra çalışır.

Bir eylem işlenmemiş bir hata veya özel durum `Failed`attığında, eylem işaretlenir ve ardıl eylem işaretlenir. `Skipped` Bu davranış, paralel dalları olan bir eylem için gerçekleşirse, Mantıksal Uygulamalar altyapısı tamamlanma durumlarını belirlemek için diğer dalları izler. Örneğin, bir dal bir `Skipped` eylemle biterse, o dalanın tamamlanma durumu atlanan eylemin önceki durumuna dayanır. Mantık uygulaması tamamlandıktan sonra, motor tüm şube durumlarını değerlendirerek tüm çalıştırmanın durumunu belirler. Herhangi bir dal başarısızlıkla sonuçlanırsa, `Failed`tüm mantık uygulaması çalışması işaretlenir.

![Çalışma durumlarının nasıl değerlendirildiğini gösteren örnekler](./media/logic-apps-exception-handling/status-evaluation-for-parallel-branches.png)

Bir eylemin öncül'ün durumuna rağmen hala çalıştırıladığından emin olmak için, [bir eylemin "sonra koş" davranışını](#customize-run-after) öncülün başarısız durumlarını işlemek için özelleştirin.

<a name="customize-run-after"></a>

### <a name="customize-run-after-behavior"></a>"Sonra çalıştır" davranışını özelleştirme

Bir eylemin "sonra" davranışını özelleştirebilirsiniz, böylece eylem, önceki durumun , `Succeeded` `Failed`, `Skipped` `TimedOut`, veya bu durumlardan herhangi biri olduğunda çalışır. Örneğin, Excel Çevrimiçi `Add_a_row_into_a_table` selefi eylemi işaretlendikten `Failed`sonra e-posta göndermek için ,yerine, `Succeeded`her iki adımı izleyerek "sonra çalıştır" davranışını değiştirin:

* Tasarım görünümünde, elipsler (**...**) düğmesini seçin ve **sonra 'dan sonra çalıştır'ı yapılandır'ı**seçin.

  ![Eylem için "sonra çalıştır" davranışını yapılandırma](./media/logic-apps-exception-handling/configure-run-after-property-setting.png)

  Eylem şekli, bu örnekte **bir tabloya satır ekle** olan öncül eylem için gerekli olan varsayılan durumu gösterir:

  ![Bir eylem için varsayılan "sonra çalıştır" davranışı](./media/logic-apps-exception-handling/change-run-after-property-status.png)

  Bu örnekte **başarısız** olan "sonra çalıştır" davranışını istediğiniz durumla değiştirin:

  !["Sonra çalıştır" davranışını "başarısız oldu" olarak değiştirme](./media/logic-apps-exception-handling/run-after-property-status-set-to-failed.png)

  Eylemin, öncül eylemin "veya `Failed` `Skipped` `TimedOut`diğer durumları seç" olarak işaretlenip işaretlenmediğini belirtmek için:

  ![Başka bir statüye sahip olacak şekilde "sonra çalıştır" davranışını değiştirme](./media/logic-apps-exception-handling/run-after-property-multiple-statuses.png)

* Kod görünümünde, eylemin JSON tanımında, bu `runAfter` sözdizimini izleyen özelliği ni de

  ```json
  "<action-name>": {
     "inputs": {
        "<action-specific-inputs>"
     },
     "runAfter": {
        "<preceding-action>": [
           "Succeeded"
        ]
     },
     "type": "<action-type>"
  }
  ```

  Bu örnekte, `runAfter` özelliği `Succeeded` aşağıdakilerden `Failed`

  ```json
  "Send_an_email_(V2)": {
     "inputs": {
        "body": {
           "Body": "<p>Failed to&nbsp;add row to &nbsp;@{body('Add_a_row_into_a_table')?['Terms']}</p>",,
           "Subject": "Add row to table failed: @{body('Add_a_row_into_a_table')?['Terms']}",
           "To": "Sophia.Owen@fabrikam.com"
        },
        "host": {
           "connection": {
              "name": "@parameters('$connections')['office365']['connectionId']"
           }
        },
        "method": "post",
        "path": "/v2/Mail"
     },
     "runAfter": {
        "Add_a_row_into_a_table": [
           "Failed"
        ]
     },
     "type": "ApiConnection"
  }
  ```

  Eylemin, öncül eylemin "veya `Failed` `Skipped` `TimedOut`diğer durumları ekle" olarak işaretlenip işaretlenmediğini belirtmek için:

  ```json
  "runAfter": {
     "Add_a_row_into_a_table": [
        "Failed", "Skipped", "TimedOut"
     ]
  },
  ```

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Kapsamları ve sonuçları ile eylemleri değerlendirmek

`runAfter` Özellik ile tek tek eylemlerden sonra adımları çalıştırmaya benzer şekilde, eylemleri bir [kapsam](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)içinde gruplayabilirsiniz. Eylemleri mantıksal olarak gruplandırmak, kapsamın toplam durumunu değerlendirmek ve bu duruma göre eylemler gerçekleştirmek istediğinizde kapsamları kullanabilirsiniz. Bir kapsamdaki tüm eylemler çalışma ktan sonra, kapsamın kendisi kendi durumunu alır.

Bir kapsam durumunu denetlemek için, bir mantık uygulamasının çalışma durumunu denetlemek için kullandığınız ölçütleri kullanabilirsiniz( `Succeeded`, , ve `Failed`benzeri).

Varsayılan olarak, tüm kapsam eylemleri başarılı olduğunda, kapsamın durumu `Succeeded`işaretlenir. Bir kapsamdaki son eylem, `Failed` `Aborted`kapsamın durumu olarak veya `Failed`olarak sonuçlanırsa.

Bir `Failed` kapsamdaki özel durumları yakalamak ve bu hataları işleyen `runAfter` eylemleri çalıştırmak `Failed` için özelliği bu kapsam için kullanabilirsiniz. Bu şekilde, kapsamdaki *herhangi bir* eylem başarısız `runAfter` olursa ve özelliği bu kapsam için kullanırsanız, hataları yakalamak için tek bir eylem oluşturabilirsiniz.

Kapsamsınırlamaları için [Sınırlar ve config'e](../logic-apps/logic-apps-limits-and-config.md)bakın.

<a name="get-results-from-failures"></a>

### <a name="get-context-and-results-for-failures"></a>Hatalar için bağlam ve sonuç alma

Bir kapsamdan hataları yakalamak yararlı olsa da, tam olarak hangi eylemlerin başarısız olduğunu ve döndürülen hataları veya durum kodlarını tam olarak anlamanıza yardımcı olacak bağlam isteyebilirsiniz.

İşlev, [`result()`](../logic-apps/workflow-definition-language-functions-reference.md#result) bir kapsamdaki tüm eylemlerin sonuçları hakkında bağlam sağlar. İşlev, `result()` kapsamın adı olan tek bir parametreyi kabul eder ve bu kapsamdaki tüm eylem sonuçlarını içeren bir dizi döndürür. Bu eylem nesneleri, eylemin başlangıç `actions()` zamanı, bitiş saati, durum, girişler, korelasyon iI'leri ve çıktıları gibi nesneyle aynı öznitelikleri içerir. Kapsam içinde başarısız olan eylemler için bağlam göndermek için, `@result()` bir `runAfter` ifadeyi özellik ile kolayca eşleştirebilirsiniz.

Sonuç olan bir kapsamda her eylem için `Failed` bir eylem çalıştırmak ve sonuç dizisini başarısız eylemlere `@result()` filtrelemek için, bir ifadeyi [**Filtre Dizisi**](logic-apps-perform-data-operations.md#filter-array-action) eylemi ve her döngü [**için**](../logic-apps/logic-apps-control-flow-loops.md) eşleştirebilirsiniz. Filtre uygulanmış sonuç dizisini alabilir ve döngüyü `For_each` kullanarak her hata için bir eylem gerçekleştirebilirsiniz.

Burada bir örnek, ayrıntılı bir açıklama ardından, bu kapsamında "My_Scope" başarısız herhangi bir eylem için yanıt gövdesi ile bir HTTP POST isteği gönderir:

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": "@result('My_Scope')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "My_Scope": [
         "Failed"
      ]
    }
},
"For_each": {
   "type": "foreach",
   "actions": {
      "Log_exception": {
         "type": "Http",
         "inputs": {
            "method": "POST",
            "body": "@item()['outputs']['body']",
            "headers": {
               "x-failed-action-name": "@item()['name']",
               "x-failed-tracking-id": "@item()['clientTrackingId']"
            },
            "uri": "http://requestb.in/"
         },
         "runAfter": {}
      }
   },
   "foreach": "@body('Filter_array')",
   "runAfter": {
      "Filter_array": [
         "Succeeded"
      ]
   }
}
```

Aşağıda, bu örnekte neler olduğunu açıklayan ayrıntılı bir gözden geçirme durumu verilmiştir:

1. "My_Scope" içindeki tüm eylemlerin sonucunu almak için **Filtre Dizisi** eylemi şu filtre ifadesini kullanır:`@result('My_Scope')`

1. Filtre Dizisi'nin `@result()` `Failed`koşulu, ''ye eşit bir durumu olan herhangi bir öğedir. **Filter Array** Bu koşul, "My_Scope" tüm eylem sonuçlarına sahip diziyi yalnızca başarısız eylem sonuçlarıyla birlikte bir diziye filtreler.

1. Filtre `For_each` *uygulanmış dizi* çıktılarında döngü eylemi gerçekleştirin. Bu adım, daha önce filtre uygulanmış her başarısız eylem sonucu için bir eylem gerçekleştirir.

   Kapsamdaki tek bir eylem başarısız olursa, `For_each` döngüdeki eylemler yalnızca bir kez çalışır. Birden çok başarısız eylemler, hata başına bir eyleme neden olur.

1. Öğe yanıt gövdesine `For_each` bir HTTP POST gönderin, bu ifadedir. `@item()['outputs']['body']`

   `@result()` Madde şekli `@actions()` şekille aynıdır ve aynı şekilde ayrıştırılabilir.

1. Başarısız eylem adı (`@item()['name']`) ve başarısız çalıştırılamamış istemci`@item()['clientTrackingId']`izleme kimliği ( ) ile iki özel üstbilgi ekleyin.

`@result()` Başvuru için, önceki örnekte ayrıştılan `name`, `body`ve `clientTrackingId` özellikleri gösteren tek bir öğenin bir örneği verilmiştir. Eylem `For_each` dışında, `@result()` bu nesnelerin bir dizi döndürür.

```json
{
   "name": "Example_Action_That_Failed",
   "inputs": {
      "uri": "https://myfailedaction.azurewebsites.net",
      "method": "POST"
   },
   "outputs": {
      "statusCode": 404,
      "headers": {
         "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
         "Server": "Microsoft-IIS/8.0",
         "X-Powered-By": "ASP.NET",
         "Content-Length": "68",
         "Content-Type": "application/json"
      },
      "body": {
         "code": "ResourceNotFound",
         "message": "/docs/folder-name/resource-name does not exist"
      }
   },
   "startTime": "2016-08-11T03:18:19.7755341Z",
   "endTime": "2016-08-11T03:18:20.2598835Z",
   "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
   "clientTrackingId": "08587307213861835591296330354",
   "code": "NotFound",
   "status": "Failed"
}
```

Farklı özel durum işleme desenleri gerçekleştirmek için, bu makalede daha önce açıklanan ifadeleri kullanabilirsiniz. Filtre uygulanmış tüm hata dizisini kabul eden kapsam dışında tek bir özel durum `For_each` işleme eylemini yürütmeyi ve eylemi kaldırmayı seçebilirsiniz. Yanıttan `\@result()` daha önce açıklandığı gibi diğer yararlı özellikleri de ekleyebilirsiniz.

## <a name="set-up-azure-monitor-logs"></a>Azure Monitör günlüklerini ayarlama

Önceki desenler, bir çalıştırmadaki hataları ve özel durumları işlemek için harika bir yoldur, ancak çalıştırın kendisinden bağımsız hataları da tanımlayabilir ve bunlara yanıt verebilirsiniz. [Azure Monitor,](../azure-monitor/overview.md) tüm çalıştırma ve eylem durumları da dahil olmak üzere tüm iş akışı etkinliklerini Bir [Log Analytics çalışma alanına,](../azure-monitor/platform/data-platform-logs.md) [Azure depolama hesabına](../storage/blobs/storage-blobs-overview.md)veya [Azure Etkinlik Hub'larına göndermenin](../event-hubs/event-hubs-about.md)basit bir yolunu sağlar.

Çalıştırma durumlarını değerlendirmek için günlükleri ve ölçümleri izleyebilir veya bunları tercih ettiğiniz herhangi bir izleme aracında yayımlayabilirsiniz. Olası seçeneklerden biri, tüm etkinlikleri Etkinlik Hub'ları üzerinden [Azure Akış Analizi'ne](https://azure.microsoft.com/services/stream-analytics/)aktarmaktır. Akış Analizi'nde, tanı günlüklerinden kaynaklanan anormallikleri, ortalamaları veya hataları temel alan canlı sorgular yazabilirsiniz. Sıralar, konular, SQL, Azure Cosmos DB veya Power BI gibi diğer veri kaynaklarına bilgi göndermek için Akış Analizi'ni kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps ile müşterinin hata işlemeyi nasıl oluşturduğunu görün](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Daha fazla Mantık Uygulamaları örneği ve senaryosu bulun](../logic-apps/logic-apps-examples-and-scenarios.md)
