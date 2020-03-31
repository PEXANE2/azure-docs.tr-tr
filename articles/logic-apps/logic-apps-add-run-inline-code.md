---
title: Satır kodu kullanarak kod parçacıkları ekleme ve çalıştırma
description: Azure Logic Apps ile oluşturduğunuz otomatik görevler ve iş akışları için satır ara kod eylemlerini kullanarak kod parçacıklarını nasıl oluşturup çalıştırabileceğinizi öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: deli, logicappspm
ms.topic: article
ms.date: 05/14/2019
ms.openlocfilehash: f7a134fd026b42d1666b8310b3fb0c10642c7bb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75453507"
---
# <a name="add-and-run-code-snippets-by-using-inline-code-in-azure-logic-apps"></a>Azure Logic Apps'ta satır çizgisi kodunu kullanarak kod parçacıkları ekleme ve çalıştırma

Mantık uygulamanızın içinde bir kod parçası çalıştırmak istediğinizde, yerleşik **Inline Code** eylemini mantık uygulamanızın iş akışında bir adım olarak ekleyebilirsiniz. Bu eylem, bu senaryoya uyan kodu çalıştırmak istediğinizde en iyi şekilde çalışır:

* JavaScript'te çalışır. Yakında daha fazla dil var.
* Beş saniye veya daha az sürede çalışır.
* 50 MB boyutuna kadar olan verileri işler.
* Henüz desteklenmeyen [ **Değişkenler** eylemleriyle](../logic-apps/logic-apps-create-variables-store-values.md)çalışmayı gerektirmez.
* Node.js sürüm 8.11.1 kullanır. Daha fazla bilgi için Standart [yerleşik nesnelere](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)bakın. 

  > [!NOTE]
  > İşlev, `require()` JavaScript'i çalıştırmak için **Satır Satır Kodu** eylemi tarafından desteklenmez.

Bu eylem kod parçacıkını çalıştırırken, mantık uygulamanızdaki sonraki eylemlerde kullanabileceğiniz **Sonuç**adlı bir belirteç olarak bu parçacıktan çıktıyı döndürür. Kodunuz için bir işlev oluşturmak istediğiniz diğer senaryolar için, mantık uygulamanızda [bir Azure işlevi oluşturmayı ve aramayı](../logic-apps/logic-apps-azure-functions.md) deneyin.

Bu makalede, örnek mantık uygulaması, Office 365 Outlook hesabına yeni bir e-posta geldiğinde tetikler. Kod snippet ayıklar ve e-posta gövdesinde görünen herhangi bir e-posta adresleri döndürür.

