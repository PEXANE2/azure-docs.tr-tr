---
title: JSON ve XML 'yi likit şablonlar ile dönüştürme
description: Azure Logic Apps 'de haritalar olarak sıvı şablonları kullanarak JSON ve XML dönüştürme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, daviburg, logicappspm
ms.topic: article
ms.date: 07/31/2020
ms.openlocfilehash: b3919cbbe0ba7a796a21ae566afb8e2d9fa784db
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88716682"
---
# <a name="transform-json-and-xml-using-liquid-templates-as-maps-in-azure-logic-apps"></a>Azure Logic Apps 'de haritalar olarak sıvı şablonları kullanarak JSON ve XML dönüştürme

Mantıksal uygulamalarınızda temel JSON dönüştürmeleri gerçekleştirmek istediğinizde, JSON **oluşturma** veya **ayrıştırma**gibi yerel [veri işlemlerini](../logic-apps/logic-apps-perform-data-operations.md) kullanabilirsiniz. Yinelemeler, denetim akışları ve değişkenler gibi öğelere sahip olan JSON dönüşümlerine gelişmiş ve karmaşık JSON için, [sıvı](https://shopify.github.io/liquid/) açık kaynak şablon dilini kullanarak bu dönüşümleri tanımlayan şablonlar oluşturun ve kullanın. Ayrıca, örneğin JSON-Text, XML-JSON ve XML 'den metne [diğer dönüşümler de gerçekleştirebilirsiniz](#other-transformations).

Mantıksal uygulamanızda bir sıvı dönüştürmesi gerçekleştirebilmek için önce istediğiniz eşlemeyi tanımlayan bir likit şablon oluşturmanız gerekir. Ardından, şablonu [tümleştirme hesabınıza](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) [bir eşleme olarak yüklersiniz](../logic-apps/logic-apps-enterprise-integration-maps.md) . Mantıksal uygulamanıza **JSON 'Yı JSON-likit** eylemini ekleyerek, kullanılacak eyleme yönelik harita olarak sıvı şablonu ' nu seçebilirsiniz.

Bu makalede, bu görevlerin nasıl tamamlanacağı gösterilmektedir:

* Bir likit şablon oluşturun.
* Şablonu tümleştirme hesabınıza ekleyin.
* Mantıksal uygulamanıza likit dönüştürme eylemi ekleyin.
* Kullanmak istediğiniz eşleme olarak şablonu seçin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* [Tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* [Sıvı şablonu dili](https://shopify.github.io/liquid/) hakkında temel bilgi

  > [!NOTE]
  > **JSON 'DAN JSON 'A dönüştürme-sıvı** eylemi, sıvı için [Shopify uygulamasının](https://shopify.github.io/liquid)belirli durumlarında farklı olan [sıvı için dotlikit uygulamasını](https://github.com/dotliquid/dotliquid)izler. Daha fazla bilgi için bkz. [sıvı şablonu konuları](#liquid-template-considerations).

## <a name="create-the-template"></a>Şablonu oluşturma

1. JSON dönüştürmesi için eşleme olarak kullandığınız likit şablonu oluşturun. İstediğiniz herhangi bir düzenleyici aracı kullanabilirsiniz.

   Bu örnek için, bu bölümde açıklandığı gibi örnek likit şablonu oluşturun:

   ```json
   {%- assign deviceList = content.devices | Split: ', ' -%}

   {
      "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
      "firstNameUpperCase": "{{content.firstName | Upcase}}",
      "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
      "devices" : [
         {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
         {%- endfor -%}
      ]
   }
   ```

1. Uzantıyı kullanarak şablonu kaydedin `.liquid` . Bu örnekte, kullanılır `SimpleJsonToJsonTemplate.liquid` .

## <a name="upload-the-template"></a>Şablonu karşıya yükle

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portal arama kutusuna `integration accounts` , **tümleştirme hesapları**' nı girin ve seçin.

   !["Tümleştirme hesaplarını" bul](./media/logic-apps-enterprise-integration-liquid-transform/find-integration-accounts.png)

1. Tümleştirme hesabınızı bulun ve seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

1. **Genel bakış** bölmesinde, **Bileşenler**altında **haritalar**' ı seçin.

    !["Haritalar" kutucuğunu seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-maps-tile.png)

1. **Haritalar** bölmesinde **Ekle** ' yi seçin ve Haritalarınız için bu ayrıntıları sağlayın:

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Ad** | `JsonToJsonTemplate` | Bu örnekteki "JsonToJsonTemplate" olan haritaınızın adı |
   | **Eşleme türü** | **sıvı** | Haritalarınızın türü. JSON-JSON dönüştürmesi için **likit**' i seçmeniz gerekir. |
   | **Harita** | `SimpleJsonToJsonTemplate.liquid` | Bu örnekte "SimpleJsonToJsonTemplate. sıvı" olan dönüştürme için kullanılacak mevcut bir likit şablon veya eşleme dosyası. Bu dosyayı bulmak için dosya seçiciyi kullanabilirsiniz. Harita boyutu sınırları için bkz. [sınırlara ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits). |
   |||

   ![Likit Şablon Ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

## <a name="add-the-liquid-transformation-action"></a>Sıvı dönüştürme eylemini ekleme

1. Azure portal [boş bir mantıksal uygulama oluşturmak](../logic-apps/quickstart-create-first-logic-app-workflow.md)için aşağıdaki adımları izleyin.

1. Mantıksal uygulama Tasarımcısı ' nda, mantıksal uygulamanıza [istek tetikleyiciyi](../connectors/connectors-native-reqres.md#add-request) ekleyin.

1. Tetikleyici altında **yeni adım**' ı seçin. Arama kutusuna `liquid` filtreniz olarak girin ve şu eylemi seçin: **JSON 'u JSON 'a Dönüştür-sıvı**

   ![Sıvı eylemini bul ve Seç](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

1. **Harita** listesini açın ve bu örnekteki "JsonToJsonTemplate" olan sıvı şablonunuzu seçin.

   ![Eşleme Seç](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Haritalar listesi boşsa, büyük olasılıkla mantıksal uygulamanız tümleştirme hesabınıza bağlı değildir. 
   Mantıksal uygulamanızı likit şablon veya eşleme içeren tümleştirme hesabına bağlamak için şu adımları izleyin:

   1. Mantıksal uygulama menünüzde **Iş akışı ayarları**' nı seçin.

   1. **Tümleştirme hesabı seçin** listesinden tümleştirme hesabınızı seçin ve **Kaydet**' i seçin.

      ![Mantıksal uygulamayı tümleştirme hesabına bağlama](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

1. Şimdi bu eyleme **içerik** özelliğini ekleyin. **Yeni parametre Ekle** listesini açın ve **içerik**' i seçin.

   ![Eyleme "Içerik" özelliği Ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-content-property-to-action.png)

1. **İçerik** özellik değerini ayarlamak Için, **içerik** kutusunun içine tıklayarak dinamik içerik listesinin görünmesini sağlayın. Tetikleyiciden gelen gövde içerik çıkışını temsil eden **gövde** belirtecini seçin.

   !["Içerik" özelliği değeri için "gövde" belirtecini seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)

   İşlem tamamlandığında eylem şu örnekteki gibi görünür:

   !["JSON 'dan JSON 'a dönüştürme" eylemi bitti](./media/logic-apps-enterprise-integration-liquid-transform/finished-transform-action.png)

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

[Postman](https://www.getpostman.com/postman) veya benzer bir aracı kullanarak, MANTıKSAL uygulamanıza JSON girişi gönderin. Mantıksal uygulamanızdan dönüştürülmüş JSON çıktısı şu örneğe benzer şekilde görünür:

![Örnek çıkış](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontojson.png)

<a name="template-considerations"></a>

## <a name="liquid-template-considerations"></a>Sıvı şablonu konuları

* Likit şablonlar, Azure Logic Apps [haritalar için dosya boyutu sınırlarını](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits) izler.

* **JSON 'DAN JSON 'A dönüştürme-sıvı** eylemi, [sıvı için dotlikit uygulamasını](https://github.com/dotliquid/dotliquid)izler. Bu uygulama, [sıvı Shopify uygulamasındaki](https://shopify.github.io/liquid/) .NET Framework bir bağlantı noktasıdır ve [belirli durumlarda](https://github.com/dotliquid/dotliquid/issues)farklılık gösterir.

  Bilinen farklar şunlardır:

  * JSON 'dan **JSON 'A dönüştürme-sıvı** EYLEMI, JSON, XML, HTML vb. içerebilen bir dizeyi yerel olarak verir. Sıvı eylemi yalnızca sıvı şablonundan beklenen metin çıkışının bir JSON dizesi olduğunu gösterir. Bu eylem mantıksal uygulamanızı girişi bir JSON nesnesi olarak ayrıştırmasını ve likit 'in JSON yapısını yorumlayabilmesi için bir sarmalayıcı uygular. Dönüşümden sonra eylem, mantıksal uygulamanızı, sıvı geri 'den JSON 'a doğru metin çıkışını ayrıştırmasını söyler.

    Dotlikit, JSON 'u yerel olarak anlamıyor, bu nedenle ters eğik çizgi karakteri ( `\` ) ve diğer tüm ayrılmış JSON karakterlerini attığınızdan emin olun.

  * Şablonunuz [likit filtreler](https://shopify.github.io/liquid/basics/introduction/#filters)kullanıyorsa, cümle büyük/ *küçük harf*kullanan [dotlikit ve C# adlandırma kurallarını](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers#filter-and-output-casing)izlediğinizden emin olun. Tüm sıvı dönüştürmeleri için, şablonunuzda filtre adlarında Ayrıca tümce büyük/küçük harf de kullandığınızdan emin olun. Aksi takdirde, filtreler çalışmaz.

    Örneğin, filtresini kullandığınızda, öğesini `replace` kullanın `Replace` `replace` . Aynı kural, [Dotsıvı online](http://dotliquidmarkup.org/try-online)'da örnekleri denerseniz geçerli olur. Daha fazla bilgi için bkz. [Shopify sıvı Filters](https://shopify.dev/docs/themes/liquid/reference/filters) ve [dotsıvı sıvı Filters](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Developers#create-your-own-filters). Shopify belirtimi her bir filtreye yönelik örnekleri içerir, bu nedenle karşılaştırma için bu örnekleri Dotlikit ' de deneyebilirsiniz [-çevrimiçi deneyin](https://dotliquidmarkup.org/try-online).

  * `json`Shopify uzantı filtrelerinden gelen filtre Şu anda [dotlikit içinde uygulanmıyor](https://github.com/dotliquid/dotliquid/issues/384). Genellikle, bu filtreyi JSON dize ayrıştırması için metin çıktısı hazırlamak üzere kullanabilirsiniz, ancak bunun yerine filtreyi kullanmanız gerekir `Replace` .

  * `Replace` [Dotlikit](https://github.com/dotliquid/dotliquid/blob/b6a7d992bf47e7d7dcec36fb402f2e0d70819388/src/DotLiquid/StandardFilters.cs#L425) uygulamasındaki standart filtre, [normal ifade (Regex) eşleştirmeyi](/dotnet/standard/base-types/regular-expression-language-quick-reference)kullanır, ancak [Shopify uygulamasının](https://shopify.github.io/liquid/filters/replace/) [basit dize eşlemesi](https://github.com/Shopify/liquid/issues/202)kullanır. Her iki uygulama da, bir RegEx ayrılmış karakteri ya da Match parametresinde bir kaçış karakteri kullanana kadar aynı şekilde çalışır.

    Örneğin, RegEx ile ayrılmış ters eğik çizgi ( `\` ) kaçış karakterini atlamak için, `| Replace: '\\', '\\'` ve kullanın `| Replace: '\', '\\'` . Bu örnekler, `Replace` ters eğik çizgiden çıkmak istediğinizde filtrenin farklı davrandığını gösterir. Bu sürüm başarıyla çalışırken:

    `{ "SampleText": "{{ 'The quick brown fox "jumped" over the sleeping dog\\' | Replace: '\\', '\\' | Replace: '"', '\"'}}"}`

    Bu sonuçla:

    `{ "SampleText": "The quick brown fox \"jumped\" over the sleeping dog\\\\"}`

    Bu sürüm başarısız olur:

    `{ "SampleText": "{{ 'The quick brown fox "jumped" over the sleeping dog\\' | Replace: '\', '\\' | Replace: '"', '\"'}}"}`

    Bu hatayla:

    `{ "SampleText": "Liquid error: parsing "\" - Illegal \ at end of pattern."}`

    Daha fazla bilgi için bkz [. Replace standart filtresi Regex model eşleştirmeyi kullanır...](https://github.com/dotliquid/dotliquid/issues/385)

  * `Sort` [Dotlikit uygulamasındaki](https://github.com/dotliquid/dotliquid/blob/b6a7d992bf47e7d7dcec36fb402f2e0d70819388/src/DotLiquid/StandardFilters.cs#L326) filtre, bir dizi veya koleksiyondaki öğeleri özelliğe göre sıralar, ancak şu farklılıklar vardır:<p>

    * [Shopify 'in sıralama davranışını](https://shopify.github.io/liquid/filters/sort/)değil, [Shopify sort_natural davranışını](https://shopify.github.io/liquid/filters/sort_natural/)izler.

    * Yalnızca dize alfasayısal düzeninde sıralar. Daha fazla bilgi için bkz. [sayısal sıralama](https://github.com/Shopify/liquid/issues/980).

    * Büyük/küçük harfe duyarlı sıra değil, *büyük/küçük harfe duyarsız* sırayı kullanır. Daha fazla bilgi için bkz. [sıralama filtresi, Shopify 'ın belirtiminden büyük/küçük harf davranışını izlemez]( https://github.com/dotliquid/dotliquid/issues/393).

<a name="other-transformations"></a>

## <a name="other-transformations-using-liquid"></a>Sıvı kullanan diğer dönüşümler

Sıvı yalnızca JSON dönüşümlerine sınırlı değildir. Ayrıca, diğer dönüştürmeleri gerçekleştirmek için likit de kullanabilirsiniz. Örneğin:

* [JSON-metin](#json-text)
* [XML-JSON](#xml-json)
* [XML 'den metne](#xml-text)

<a name="json-text"></a>

### <a name="transform-json-to-text"></a>JSON 'ı metne Dönüştür

Bu örnek için kullanılan likit şablon aşağıda verilmiştir:

```json
{{content.firstName | Append: ' ' | Append: content.lastName}}
```

Örnek girişler ve çıktılar aşağıda verilmiştir:

![Örnek çıkış JSON-Text](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontotext.png)

<a name="xml-json"></a>

### <a name="transform-xml-to-json"></a>XML 'i JSON 'ye Dönüştür

Bu örnek için kullanılan likit şablon aşağıda verilmiştir:

``` json
[{% JSONArrayFor item in content -%}
      {{item}}
  {% endJSONArrayFor -%}]
```

`JSONArrayFor`Döngü, sondaki virgülden KAÇıNACAK JSON yükleri oluşturabilmeniz IÇIN XML girişi için özel bir döngü mekanizmasıdır. Ayrıca, `where` Bu özel döngü mekanizmasının koşulu, diğer likit filtreleri gibi öğenin değeri yerine karşılaştırma IÇIN XML öğesinin adını kullanır. Daha fazla bilgi için bkz. [Set-Body Policy-nesnelerin koleksiyonları](https://azure.microsoft.com/blog/deep-dive-on-set-body-policy).

Örnek girişler ve çıktılar aşağıda verilmiştir:

![Örnek çıkış XML-JSON](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltojson.png)

<a name="xml-text"></a>

### <a name="transform-xml-to-text"></a>XML 'e metin dönüştürme

Bu örnek için kullanılan likit şablon aşağıda verilmiştir:

``` json
{{content.firstName | Append: ' ' | Append: content.lastName}}
```

Örnek girişler ve çıktılar aşağıda verilmiştir:

![Örnek çıkış XML-Text](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltotext.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Shopify sıvı dili ve örnekleri](https://shopify.github.io/liquid/basics/introduction/)
* [Dotlikit](http://dotliquidmarkup.org/)
* [Dotsıvı-çevrimiçi deneyin](https://dotliquidmarkup.org/try-online)
* [Dotsıvı GitHub](https://github.com/dotliquid/dotliquid)
* [Dotlikit GitHub sorunları](https://github.com/dotliquid/dotliquid/issues/)
* [Haritalar](../logic-apps/logic-apps-enterprise-integration-maps.md) hakkında daha fazla bilgi edinin
