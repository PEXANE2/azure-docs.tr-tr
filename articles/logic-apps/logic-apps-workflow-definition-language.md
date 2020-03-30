---
title: İş Akışı Tanımı Dil şema başvurusu
description: Azure Logic Apps'ta iş akışlarını açıklayan İş Akışı Tanımı Dili için JSON şeması ve sözdizimi için başvuru kılavuzu
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: ff2267c2d03076d3abc44d0bd1dddc64577cc7f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283868"
---
# <a name="schema-reference-guide-for-the-workflow-definition-language-in-azure-logic-apps"></a>Azure Logic Apps'ta İş Akışı Tanımı Dili için Şema başvuru kılavuzu

[Azure Logic Apps'ta](../logic-apps/logic-apps-overview.md)bir mantık uygulaması oluşturduğunuzda, mantık uygulamanız, mantık uygulamanızda çalışan gerçek mantığı açıklayan temel bir iş akışı tanımına sahiptir. Bu iş akışı tanımı [JSON](https://www.json.org/) kullanır ve İş Akışı Tanımı Dili şeması tarafından doğrulanmış bir yapı izler. Bu başvuru, bu yapı ve şema iş akışı tanımında öznitelikleri nasıl tanımladığı hakkında genel bir bakış sağlar.

## <a name="workflow-definition-structure"></a>İş akışı tanım yapısı

İş akışı tanımı her zaman mantık uygulamanızı anında hale getiren bir tetikleyicinin yanı sıra tetikleyici yangınlarından sonra çalışan bir veya daha fazla eylem içerir.

İş akışı tanımı için üst düzey yapı aşağıda verilmiştir:

```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "actions": { "<workflow-action-definitions>" },
  "contentVersion": "<workflow-definition-version-number>",
  "outputs": { "<workflow-output-definitions>" },
  "parameters": { "<workflow-parameter-definitions>" },
  "staticResults": { "<static-results-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" }
}
```

| Öznitelik | Gerekli | Açıklama |
|-----------|----------|-------------|
| `definition` | Evet | İş akışı tanımınızın başlangıç öğesi |
| `$schema` | Yalnızca iş akışı tanımına dışarıdan başvururken | Burada bulabileceğiniz İş Akışı Tanımı Dili sürümünü açıklayan JSON şema dosyasının konumu: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json`</p> |
| `actions` | Hayır | İş akışı çalışma zamanında yürütülecek bir veya daha fazla eylemin tanımları. Daha fazla bilgi için [Tetikleyiciler ve eylemlere](#triggers-actions)bakın. <p><p>Maksimum eylem: 250 |
| `contentVersion` | Hayır | İş akışı tanımınızın varsayılan olarak "1.0.0.0" olan sürüm numarası. İş akışını dağıtırken doğru tanımı belirlemeye ve onaylamaya yardımcı olmak için kullanılacak bir değer belirtin. |
| `outputs` | Hayır | Çıktıların iş akışı çalışmasından döndürülecek tanımları. Daha fazla bilgi için [Çıktılar'a](#outputs)bakın. <p><p>Maksimum çıkışsayısı: 10 |
| `parameters` | Hayır | Mantık uygulamanızın çalışma zamanında kullanılacak değerleri geçen bir veya daha fazla parametrenin tanımları. Daha fazla bilgi için [Parametreler'e](#parameters)bakın. <p><p>Maksimum parametreler: 50 |
| `staticResults` | Hayır | Bir veya daha fazla statik sonuç için tanımlar, bu eylemlerde statik sonuçlar etkinleştirildiğinde, eylemler tarafından sahte çıktılar olarak döndürülür. Her eylem tanımında, `runtimeConfiguration.staticResult.name` öznitelik içindeki `staticResults`karşılık gelen tanıma başvurur. Daha fazla bilgi için [Statik sonuçlara](#static-results)bakın. |
| `triggers` | Hayır | Bir veya daha fazla tetikleyicinin tanımları iş akışınızı anında tetikler. Birden fazla tetikleyici tanımlayabilirsiniz, ancak yalnızca İş Akışı Tanım Dili ile değil, Mantıksal Uygulamalar Tasarımcısı aracılığıyla görsel olarak tanımlayabilirsiniz. Daha fazla bilgi için [Tetikleyiciler ve eylemlere](#triggers-actions)bakın. <p><p>Maksimum tetikleme: 10 |
||||

<a name="triggers-actions"></a>

## <a name="triggers-and-actions"></a>Tetikleyiciler ve eylemler

İş akışı tanımında, `triggers` `actions` iş akışıve bölümler, iş akışınızın yürütülmesi sırasında meydana gelen çağrıları tanımlar. Sözdizimi ve bu bölümler hakkında daha fazla bilgi için [İş Akışı tetikleyicileri ve eylemleri](../logic-apps/logic-apps-workflow-actions-triggers.md)bölümüne bakın.

<a name="parameters"></a>

## <a name="parameters"></a>Parametreler

Dağıtım yaşam döngüsü genellikle geliştirme, test, evreleme ve üretim için farklı ortamlara sahiptir. Mantık uygulamalarını çeşitli ortamlara dağıtırken, büyük olasılıkla dağıtım gereksinimlerinize bağlı olarak bağlantı dizeleri gibi farklı değerleri kullanmak isteyebilirsiniz. Veya, mantık uygulamanız boyunca hardcoding olmadan yeniden kullanmak istediğiniz veya sık sık değişen değerlere sahip olabilirsiniz. İş akışı tanımınızın `parameters` bölümünde, mantık uygulamanızın çalışma zamanında kullandığı değerler için parametrelertanımlayabilir veya edebilirsiniz. İş akışı tanımınızın başka bir yerinde bu parametreleri referans alamadan önce bu parametreleri tanımlamanız gerekir.

Parametre tanımının genel yapısı aşağıda verilmiştir:

```json
"parameters": {
   "<parameter-name>": {
      "type": "<parameter-type>",
      "defaultValue": <default-parameter-value>,
      "allowedValues": [ <array-with-permitted-parameter-values> ],
      "metadata": {
         "description": "<parameter-description>"
      }
   }
},
```

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| <*parametre adı*> | Evet | Dize | Tanımlamak istediğiniz parametrenin adı |
| <*parametre türü*> | Evet | int, float, string, bool, dizi, nesne, securestring, secureobject <p><p>**Not**: Tüm parolalar, anahtarlar ve `securestring` `secureobject` sırlar `GET` için, işlem bu türleri döndürmediği için veya türleri kullanın. Parametrelerin güvenliğini sağlama hakkında daha fazla bilgi [için, eylem ve giriş parametreleri için Güvenlik önerilerine](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)bakın. | Parametrenin türü |
| <*varsayılan parametre-değer*> | Evet | Aynı`type` | İş akışı anında olduğunda değer belirtilmemişse kullanılacak varsayılan parametre değeri. `defaultValue` Mantık Uygulama Tasarımcısı'nın parametreyi doğru şekilde gösterebilmeleri için öznitelik gereklidir, ancak boş bir değer belirtebilirsiniz. |
| <*dizi-izin verilen parametre-değerleri*> | Hayır | Dizi | Parametrenin kabul edebileceği değerlere sahip bir dizi |
| <*parametre açıklaması*> | Hayır | JSON nesnesi | Parametre nin açıklaması gibi diğer parametre ayrıntıları |
||||

Ardından, iş akışı tanımınız için bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/templates/overview.md) oluşturun, dağıtımda istediğiniz değerleri kabul eden şablon parametrelerini tanımlayın, şablon veya iş akışı tanımı parametrelerine yapılan başvurularla değiştirmeniz ve dağıtımda kullanılacak değerleri ayrı bir [parametre dosyasında](../azure-resource-manager/templates/parameter-files.md)depolar. Bu şekilde, mantık uygulamanızı güncelleştirmek ve yeniden dağıtmak zorunda kalmadan parametre dosyası üzerinden bu değerleri daha kolay değiştirebilirsiniz. Kullanıcı adları, parolalar ve sırlar gibi hassas veya güvenli olması gereken bilgiler için, bu değerleri Azure Key Vault'ta saklayabilir ve parametre dosyanızın bu değerleri anahtar kasanızdan almasına izin verebilirsiniz. Şablon ve iş akışı tanım düzeylerinde parametreleri tanımlama hakkında daha fazla bilgi ve örnekler için [bkz.](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)

<a name="static-results"></a>

## <a name="static-results"></a>Statik sonuçlar

Öznitelikte, `staticResults` eylemin sahtesini `outputs` ve `status` eylemin statik sonuç ayarı açık olduğunda eylemin geri döndüğünü tanımlayın. Eylemin tanımında, `runtimeConfiguration.staticResult.name` öznitelik içindeki `staticResults`statik sonuç tanımının adına başvurur. Statik sonuçlar [ayarlayarak mantıksal uygulamaları sahte verilerle](../logic-apps/test-logic-apps-mock-data-static-results.md)nasıl test edebileceğinizi öğrenin.

```json
"definition": {
   "$schema": "<...>",
   "actions": { "<...>" },
   "contentVersion": "<...>",
   "outputs": { "<...>" },
   "parameters": { "<...>" },
   "staticResults": {
      "<static-result-definition-name>": {
         "outputs": {
            <output-attributes-and-values-returned>,
            "headers": { <header-values> },
            "statusCode": "<status-code-returned>"
         },
         "status": "<action-status>"
      }
   },
   "triggers": { "<...>" }
}
```

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| <*statik-sonuç tanımı-adı*> | Evet | Dize | Eylem tanımının bir `runtimeConfiguration.staticResult` nesne aracılığıyla başvurulabileceği statik sonuç tanımının adı. Daha fazla bilgi için [Runtime yapılandırma ayarlarına](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-config-options)bakın. <p>İstediğiniz benzersiz adı kullanabilirsiniz. Varsayılan olarak, bu benzersiz ad, gerektiği gibi artımlı bir sayı ile eklenir. |
| <*çıktı öznitelikleri-ve-values-döndürülen*> | Evet | Değişir | Bu öznitelikleriçin gereksinimler farklı koşullara bağlı olarak değişir. Örneğin, `status` öznitelik, `Succeeded` `outputs` eylem tarafından sahte çıktılar olarak döndürülen öznitelikleri ve değerleri içerir. `status` Ise, `Failed` `outputs` öznitelik hata bilgisi `errors` olan bir veya daha fazla hata `message` nesneleri ile bir dizi öznitelik içerir. |
| <*üstbilgi değerleri*> | Hayır | JSON | Eylem tarafından döndürülen üstbilgi değerleri |
| <*durum kodu döndürülen*> | Evet | Dize | Eylem tarafından döndürülen durum kodu |
| <*eylem durumu*> | Evet | Dize | Örneğin, eylemin durumu `Succeeded` veya`Failed` |
|||||

Örneğin, bu HTTP eylem tanımında, `runtimeConfiguration.staticResult.name` `HTTP0` eylem `staticResults` için sahte çıktıların tanımlandığı öznitelik içindeki öznitelik başvuruları. Öznitelik, `runtimeConfiguration.staticResult.staticResultOptions` statik sonuç ayarının HTTP `Enabled` eyleminde olduğunu belirtir.

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.microsoft.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "staticResult": {
            "name": "HTTP0",
            "staticResultOptions": "Enabled"
         }
      },
      "type": "Http"
   }
},
```

HTTP eylemi, içindeki `HTTP0` `staticResults`tanımdaki çıktıları döndürür. Bu örnekte, durum kodu için, `OK`sahte çıktı . Üstbilgi değerleri için, sahte `"Content-Type": "application/JSON"`çıktı. Eylemin durumu için, sahte `Succeeded`çıktı.

```json
"definition": {
   "$schema": "<...>",
   "actions": { "<...>" },
   "contentVersion": "<...>",
   "outputs": { "<...>" },
   "parameters": { "<...>" },
   "staticResults": {
      "HTTP0": {
         "outputs": {
            "headers": {
               "Content-Type": "application/JSON"
            },
            "statusCode": "OK"
         },
         "status": "Succeeded"
      }
   },
   "triggers": { "<...>" }
},
```

<a name="expressions"></a>

## <a name="expressions"></a>İfadeler

JSON ile, tasarım zamanında var olan gerçek değerlere sahip olabilirsiniz, örneğin:

```json
"customerName": "Sophia Owen",
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"],
"rainbowColorsCount": 7
```

Çalışma süresine kadar var olmayan değerlere de sahip olabilirsiniz. Bu değerleri temsil etmek için, çalışma zamanında değerlendirilen *ifadeleri*kullanabilirsiniz. İfade, bir veya daha fazla [işlev,](#functions) [işleç,](#operators)değişken, açık değerler veya sabitler içerebilen bir dizidir. İş akışı tanımınızda, ifadeyi at işareti ( ile\@öne tutarak json dize değerinin herhangi bir yerinde bir ifade kullanabilirsiniz. JSON değerini temsil eden bir ifade değerlendirilirken, ifade gövdesi \@ karakteri kaldırarak ayıklanır ve her zaman başka bir JSON değeriyle sonuçlanır.

Örneğin, daha önce tanımlanmış `customerName` özellik için, bir ifadedeki [parametreler()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) işlevini kullanarak özellik değerini alabilir `accountName` ve bu değeri özelliğe atayabilirsiniz:

```json
"customerName": "Sophia Owen",
"accountName": "@parameters('customerName')"
```

*String enterpolasyonu* da \@ karakter ve kıvırcık parantez tarafından sarılmış dizeleri içinde{}birden çok ifade kullanmanıza olanak sağlar ( ). İşte sözdizimi:

```json
@{ "<expression1>", "<expression2>" }
```

Sonuç her zaman bir dize, bu `concat()` yeteneği işlevine benzer hale, örneğin: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Karakterle başlayan gerçek bir dizeniz varsa, karakteri kaçış \@ karakteri olarak başka bir karakterle önegetirin: \@ \@\@\@

Bu örnekler ifadelerin nasıl değerlendirildiğini gösterir:

| JSON değeri | Sonuç |
|------------|--------|
| "Sophia Owen" | Bu karakterleri geri ver: 'Sophia Owen' |
| "dizi[1]" | Bu karakterleri döndürün: 'array[1]' |
| "\@\@" | Bu karakterleri tek karakterli bir dize olarak döndürün: ' '\@ |
| " \@" | Bu karakterleri iki karakterli bir dize olarak döndürün: ' ' \@ |
|||

Bu örnekler için, "myBirthMonth" "Ocak" ve "myAge" sayısı 42 eşit eşit tanımlar varsayalım:

```json
"myBirthMonth": "January",
"myAge": 42
```

Bu örnekler, aşağıdaki ifadelerin nasıl değerlendirildiğini gösterir:

| JSON ifadesi | Sonuç |
|-----------------|--------|
| "\@parametreleri('myBirthMonth')" | Bu dizeyi döndür: "Ocak" |
| "\@{parameters('myBirthMonth')}" | Bu dizeyi döndür: "Ocak" |
| "\@parametreleri('myAge')" | Bu numarayı iade edin: 42 |
| "\@{parameters('myAge')}" | Bu sayıyı dize olarak döndür: "42" |
| "Benim yaşım \@{parameters('myAge')}" | Bu dize dön: "Benim yaş 42" |
| "\@concat('Benim yaşım ', string('myAge'))) | Bu dize dön: "Benim yaş 42" |
| "Benim yaşım \@ \@{parameters('myAge')}" | İfadeyi içeren bu dizeyi döndürün: "Yaşım \@{parameters('myAge')}' |
|||

Logic Apps Designer'da görsel olarak çalışırken, Örneğin İfade oluşturucu aracılığıyla ifadeler oluşturabilirsiniz:

![Mantık Uygulamaları Tasarımcı > İfade oluşturucu](./media/logic-apps-workflow-definition-language/expression-builder.png)

İş bittiğinde, ifade iş akışı tanımınızda ilgili özellik için görünür, örneğin, buradaki `searchQuery` özellik:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
        "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="outputs"></a>

## <a name="outputs"></a>Çıkışlar

`outputs` Bölümde, çalışma tamamlandığında iş akışınızın döndürebileceği verileri tanımlayın. Örneğin, her çalıştırmadan belirli bir durumu veya değeri izlemek için, iş akışı çıktısının bu verileri döndürür belirtin.

> [!NOTE]
> Bir hizmetin REST API'sinden gelen isteklere yanıt `outputs`verirken, kullanmayın. Bunun yerine, `Response` eylem türünü kullanın. Daha fazla bilgi için [Bkz. İş Akışı tetikleyicileri ve eylemleri.](../logic-apps/logic-apps-workflow-actions-triggers.md)

Çıktı tanımının genel yapısı aşağıda verilmiştir:

```json
"outputs": {
  "<key-name>": {
    "type": "<key-type>",
    "value": "<key-value>"
  }
}
```

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| <*anahtar adı*> | Evet | Dize | Çıktı iade değeri için anahtar ad |
| <*anahtar türü*> | Evet | int, float, string, securestring, bool, dizi, JSON nesne | Çıktı iade değeri türü |
| <*anahtar değeri*> | Evet | <*anahtar türüyle* aynı> | Çıktı iade değeri |
|||||

Bir iş akışı çalışmasından çıktı almak için, mantıksal uygulamanızın çalışma geçmişini ve ayrıntılarını Azure portalında gözden geçirin veya [İş Akışı REST API'sini](https://docs.microsoft.com/rest/api/logic/workflows)kullanın. Ayrıca, panel oluşturabilmeniz için çıktıyı harici sistemlere (örneğin Power BI) geçirebilirsiniz.

<a name="operators"></a>

## <a name="operators"></a>İşleçler

İfadelerde ve [işlevlerde, işleçler](#expressions) bir özellik veya bir dizideki değer gibi belirli görevleri gerçekleştirir. [functions](#functions)

| İşleç | Görev |
|----------|------|
| ' | Giriş olarak veya ifadeler ve işlevler olarak bir dize kullanmak için, dize yi `'<myString>'`yalnızca tek tırnak işaretleriyle sarın, örneğin. Tüm ifadenin etrafında JSON biçimlendirmesiyle çakışan çift tırnak işaretleri ("") kullanmayın. Örnek: <p>**Evet**: uzunluk('Merhaba') </br>**No**: uzunluk("Merhaba") <p>Dizileri veya sayıları geçtiğinde, noktalama işaretlerini tamamlamanız gerekmez. Örnek: <p>**Evet**: uzunluk([1, 2, 3]) </br>**No**: uzunluk("[1, 2, 3]") |
| [] | Dizideki belirli bir konumda (dizin) bir değere başvurmak için kare ayraçları kullanın. Örneğin, bir dizideki ikinci öğeyi almak için: <p>`myArray[1]` |
| . | Bir nesnedeki bir özelliğe başvurmak için nokta işlecikullanın. Örneğin, bir `name` `customer` JSON nesnesinin özelliğini almak için: <p>`"@parameters('customer').name"` |
| ? | Çalışma zamanı hatası olmayan bir nesnedeki null özelliklerine başvurmak için soru işareti işlecikullanın. Örneğin, bir tetikleyiciden null çıktıları işlemek için şu ifadeyi kullanabilirsiniz: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` |
|||

<a name="functions"></a>

## <a name="functions"></a>İşlevler

Bazı ifadeler, iş akışı tanımınız çalışmaya başladığında henüz var olmayan çalışma zamanı eylemlerinden değerlerini alır. İfadelerde bu değerlere başvuru yapmak veya bu değerlerle çalışmak için İş Akışı Tanım Dili'nin sağladığı [*işlevleri*](../logic-apps/workflow-definition-language-functions-reference.md) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* İş [Akışı Tanımı Dil eylemleri ve tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md) hakkında bilgi edinin
* [İş Akışı REST API](https://docs.microsoft.com/rest/api/logic/workflows) ile mantık uygulamalarını programlı olarak oluşturma ve yönetme hakkında bilgi edinin