![Örnek genel bakış](./media/logic-apps-add-run-inline-code/inline-code-example-overview.png)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Tetikleyici de dahil olmak üzere kod parçacığınızı eklemek istediğiniz mantık uygulaması. Bir mantık uygulamanız yoksa, [Bkz. Quickstart: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

   Bu konudaki örnek mantık uygulaması bu Office 365 Outlook tetikleyicisini kullanır: **Yeni bir e-posta geldiğinde**

* Mantık uygulamanızla bağlantılı bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

  > [!NOTE]
  > Kullanım durumunuza veya senaryonuza uygun bir entegrasyon hesabı kullandığınızdan emin olun. Örneğin, [Serbest katmanlı](../logic-apps/logic-apps-pricing.md#integration-accounts) tümleştirme hesapları yalnızca araştırmacı senaryolar ve iş yükleri içindir, üretim senaryoları için değil, kullanım ve iş üretimi sınırlıdır ve hizmet düzeyi sözleşmesi (SLA) tarafından desteklenmez. Diğer katmanlar maliyetlere tabidir, ancak SLA desteği, daha fazla iş fazlası sunar ve daha yüksek sınırlara sahiptir. Entegrasyon hesap [katmanları,](../logic-apps/logic-apps-pricing.md#integration-accounts) [fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/)ve [limitler](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)hakkında daha fazla bilgi edinin.

## <a name="add-inline-code"></a>Satır satır kodu ekleme

1. Azure [portalında](https://portal.azure.com)henüz yapmadıysanız, mantık uygulamanızı Logic App Designer'da açın.

1. Tasarımcıda, mantık uygulamanızın iş akışına istediğiniz konuma **Satır İçi Kod** eylemini ekleyin.

   * İş akışınızın sonundaki eylemi eklemek için **Yeni adımı**seçin.

   * Eylemi varolan adımlar arasına eklemek için fare işaretçinizi bu adımları bağlayan okun üzerine taşıyın. Artı işaretini**+** seçin ( ), ve **eylem ekle'yi**seçin.

   Bu örnek, Office 365 Outlook tetikleyicisi altında **Satır Çizgisi Kodu** eylemini ekler.

   ![Yeni adım ekleme](./media/logic-apps-add-run-inline-code/add-new-step.png)

1. Arama kutusuna **bir eylem seçin**altında filtreniz olarak "satır çizgisi kodu" girin. Eylemler listesinden şu eylemi seçin: **JavaScript Kodunu Yürüt**

   !["JavaScript Kodunu Çalıştır" seçeneğini belirleyin](./media/logic-apps-add-run-inline-code/select-inline-code-action.png)

   Eylem tasarımcıda görünür ve iade deyimi de dahil olmak üzere bazı varsayılan örnek kodu içerir.

   ![Varsayılan örnek kodu ile Satır Kodu eylemi](./media/logic-apps-add-run-inline-code/inline-code-action-default.png)

1. **Kod** kutusuna, örnek kodu silin ve çalıştırmak istediğiniz kodu girin. Bir yöntemin içine koyduğunuz, ancak yöntem imzasını tanımlamadan kod yazın. 

   Tanınan bir anahtar kelime yazdığınızda, kullanılabilir anahtar kelimelerden seçim yapabilmeniz için otomatik tamamlama listesi görüntülenir:

   ![Anahtar kelime otomatik tamamlama listesi](./media/logic-apps-add-run-inline-code/auto-complete.png)

   Bu örnek kod snippet ilk *olarak,* giriş metninde eşleşecek bir desen belirten normal bir ifade yi depolayan bir değişken oluşturur. Kod daha sonra tetikleyici den e-posta gövdesi verileri depolayan bir değişken oluşturur.

   ![Değişken oluşturma](./media/logic-apps-add-run-inline-code/save-email-body-variable.png)

   Tetikleyici ve önceki eylemlerin sonuçlarıyla başvuruyu kolaylaştırmak için imleciniz **Kod** kutusunun içindeyken dinamik içerik listesi görüntülenir. Bu örnekte, liste, şimdi seçebileceğiniz **Gövde** belirteci de dahil olmak üzere tetikleyiciden elde edilen kullanılabilir sonuçları gösterir.

   **Gövde** belirteci'ni seçtikten sonra, satır satır kodu `workflowContext` eylemi belirteci e-postanın `Body` özellik değerine başvuran bir nesneye giderir:

   ![Sonucu seçin](./media/logic-apps-add-run-inline-code/inline-code-example-select-outputs.png)

   **Kod** kutusunda, parçacıkınızın salt `workflowContext` okunur nesnesini giriş olarak kullanabileceğiz. Bu nesne, kodunuzu iş akışınızdaki tetikleyicive önceki eylemlerin sonuçlarına erişim sağlayan alt özelliklere sahiptir.
   Daha fazla bilgi için, bu konunun ilerleyen bölümlerinde şu bölüme bakın: [Kodunuzdaki başvuru tetikleyicisi ve eylem sonuçları.](#workflowcontext)

   > [!NOTE]
   >
   > Kod parçacığınız nokta (.) işlecikullanan eylem adlarına başvuruyorsa, bu eylem adlarını [ **Eylemler** parametresine](#add-parameters)eklemeniz gerekir. Bu başvurular, eylem adlarını kare ayraçlarla ([]) ve tırnak işaretleriyle de içine almalıdır, örneğin:
   >
   > `// Correct`</br> 
   > `workflowContext.actions["my.action.name"].body`</br>
   >
   > `// Incorrect`</br>
   > `workflowContext.actions.my.action.name.body`

   Satır satırkodu eylemi bir `return` deyim gerektirmez, ancak bir `return` deyimin sonuçları **Sonuç** belirteci aracılığıyla sonraki eylemlerde başvuru için kullanılabilir. 
   Örneğin, kod snippet normal ifade karşı `match()` e-posta gövdesinde eşleşmeleri bulur işlevi çağırarak sonucu döndürür. **Oluşturma** eylemi, satır satır kodu eyleminden elde edilen sonuçlara başvurmak için **Sonuç** belirteci kullanır ve tek bir sonuç oluşturur.

   ![Tamamlanmış mantıksal uygulama](./media/logic-apps-add-run-inline-code/inline-code-complete-example.png)

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

<a name="workflowcontext"></a>

### <a name="reference-trigger-and-action-results-in-your-code"></a>Kodunuzda başvuru tetikleyicisi ve eylem sonuçları

Nesne, `workflowContext` `actions`, ve `trigger` `workflow` alt özellikleri içeren bu yapıya sahiptir:

```json
{
   "workflowContext": {
      "actions": {
         "<action-name-1>": @actions('<action-name-1>'),
         "<action-name-2>": @actions('<action-name-2>')
      },
      "trigger": {
         @trigger()
      },
      "workflow": {
         @workflow()
      }
   }
}
```

Bu tablo, bu alt özellikler hakkında daha fazla bilgi içerir:

| Özellik | Tür | Açıklama |
|----------|------|-------|
| `actions` | Nesne toplama | Kod snippet'iniz çalışmadan önce çalışan eylemlerden nesneler elde edin. Her nesnenin anahtar *değeri* çiftinin bir eylemin adı olduğu ve değerin [eylemler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#actions) `@actions('<action-name>')`. Eylemin adı, temel iş akışı tanımında kullanılan ve eylem adındaki boşlukların (" ") alt alt çizerlerle (_) yerini alan aynı eylem adını kullanır. Bu nesne, geçerli iş akışı örneği çalıştırından eylem özelliği değerlerine erişim sağlar. |
| `trigger` | Nesne | Tetikleyiciden sonuç nesnesi ve [tetikleyici() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#trigger)çağırmaya eşdeğerdir. Bu nesne, geçerli iş akışı örneği çalıştırÇalıştırAn özellik değerlerini tetikleme erişimi sağlar. |
| `workflow` | Nesne | İş akışı nesnesi ve [iş akışı() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#workflow)çağırmaya eşdeğerdir. Bu nesne, geçerli iş akışı örneği çalıştırıçalıştırAn iş akışı adı, çalışma kimliği ve benzeri gibi iş akışı özelliği değerlerine erişim sağlar. |
|||

Bu konunun örneğinde, `workflowContext` nesne, kodunuzu erişebileceği özelliklere sahiptir:

```json
{
   "workflowContext": {
      "trigger": {
         "name": "When_a_new_email_arrives",
         "inputs": {
            "host": {
               "connection": {
                  "name": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/office365"
               }
            },
            "method": "get",
            "path": "/Mail/OnNewEmail",
            "queries": {
               "includeAttachments": "False"
            }
         },
         "outputs": {
            "headers": {
               "Pragma": "no-cache",
               "Content-Type": "application/json; charset=utf-8",
               "Expires": "-1",
               "Content-Length": "962095"
            },
            "body": {
               "Id": "AAMkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgBGAAAAAABmZwxUQtCGTqSPpjjMQeD",
               "DateTimeReceived": "2019-03-28T19:42:16+00:00",
               "HasAttachment": false,
               "Subject": "Hello World",
               "BodyPreview": "Hello World",
               "Importance": 1,
               "ConversationId": "AAQkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgAQ",
               "IsRead": false,
               "IsHtml": true,
               "Body": "Hello World",
               "From": "<sender>@<domain>.com",
               "To": "<recipient-2>@<domain>.com;<recipient-2>@<domain>.com",
               "Cc": null,
               "Bcc": null,
               "Attachments": []
            }
         },
         "startTime": "2019-05-03T14:30:45.971564Z",
         "endTime": "2019-05-03T14:30:50.1746874Z",
         "scheduledTime": "2019-05-03T14:30:45.8778117Z",
         "trackingId": "1cd5ffbd-f989-4df5-a96a-6e9ce31d03c5",
         "clientTrackingId": "08586447130394969981639729333CU06",
         "originHistoryName": "08586447130394969981639729333CU06",
         "code": "OK",
         "status": "Succeeded"
      },
      "workflow": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>",
         "name": "<logic-app-workflow-name>",
         "type": "Microsoft.Logic/workflows",
         "location": "<Azure-region>",
         "run": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>/runs/08586453954668694173655267965CU00",
            "name": "08586453954668694173655267965CU00",
            "type": "Microsoft.Logic/workflows/runs"
         }
      }
   }
}
```

<a name="add-parameters"></a>

## <a name="add-parameters"></a>Parametre ekleme

Bazı durumlarda, **Satır İçi Kod** eyleminin **tetikleyiciden** veya tetikleyici veya **Eylemler** parametrelerini ekleyerek kodunuzu bağımlılık olarak başvuran tetikleyiciden veya belirli eylemlerden sonuçlar içerdiğini açıkça talep etmeniz gerekebilir. Bu seçenek, başvurulan sonuçların çalışma zamanında bulunamadığı senaryolar için yararlıdır.

> [!TIP]
> Kodunuzu yeniden kullanmayı planlıyorsanız, tetikleyiciyi veya eylemleri açık bağımlılıklar olarak eklemek yerine, kodunuzu çözümlenmiş belirteç başvurularını içerecek şekilde **Kod** kutusunu kullanarak özelliklere başvurular ekleyin.

Örneğin, Office 365 Outlook bağlayıcısı için **onay e-posta gönder** eyleminden **SelectedOption** sonucuna başvuran bir kodunuz olduğunu varsayalım. Oluşturma zamanında, Logic Apps altyapısı herhangi bir tetikleyici veya eylem sonuçlarına başvurup başvurmadığınızı belirlemek için kodunuzu analiz eder ve bu sonuçları otomatik olarak içerir. Çalışma zamanında, başvurulan tetikleyici veya eylem sonucu belirtilen `workflowContext` nesnede kullanılamadığı bir hata alırsanız, bu tetikleyiciyi veya eylemi açık bir bağımlılık olarak ekleyebilirsiniz. Bu örnekte, **Eylemler** parametresini ekler ve **Satır İçi Kod** eyleminin onay **e-posta gönder** eyleminden elde edilen sonucu açıkça içerdiğini belirtirsiniz.

Bu parametreleri eklemek için **yeni parametre** ekle listesini açın ve istediğiniz parametreleri seçin:

   ![Parametre ekleme](./media/logic-apps-add-run-inline-code/inline-code-action-add-parameters.png)

   | Parametre | Açıklama |
   |-----------|-------------|
   | **Eylemler** | Önceki eylemlerin sonuçlarını ekleyin. Bkz. [Eylem sonuçlarını dahil et.](#action-results) |
   | **Tetikleyici** | Tetikleyicinin sonuçlarını ekleyin. Bkz. [Tetikleyici sonuçları dahil et.](#trigger-results) |
   |||

<a name="trigger-results"></a>

### <a name="include-trigger-results"></a>Tetikleyici sonuçları ekleme

**Tetikleyiciler'i**seçerseniz, tetikleyici sonuçları ekleyip eklememeniz istenir.

* **Tetikleyici** listesinden **Evet'i**seçin.

<a name="action-results"></a>

### <a name="include-action-results"></a>Eylem sonuçlarını ekleme

**Eylemler'i**seçerseniz, eklemek istediğiniz eylemler için istenirsiniz. Ancak, eylem eklemeye başlamadan önce, mantık uygulamasının temel iş akışı tanımında görünen eylem adının sürümüne ihtiyacınız vardır.

* Bu özellik değişkenleri, döngüleri ve yineleme dizinlerini desteklemez.

* Mantık uygulamanızın iş akışı tanımındaki adlar bir boşluk değil, alt alt (_) kullanır.

* Nokta işleci (.) kullanan eylem adları için, örneğin aşağıdakileri içerir:

  `My.Action.Name`

1. Tasarımcı araç çubuğunda **Kod görünümü'ni**seçin `actions` ve eylem adı için öznitelik içinde arama yapın.

   Örneğin, `Send_approval_email_` **onay e-posta gönder** eyleminin JSON adıdır.

   ![JSON'da eylem adını bulma](./media/logic-apps-add-run-inline-code/find-action-name-json.png)

1. Tasarımcı görünümüne dönmek için, kod görünümü araç **çubuğunda, Designer'ı**seçin.

1. İlk eylemi eklemek için **Eylemler Öğesi - 1** kutusuna eylemin JSON adını girin.

   ![İlk eylemi girin](./media/logic-apps-add-run-inline-code/add-action-parameter.png)

1. Başka bir eylem eklemek **için yeni öğe ekle'yi**seçin.

## <a name="reference"></a>Başvuru

İş Akışı Tanım Dilini kullanarak mantık uygulamanızın temel iş akışı tanımındaki **Execute JavaScript Kodu** eyleminin yapısı ve sözdizimi hakkında daha fazla bilgi için bu eylemin [başvuru bölümüne](../logic-apps/logic-apps-workflow-actions-triggers.md#run-javascript-code)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Mantık Uygulamaları için Bağlayıcılar](../connectors/apis-list.md) hakkında daha fazla bilgi edinin
