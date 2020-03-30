---
title: Veriler üzerinde işlem yapma
description: Azure Logic Apps'ta veri çıktılarını ve biçimlerini dönüştürme, yönetme ve işleme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: baa6e5732221d120ff71217a3a86a942794c53f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283946"
---
# <a name="perform-data-operations-in-azure-logic-apps"></a>Azure Logic Apps'ta veri işlemleri gerçekleştirin

Bu makalede, bu görevler ve daha fazlası için eylemler ekleyerek mantık uygulamalarınızdaki verilerle nasıl çalışabileceğiniz gösterilmektedir:

* Dizilerden tablolar oluşturun.
* Bir koşula dayalı olarak diğer dizilerden diziler oluşturun.
* JavaScript Nesne Gösterimi (JSON) nesne özelliklerinden kullanıcı dostu belirteçler oluşturun, böylece bu özellikleri iş akışınızda kolayca kullanabilirsiniz.

İstediğini burada bulamazsanız, Azure Logic Apps'ın sağladığı çeşitli [veri işleme işlevlerine](../logic-apps/workflow-definition-language-functions-reference.md) göz atmayın.

Bu tablolar, kullanabileceğiniz veri işlemlerini özetler ve işlemlerin üzerinde çalıştığı kaynak veri türlerine göre düzenlenir, ancak her açıklama alfabetik olarak görüntülenir.

**Dizi eylemleri** 

Bu eylemler, diziler halindeki verilerle çalışmanıza yardımcı olur.

| Eylem | Açıklama |
|--------|-------------|
| [**CSV tablosu oluşturma**](#create-csv-table-action) | Bir diziden virgülle ayrılmış bir değer (CSV) tablosu oluşturun. |
| [**HTML tablosu oluşturma**](#create-html-table-action) | Bir diziden bir HTML tablosu oluşturun. |
| [**Filtre dizisi**](#filter-array-action) | Belirtilen filtreyi veya koşulu temel alan bir diziden bir dizi alt kümesi oluşturun. |
| [**Birleştir**](#join-action) | Bir dizideki tüm öğelerden bir dize oluşturun ve her öğeyi belirtilen karakterle ayırın. |
| [**Seçin**](#select-action) | Farklı bir dizideki tüm öğeler için belirtilen özelliklerden bir dizi oluşturun. |
||| 

**JSON eylemleri**

Bu eylemler, JavaScript Nesne Gösterimi (JSON) biçimindeki verilerle çalışmanıza yardımcı olur.

| Eylem | Açıklama |
|--------|-------------|
| [**Oluşturmak**](#compose-action) | Çeşitli veri türlerine sahip birden çok girişten bir ileti veya dize oluşturun. Daha sonra bu dizeyi, aynı girişleri tekrar tekrar girmek yerine tek bir giriş olarak kullanabilirsiniz. Örneğin, çeşitli girişlerden tek bir JSON iletisi oluşturabilirsiniz. |
| [**Ayrışdırık JSON**](#parse-json-action) | JSON içeriğindeki özellikler için kullanıcı dostu veri belirteçleri oluşturun, böylece mantık uygulamalarınızdaki özellikleri daha kolay kullanabilirsiniz. |
|||

Daha karmaşık JSON dönüşümleri oluşturmak için [bkz.](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Verilerle çalışmak için operasyona ihtiyaç duyduğunuz mantık uygulaması

  Mantık uygulamalarında yeniyseniz, [Azure Mantık Uygulamaları nedir?ve](../logic-apps/logic-apps-overview.md) [Quickstart'ı inceleyin: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Mantık uygulamanızın ilk adımı olarak bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) 

  Veri işlemleri yalnızca eylem olarak kullanılabilir, bu nedenle bu eylemleri kullanmadan önce mantık uygulamanızı bir tetikleyiciyle başlatın ve istediğiniz çıktıları oluşturmak için gereken diğer eylemleri ekleyin.

<a name="compose-action"></a>

## <a name="compose-action"></a>Eylem oluşturma

Birden çok girişten JSON nesnesi gibi tek bir çıktı oluşturmak için **Oluşturma** eylemini kullanabilirsiniz. Girişleriniz, tamsayılar, Booleans, diziler, JSON nesneleri ve Azure Logic Apps'ın desteklediği ikili ve XML gibi diğer yerel türlere sahip olabilir. Daha sonra, **Oluşturma** eyleminden sonra çıktıyı izleyen eylemlerde kullanabilirsiniz. **Oluşturma** eylemi, mantık uygulamanızın iş akışını oluştururken aynı girişleri tekrar tekrar girmenizi de sağlayabilir.

Örneğin, insanların adlarını ve soyadlarını depolayan dize değişkenleri gibi birden çok değişkenden json iletisi ve kişilerin yaşlarını depolayan bir tamsayı değişkeni oluşturabilirsiniz. Burada, **Oluştur** eylemi şu girdileri kabul eder:

`{ "age": <ageVar>, "fullName": "<lastNameVar>, <firstNameVar>" }`

ve bu çıktıyı oluşturur:

`{"age":35,"fullName":"Owens,Sophie"}`

Bir örnek denemek için, Mantık Uygulama Tasarımcısı'nı kullanarak aşağıdaki adımları izleyin. Veya kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, bu makaledeki değişken eylem tanımlarını **oluştur** ve **başlangıç** örneğini kendi mantık uygulamanızın temel iş akışı tanımına kopyalayabilirsiniz: [Veri işlem kodu örnekleri - Oluşturma](../logic-apps/logic-apps-data-operations-code-samples.md#compose-action-example) 

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi olan bir mantık uygulaması ve birkaç **Initialize değişken** eylemi kullanır. Bu eylemler, iki dize değişkeni ve bir bir arastıcı değişken oluşturmak için ayarlanır. Daha sonra mantık uygulamanızı test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   !["Oluşturma" eylemi için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-compose-action.png)

1. Çıktıyı oluşturmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin: 

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     !["Oluştur" eylemi için "Yeni adım" seçeneğini belirleyin](./media/logic-apps-perform-data-operations/add-compose-operation-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Bir **eylem seçin**altında, arama `compose` kutusuna filtreniz olarak girin. Eylemler listesinden Eylem **Oluştur'u** seçin.

   !["Oluştur" eylemini seçin](./media/logic-apps-perform-data-operations/select-compose-action.png)

1. **Girişler** kutusunda, çıktı oluşturmak için istediğiniz girişleri sağlayın.

   Bu örnekte, **Girişkutusu'nun** içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece önceden oluşturulmuş değişkenleri seçebilirsiniz:

   !["Oluşturma" eylemi için kullanılacak girişleri seçin](./media/logic-apps-perform-data-operations/configure-compose-action.png)

   Burada bitmiş örnek **Oluşturma** eylem: 

   !["Oluşturma" eylemi için bitmiş örnek](./media/logic-apps-perform-data-operations/finished-compose-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Temel iş akışı tanımınızda bu eylem hakkında daha fazla bilgi [için, Eylemi Oluştur'a](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action)bakın.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

**Oluştur** eyleminin beklenen sonuçları oluşturup oluşturmadığını doğrulamak için, kendinize **Oluşturma** eyleminden çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **Oluşturma** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında, **Oluşturma** eylemi altında **Çıktı'yı**seçin.

   Bu örnek, **e-posta gönder** eylemini kullanır ve e-postanın gövdesinde ve konusundaki **Çıktı** alanlarını içerir:

   !["Oluştur" eylemi için "Çıktı" alanları](./media/logic-apps-perform-data-operations/send-email-compose-action.png)

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["Oluştur" eylem sonuçları içeren e-posta](./media/logic-apps-perform-data-operations/compose-email-results.png)

<a name="create-csv-table-action"></a>

## <a name="create-csv-table-action"></a>CSV tablo eylemi oluşturma

Bir dizideki JavaScript Nesne Gösterimi (JSON) nesnelerinin özelliklerini ve değerlerini içeren virgülle ayrılmış bir değer (CSV) tablosu oluşturmak için **CSV tablo oluşturma** eylemini kullanın. Ardından, **CSV tablo** eylemi oluştur'u izleyen eylemlerde ortaya çıkan tabloyu kullanabilirsiniz.

Kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, **csv tablosu oluştur** ve değişken eylem tanımlarını bu makaleden kendi mantık uygulamanızın temel iş akışı tanımına **kopyalayabilirsiniz:** Veri işlem kodu örnekleri - [CSV tablosu oluştur](../logic-apps/logic-apps-data-operations-code-samples.md#create-csv-table-action-example)

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi ve **Initialize değişken** eylemi içeren bir mantık uygulaması kullanır. Eylem, ilk değeri JSON biçiminde bazı özellikleri ve değerleri olan bir dizi olan bir değişken oluşturmak için ayarlanır. Daha sonra mantık uygulamanızı test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   !["CSV tablosu oluşturma" eylemi için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

1. CSV tablosunu oluşturmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin: 

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     !["CSV tablosu oluşturma" eylemi için "Yeni adım" seçeneğini belirleyin](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Bir **eylem seçin**altında, arama `create csv table` kutusuna filtreniz olarak girin. Eylemler listesinden **CSV tablo eylemi oluştur'u** seçin.

   !["CSV tablosu oluşturma" eylemini seçin](./media/logic-apps-perform-data-operations/select-create-csv-table-action.png)

1. **Gönderen** kutusunda, tabloyu oluşturmak için istediğiniz diziyi veya ifadeyi sağlayın.

   Bu örnekte, **Gönderen** kutusunun içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece önceden oluşturulmuş değişkeni seçebilirsiniz:

   ![CSV tablosu oluşturmak için dizi çıktısını seçin](./media/logic-apps-perform-data-operations/configure-create-csv-table-action.png)

   > [!TIP]
   > JSON nesnelerindeki özellikler için kullanıcı dostu belirteçler oluşturmak için bu özellikleri giriş olarak seçebilirsiniz, **CSV tablo** eylemi oluşturmadan önce [Parse JSON'ı](#parse-json-action) kullanın.

   Burada bitmiş örnek **CSV tablo** eylem oluşturun: 

   !["CSV tablosu oluşturma" eylemi için bitmiş örnek](./media/logic-apps-perform-data-operations/finished-create-csv-table-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="customize-table-format"></a>Tablo biçimini özelleştirme

Varsayılan olarak, **Sütunlar** özelliği dizi öğelerini temel alan tablo sütunlarını otomatik olarak oluşturmak üzere ayarlanır. Özel üstbilgi ve değerleri belirtmek için aşağıdaki adımları izleyin:

1. **Sütunlar** listesini açın ve **Özel'i**seçin.

1. **Üstbilgi** özelliğinde, bunun yerine kullanılacak özel üstbilgi metnini belirtin.

1. **Değer** özelliğinde, bunun yerine kullanılacak özel değeri belirtin.

Diziden değerleri döndürmek için, [ `item()` ](../logic-apps/workflow-definition-language-functions-reference.md#item) **CSV tablo** eylemi oluştur işlevini kullanabilirsiniz. Bir `For_each` döngüde, [ `items()` işlevi](../logic-apps/workflow-definition-language-functions-reference.md#items)kullanabilirsiniz.

Örneğin, bir dizideki özellik adlarını değil, yalnızca özellik değerlerine sahip tablo sütunlarını istediğinizi varsayalım. Yalnızca bu değerleri döndürmek için, tasarımcı görünümünde veya kod görünümünde çalışmak için aşağıdaki adımları izleyin. Bu örneğin döndürdüğü sonucu burada verilmiştir:

```text
Apples,1
Oranges,2
```

#### <a name="work-in-designer-view"></a>Tasarımcı görünümünde çalışma

Eylemde, **Üstbilgi** sütunu boş tutun. **Değer** sütunundaki her satırda, istediğiniz her dizi özelliğini dereference'dan ayırın. **Değer** altındaki her satır, belirtilen dizi özelliğinin tüm değerlerini döndürür ve tablonuzda bir sütun haline gelir.

1. **Değer'in**altında, istediğiniz her satırda, dinamik içerik listesinin görünmesi için edit kutusunun içini tıklatın.

1. Dinamik içerik listesinde **İfade'yi**seçin.

1. İfade düzenleyicisinde, istediğiniz dizi özellik değerini belirten bu ifadeyi girin ve **Tamam'ı**seçin.

   `item()?['<array-property-name>']`

   Örnek:

   * `item()?['Description']`
   * `item()?['Product_ID']`

   !["CSV tablosu oluştur" için "Açıklama" dereference](./media/logic-apps-perform-data-operations/csv-table-expression.png)

1. İstediğiniz her dizi özelliği için önceki adımları yineleyin. İşinizi bitirdiğinizde, eyleminiz şu örneğe benzer:

   !["CSV tablosu oluştur" işlevinde "item()" işlevi](./media/logic-apps-perform-data-operations/finished-csv-expression.png)

1. İfadeleri daha açıklayıcı sürümlere çözümlemek için kod görünümüne geçin ve tasarımcı görünümüne geri dön ve ardından daraltılmış eylemi yeniden açın:

   **CSV tablo** eylem oluştur şimdi aşağıdaki örnek gibi görünür:

   !["CSV tablosu oluşturma" - çözümlenmiş ifadeler, üstbilgi yok](./media/logic-apps-perform-data-operations/resolved-csv-expression.png)

#### <a name="work-in-code-view"></a>Kod görünümünde çalışma

Eylemin JSON tanımında, `columns` dizi içinde, `header` özelliği boş bir dize olarak ayarlayın. Her `value` özellik için, istediğiniz her dizi özelliğinin dereference'ı.

1. Tasarımcı araç çubuğunda **Kod görünümü'nü**seçin.

1. Kod düzenleyicisinde, eylemin `columns` dizisinde, istediğiniz dizi `header` değerlerinin `value` her sütunu için boş özelliği ve bu ifadeyi ekleyin:

   ```json
   {
      "header": "",
      "value": "@item()?['<array-property-name>']"
   }
   ```

   Örnek:

   ```json
   "Create_CSV_table": {
      "inputs": {
         "columns": [
            { 
               "header": "",
               "value": "@item()?['Description']"
            },
            { 
               "header": "",
               "value": "@item()?['Product_ID']"
            }
         ],
         "format": "CSV",
         "from": "@variables('myJSONArray')"
      }
   }
   ```

1. Tasarımcı görünümüne geri dön ve daraltılmış eylemi yeniden açın.

   **CSV tablo** oluşturma eylemi şimdi bu örnek gibi görünür ve ifadeler daha açıklayıcı sürümlere çözümlenmiştir:

   !["CSV tablosu oluşturma" - çözümlenmiş ifadeler ve üstbilgi yok](./media/logic-apps-perform-data-operations/resolved-csv-expression.png)

Temel iş akışı tanımınızdaki bu eylem hakkında daha fazla bilgi için [Tablo eylemine](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action)bakın.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

**CSV tablo eylemini oluşturup oluşturmadığını** doğrulamak için, kendinize **CSV tablo eyleminden** çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **CSV tablo** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında, **CSV tablo** eylemi oluşturma altında **Çıktı'yı**seçin. 

   Bu örnek, Office 365 Outlook **Bir e-posta** eylemi gönder'i kullanır ve e-postanın gövdesindeki **Çıktı** alanını içerir:

   !["CSV tablosu oluşturma" eylemi için "Çıktı" alanları](./media/logic-apps-perform-data-operations/send-email-create-csv-table-action.png)

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["CSV tablosu oluşturma" eylem sonuçları ile e-posta](./media/logic-apps-perform-data-operations/create-csv-table-email-results.png)

<a name="create-html-table-action"></a>

## <a name="create-html-table-action"></a>HTML tablo eylemi oluşturma

Bir dizideki JavaScript Object Notation (JSON) nesnelerinden özellikleri ve değerlerini içeren bir HTML tablosu oluşturmak için **HTML tablo oluşturma** eylemini kullanın. Ardından, html tablo oluşturma eylemini izleyen eylemlerde ortaya çıkan **tabloyu** kullanabilirsiniz.

Kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, örnek **HTML tablosu oluştur** ve bu makaleden değişken eylem tanımlarını kendi mantık uygulamanızın temel iş akışı tanımına **dönüştürebilirsiniz:** Veri işlem kodu örnekleri - HTML [tablosu oluştur](../logic-apps/logic-apps-data-operations-code-samples.md#create-html-table-action-example) 

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi ve **Initialize değişken** eylemi içeren bir mantık uygulaması kullanır. Eylem, ilk değeri JSON biçiminde bazı özellikleri ve değerleri olan bir dizi olan bir değişken oluşturmak için ayarlanır. Daha sonra mantık uygulamanızı test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   !["HTML tablosu oluşturma" için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

1. HTML tablosu oluşturmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin:

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     !["HTML tablosu oluşturma" eylemi için "Yeni adım" seçeneğini belirleyin](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Bir **eylem seçin**altında, arama `create html table` kutusuna filtreniz olarak girin. Eylemler listesinden HTML **tablo** eylemi oluştur'u seçin.

   !["HTML tablosu oluşturma" eylemini seçin](./media/logic-apps-perform-data-operations/select-create-html-table-action.png)

1. **Gönderen** kutusunda, tabloyu oluşturmak için istediğiniz diziyi veya ifadeyi sağlayın.

   Bu örnekte, **Gönderen** kutusunun içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece önceden oluşturulmuş değişkeni seçebilirsiniz:

   ![HTML tablosu oluşturmak için dizi çıktısını seçin](./media/logic-apps-perform-data-operations/configure-create-html-table-action.png)

   > [!TIP]
   > JSON nesnelerindeki özellikler için kullanıcı dostu belirteçler oluşturmak için bu özellikleri giriş olarak seçebilirsiniz, HTML tablo eylemi **oluşturmadan** önce [Parse JSON'ı](#parse-json-action) kullanın.

   Burada bitmiş örnek HTML tablo eylem **oluşturun:**

   !["HTML tablosu oluşturma" için bitmiş örnek](./media/logic-apps-perform-data-operations/finished-create-html-table-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="customize-table-format"></a>Tablo biçimini özelleştirme

Varsayılan olarak, **Sütunlar** özelliği dizi öğelerini temel alan tablo sütunlarını otomatik olarak oluşturmak üzere ayarlanır. Özel üstbilgi ve değerleri belirtmek için aşağıdaki adımları izleyin:

1. **Sütunlar** listesini açın ve **Özel'i**seçin.

1. **Üstbilgi** özelliğinde, bunun yerine kullanılacak özel üstbilgi metnini belirtin.

1. **Değer** özelliğinde, bunun yerine kullanılacak özel değeri belirtin.

Diziden değerleri döndürmek **için, HTML tablo** eylemi oluştur [ `item()` işlevini](../logic-apps/workflow-definition-language-functions-reference.md#item) kullanabilirsiniz. Bir `For_each` döngüde, [ `items()` işlevi](../logic-apps/workflow-definition-language-functions-reference.md#items)kullanabilirsiniz.

Örneğin, bir dizideki özellik adlarını değil, yalnızca özellik değerlerine sahip tablo sütunlarını istediğinizi varsayalım. Yalnızca bu değerleri döndürmek için, tasarımcı görünümünde veya kod görünümünde çalışmak için aşağıdaki adımları izleyin. Bu örneğin döndürdüğü sonucu burada verilmiştir:

```text
Apples,1
Oranges,2
```

#### <a name="work-in-designer-view"></a>Tasarımcı görünümünde çalışma

Eylemde, **Üstbilgi** sütunu boş tutun. **Değer** sütunundaki her satırda, istediğiniz her dizi özelliğini dereference'dan ayırın. **Değer** altındaki her satır, belirtilen özelliğin tüm değerlerini döndürür ve tablonuzda bir sütun haline gelir.

1. **Değer'in**altında, istediğiniz her satırda, dinamik içerik listesinin görünmesi için edit kutusunun içini tıklatın.

1. Dinamik içerik listesinde **İfade'yi**seçin.

1. İfade düzenleyicisinde, istediğiniz dizi özellik değerini belirten bu ifadeyi girin ve **Tamam'ı**seçin.

   `item()?['<array-property-name>']`

   Örnek:

   * `item()?['Description']`
   * `item()?['Product_ID']`

   !["HTML tablosu oluşturma" eyleminde dereference özelliği](./media/logic-apps-perform-data-operations/html-table-expression.png)

1. İstediğiniz her dizi özelliği için önceki adımları yineleyin. İşinizi bitirdiğinizde, eyleminiz şu örneğe benzer:

   !["HTML tablosu oluştur" işlevinde "item()" işlevi](./media/logic-apps-perform-data-operations/finished-html-expression.png)

1. İfadeleri daha açıklayıcı sürümlere çözümlemek için kod görünümüne geçin ve tasarımcı görünümüne geri dön ve ardından daraltılmış eylemi yeniden açın:

   **HTML tablo oluşturma** eylemi şimdi aşağıdaki örnek gibi görünür:

   !["HTML tablosu oluşturma" - çözümlenmiş ifadeler, üstbilgi yok](./media/logic-apps-perform-data-operations/resolved-html-expression.png)

#### <a name="work-in-code-view"></a>Kod görünümünde çalışma

Eylemin JSON tanımında, `columns` dizi içinde, `header` özelliği boş bir dize olarak ayarlayın. Her `value` özellik için, istediğiniz her dizi özelliğinin dereference'ı.

1. Tasarımcı araç çubuğunda **Kod görünümü'nü**seçin.

1. Kod düzenleyicisinde, eylemin `columns` dizisinde, istediğiniz dizi `header` değerlerinin `value` her sütunu için boş özelliği ve bu ifadeyi ekleyin:

   ```json
   {
      "header": "",
      "value": "@item()?['<array-property-name>']"
   }
   ```

   Örnek:

   ```json
   "Create_HTML_table": {
      "inputs": {
         "columns": [
            { 
               "header": "",
               "value": "@item()?['Description']"
            },
            { 
               "header": "",
               "value": "@item()?['Product_ID']"
            }
         ],
         "format": "HTML",
         "from": "@variables('myJSONArray')"
      }
   }
   ```

1. Tasarımcı görünümüne geri dön ve daraltılmış eylemi yeniden açın.

   **HTML tablo oluşturma** eylemi şimdi bu örnek gibi görünür ve ifadeler daha açıklayıcı sürümlere çözümlenmiştir:

   !["HTML tablosu oluşturma" - çözümlenmiş ifadeler ve üstbilgi yok](./media/logic-apps-perform-data-operations/resolved-html-expression.png)

Temel iş akışı tanımınızdaki bu eylem hakkında daha fazla bilgi için [Tablo eylemine](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action)bakın.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

HTML tablo **oluşturma** eyleminin beklenen sonuçları oluşturup oluşturmadığını doğrulamak için, kendinize **HTML tablo oluşturma** eyleminden çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **HTML tablo oluşturma** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında, **HTML tablo** oluşturma eyleminin altında **Çıktı'yı**seçin. 

   Bu örnek, Office 365 Outlook **Bir e-posta** eylemi gönder'i kullanır ve e-postanın gövdesindeki **Çıktı** alanını içerir:

   !["HTML tablosu oluşturma" için "Çıktı" alanları](./media/logic-apps-perform-data-operations/send-email-create-html-table-action.png)

   > [!NOTE]
   > HTML tablo çıktısını bir e-posta eylemine dahil ederken, E-posta eyleminin gelişmiş seçeneklerinde **İş HTML** özelliğini **Evet** olarak ayarladığınızdan emin olun. Bu şekilde, e-posta eylemi HTML tablosunu doğru biçimlendirir.

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["HTML tablosu oluşturma" sonuçlarıyla e-posta gönder](./media/logic-apps-perform-data-operations/create-html-table-email-results.png)

<a name="filter-array-action"></a>

## <a name="filter-array-action"></a>Filtre dizisi eylemi

Varolan bir diziden belirli ölçütleri karşılayan öğeleri olan daha küçük bir dizi oluşturmak için **Filtre dizi** eylemini kullanın. Daha sonra **filtre** dizisi eylemi sonra izleyen eylemlerde filtrelenmiş dizi kullanabilirsiniz.

> [!NOTE]
> Durumunuzda kullandığınız herhangi bir filtre metni büyük/küçük harf duyarlıdır. Ayrıca, bu eylem dizideki öğelerin biçimini veya bileşenlerini değiştiremez. 
> 
> **Filtre dizi** eyleminden dizi çıktısını kullanabilmek için, bu eylemlerin dizileri giriş olarak kabul etmesi veya çıktı dizisini başka bir uyumlu biçime dönüştürmeniz gerekebilir.
> 
> Bir HTTP bitiş noktası nı arar ve JSON yanıtı alırsanız, JSON yanıtını işlemek için **Parse JSON** eylemini kullanın. 
> Aksi takdirde, **Filtre dizisi** eylemi Yalnızca yanıt gövdesini okuyabilir, JSON yükünün yapısını değil.

Kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, örnek **Filtre dizisini** kopyalayabilir ve bu makaleden değişken eylem tanımlarını kendi mantık uygulamanızın temel iş akışı tanımına **dönüştürebilirsiniz:** Veri işlem kodu örnekleri - [Filtre dizi](../logic-apps/logic-apps-data-operations-code-samples.md#filter-array-action-example)

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi ve **Initialize değişken** eylemi içeren bir mantık uygulaması kullanır. Eylem, ilk değeri bazı örnek tamsayıları olan bir dizi olan bir değişken oluşturmak için ayarlanır. Daha sonra mantık uygulamanızı test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   > [!NOTE]
   > Bu örnek basit bir tamsayı dizisi kullansa da, bu eylem özellikle nesnelerin özelliklerine ve değerlerine göre filtre uygulayabileceğiniz JSON nesne dizileri için kullanışlıdır.

   !["Filtre dizisi" eylemi için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-filter-array-action.png)

1. Filtre uygulanmış diziyi oluşturmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin: 

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     !["Filtre dizisi" eylemi için "Yeni adım" seçeneğini belirleyin](./media/logic-apps-perform-data-operations/add-filter-array-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak girin. `filter array` Eylemler listesinden Filtre **dizisi** eylemini seçin.

   !["Filtre dizisi" eylemini seçin](./media/logic-apps-perform-data-operations/select-filter-array-action.png)

1. **Gönderen** kutusunda, filtrelemek istediğiniz diziveya ifadeyi sağlayın.

   Bu örnekte, **Gönderen** kutusunun içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece önceden oluşturulmuş değişkeni seçebilirsiniz:

   ![Filtre uygulanmış dizi oluşturmak için dizi çıktısını seçin](./media/logic-apps-perform-data-operations/configure-filter-array-action.png)

1. Koşul için, karşılaştırılacak dizi öğelerini belirtin, karşılaştırma işleci seçin ve karşılaştırma değerini belirtin.

   Bu örnek, `item()` dizideki her öğeye erişmek için işlevi kullanırken, **Filtre dizisi** eylemi değeri birden büyük olan dizi öğelerini arar:

   !["Filtre dizisi" eylemi için bitmiş örnek](./media/logic-apps-perform-data-operations/finished-filter-array-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Temel iş akışı tanımınızda bu eylem hakkında daha fazla bilgi için [Sorgu eylemine](../logic-apps/logic-apps-workflow-actions-triggers.md#query-action)bakın.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

**Filtre dizisi** eyleminin beklenen sonuçları oluşturup oluşturmadığını doğrulamak için, kendinize **Filtre dizisi** eyleminden çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **Filtre dizisi** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında **İfade'yi**seçin. **Filtre dizisi** eyleminden dizi çıktısını almak için Filtre **dizisi** eyleminin adını içeren bu ifadeyi girin:

   `@actionBody('Filter_array')`

   Bu örnek, Office 365 Outlook Bir e-posta eylemi gönder'i kullanır ve **e-postanın** gövdesindeki **actionBody('Filter_array')** ifadesinden çıktıları içerir:

   !["Filtre dizisi" eyleminden eylem çıktıları](./media/logic-apps-perform-data-operations/send-email-filter-array-action.png)

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["Filtre dizisi" eylem sonuçları ile e-posta](./media/logic-apps-perform-data-operations/filter-array-email-results.png)

<a name="join-action"></a>

## <a name="join-action"></a>Eyleme katılma

Bir dizideki tüm öğeleri içeren bir dize oluşturmak ve bu öğeleri belirli bir sınır dışı layıcı karakterle ayırmak için **Birleştirme** eylemini kullanın. Daha sonra dizeyi **Birleştirme** eyleminden sonra izleyen eylemlerde kullanabilirsiniz.

Kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, bu makaledeki değişken eylem tanımlarını **birleştir** ve kendi mantık uygulamanızın temel iş akışı tanımına **dönüştürebilirsiniz:** [Veri işlem kodu örnekleri - Katıl](../logic-apps/logic-apps-data-operations-code-samples.md#join-action-example)

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi ve **Initialize değişken** eylemi içeren bir mantık uygulaması kullanır. Bu eylem, ilk değeri bazı örnek tamsayılar alabilen bir dizi olan bir değişken oluşturmak için ayarlanır. Mantık uygulamanızı daha sonra test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   !["Katılma" eylemi için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-join-action.png)

1. Bir diziden dize oluşturmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin:

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     ![SSelect "Yeni adım" eylem "Join" için](./media/logic-apps-perform-data-operations/new-step-add-join-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak girin. `join` Eylemler listesinden şu eylemi seçin: **Katıl**

   !["Katıl" eylemini seçin](./media/logic-apps-perform-data-operations/select-join-operation-action.png)

1. **Gönderen** kutusunda, dize olarak birleştirmek istediğiniz öğeleri içeren diziyi sağlayın.

   Bu örnekte, **Gönderen** kutusunun içini tıklattığınızda, önceden oluşturulmuş değişkeni seçebilmeniz için görünen dinamik içerik listesi:  

   ![Dize oluşturmak için dizi çıktısını seçin](./media/logic-apps-perform-data-operations/configure-join-action.png)

1. Kutuyla **Join with** Katıl'a, her dizi öğesini ayırmak için istediğiniz karakteri girin. 

   Bu örnekte bir üst üste (:) ayırıcı olarak.

   ![Ayırıcı karakterini sağlama](./media/logic-apps-perform-data-operations/finished-join-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Temel iş akışı tanımınızda bu eylem hakkında daha fazla bilgi [için, Join eylemine](../logic-apps/logic-apps-workflow-actions-triggers.md#join-action)bakın.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

**Join** eyleminin beklenen sonuçları oluşturup oluşturmadığını doğrulamak için, kendinize **Join** eyleminden çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **Katıl** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında, **Birleştirme** eylemi altında **Çıktı'yı**seçin. 

   Bu örnek, Office 365 Outlook **Bir e-posta** eylemi gönder'i kullanır ve e-postanın gövdesindeki **Çıktı** alanını içerir:

   !["Birleştirme" eylemi için "Çıktı" alanları](./media/logic-apps-perform-data-operations/send-email-join-action.png)

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["Join" eylem sonuçları ile e-posta](./media/logic-apps-perform-data-operations/join-send-email-results.png)

<a name="parse-json-action"></a>

## <a name="parse-json-action"></a>Parse JSON eylem

JavaScript Nesne Gösterimi (JSON) içeriğindeki özelliklere başvurmak veya bunlara erişmek **için, Parse JSON** eylemini kullanarak bu özellikler için kullanıcı dostu alanlar veya belirteçler oluşturabilirsiniz. Bu şekilde, mantık uygulamanızın girişlerini belirtirken dinamik içerik listesinden bu özellikleri seçebilirsiniz. Bu eylem için, bir JSON şeması sağlayabilir veya örnek JSON içeriğinizden veya yükünüzden bir JSON şeması oluşturabilirsiniz.

Kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, bu makaleden **parse JSON** ve **Initialize değişken** eylem tanımlarını kendi mantık uygulamanızın temel iş akışı tanımına kopyalayabilirsiniz: [Veri işlem kodu örnekleri - Ayrı ayrı JSON](../logic-apps/logic-apps-data-operations-code-samples.md#parse-json-action-example)

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi ve **Initialize değişken** eylemi içeren bir mantık uygulaması kullanır. Eylem, ilk değeri özellikleri ve değerleri olan bir JSON nesnesi olan bir değişken oluşturmak için ayarlanır. Daha sonra mantık uygulamanızı test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   !["Parse JSON" eylemi için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-parse-json-action.png)

1. JSON içeriğini ayrışdırmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin:

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     !["Parse JSON" eylemi için "Yeni adım"ı seçin](./media/logic-apps-perform-data-operations/add-parse-json-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak girin. `parse json` Eylemler listesinden **Parse JSON eylemini** seçin.

   !["Ayrıştırık JSON" eylemini seçin](./media/logic-apps-perform-data-operations/select-parse-json-action.png)

1. **İçerik** kutusunda, ayrışdırmak istediğiniz JSON içeriğini sağlayın.

   Bu örnekte, **İçerik** kutusunun içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece önceden oluşturulmuş değişkeni seçebilirsiniz:

   ![Ayrıştis JSON eylemi için JSON nesnesi seçin](./media/logic-apps-perform-data-operations/configure-parse-json-action.png)

1. Ayrıştirdığınız JSON içeriğini açıklayan JSON şeasını girin.

   Bu örnek için, burada JSON şema:

   ![Ayrışdırmak istediğiniz JSON nesnesi için JSON şeması sağlayın](./media/logic-apps-perform-data-operations/provide-schema-parse-json-action.png)

   Şema yoksa, bu şema JSON içeriğinden veya *yükten*oluşturabilirsiniz, ayrıştıyorsun. 
   
   1. **Ayrışma JSON** eyleminde **şema oluşturmak için örnek yükü kullan'ı**seçin.

   1. **Örnek bir JSON yükünü girin veya yapıştırın,** JSON içeriğini sağlayın ve ardından **Bitti'yi**seçin.

      ![Şema oluşturmak için JSON içeriğini girin](./media/logic-apps-perform-data-operations/generate-schema-parse-json-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Temel iş akışı tanımınızda bu eylem hakkında daha fazla bilgi için [Parse JSON eylemine](../logic-apps/logic-apps-workflow-actions-triggers.md)bakın.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

**Parse JSON** eyleminin beklenen sonuçları oluşturup oluşturmadığını doğrulamak için, **kendinize Parse JSON** eyleminden çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **Parse JSON** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında, **Parse JSON** eylemi altında, artık ayrıştılı JSON içeriğinden özellikleri seçebilirsiniz.

   Bu örnek, Office 365 Outlook Bir e-posta eylemi gönder'i kullanır ve **e-postanın** gövdesindeki **FirstName,** **Soyadı**ve **E-posta** alanlarını içerir:

   !["E-posta gönder" eyleminde JSON özellikleri](./media/logic-apps-perform-data-operations/send-email-parse-json-action.png)

   Burada bitmiş e-posta eylem:

   ![E-posta eylemi için bitmiş örnek](./media/logic-apps-perform-data-operations/send-email-parse-json-action-2.png)

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin. 

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["Parse JSON" eylem sonuçları ile e-posta](./media/logic-apps-perform-data-operations/parse-json-email-results.png)

<a name="select-action"></a>

## <a name="select-action"></a>Eylem seçin

Varolan bir dizideki değerlerden oluşturulmuş JSON nesnelerine sahip bir dizi oluşturmak **için, Eylem Seç'i** kullanın. Örneğin, her JSON nesnesinin sahip olması gereken özellikleri ve kaynak dizideki değerleri bu özelliklerle nasıl eşleştirebileceğinizi belirterek, bir tamsayı dizisindeki her değer için bir JSON nesnesi oluşturabilirsiniz. Ve bu JSON nesnelerindeki bileşenleri değiştirebiliyor olsanız da, çıktı dizisi her zaman kaynak diziyle aynı sayıda öğeye sahiptir.

> [!NOTE]
> **Eylemleri seç** eyleminden dizi çıktısını kullanabilmek için, bu eylemlerin dizileri giriş olarak kabul etmesi veya çıktı dizisini başka bir uyumlu biçime dönüştürmeniz gerekebilir. 

Kod görünümü düzenleyicisinde çalışmayı tercih ederseniz, bu makaledeki değişken eylem tanımlarını **seç** ve **başlangıç** örneğini kendi mantık uygulamanızın temel iş akışı tanımına kopyalayabilirsiniz: Veri işlem kodu örnekleri [- Seçin](../logic-apps/logic-apps-data-operations-code-samples.md#select-action-example) 

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve **Yineleme** tetikleyicisi ve **Initialize değişken** eylemi içeren bir mantık uygulaması kullanır. Eylem, ilk değeri bazı örnek tamsayıları olan bir dizi olan bir değişken oluşturmak için ayarlanır. Daha sonra mantık uygulamanızı test ettiğinizde, tetikleyicinin ateşetmesini beklemeden uygulamanızı el ile çalıştırabilirsiniz.

   !["Seç" eylemi için örnek mantık uygulaması başlatma](./media/logic-apps-perform-data-operations/sample-starting-logic-app-select-action.png)

1. Diziyi oluşturmak istediğiniz mantık uygulamanızda aşağıdaki adımlardan birini izleyin: 

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     !["Seç" eylemi için "Yeni adım"ı seçin](./media/logic-apps-perform-data-operations/add-select-operation-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun**+** üzerine taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak girin. `select` Eylemler listesinden eylem **seç'i** seçin.

   !["Seç" eylemini seçin](./media/logic-apps-perform-data-operations/select-select-action.png)

1. **Gönderen** kutusunda, istediğiniz kaynak diziyi belirtin.

   Bu örnekte, **Gönderen** kutusunun içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece önceden oluşturulmuş değişkeni seçebilirsiniz:

   ![Eylem seç için kaynak diziyi seçin](./media/logic-apps-perform-data-operations/configure-select-action.png)

1. **Harita** kutusunun sol sütununda, kaynak dizideki her değeri atamak istediğiniz özellik adını sağlayın. Sağ sütunda, özelliği atamak istediğiniz değeri temsil eden bir ifade belirtin.

   Bu örnek, her dizi öğesine erişen bir ifadedeki `item()` işlevi kullanarak tamsayı dizisindeki her değeri atamak için özellik adı olarak "Product_ID" belirtir. 

   ![Dizi oluşturmak için JSON nesne özelliğini ve değerlerini belirtin](./media/logic-apps-perform-data-operations/configure-select-action-2.png)

   İşte bitmiş eylem:

   !["Seç" eylemi için bitmiş örnek](./media/logic-apps-perform-data-operations/finished-select-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Temel iş akışı tanımınızda bu eylem hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-workflow-actions-triggers.md)

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

**Select** eyleminin beklenen sonuçları oluşturup oluşturmadığını doğrulamak için, kendinize **Select** eyleminden çıktı içeren bir bildirim gönderin.

1. Mantık uygulamanızda, **Select** eyleminin sonuçlarını gönderebilecek bir eylem ekleyin.

1. Bu eylemde, sonuçların görünmesini istediğiniz her yeri tıklatın. Dinamik içerik listesi açıldığında **İfade'yi**seçin. **Select** eyleminden dizi çıktısını almak için, **Eylem seç'in** adını içeren bu ifadeyi girin:

   `@actionBody('Select')`

   Bu örnek, Office 365 Outlook **Bir e-posta** eylemi `@actionBody('Select')` gönder'i kullanır ve e-postanın gövdesindeki ifadeden çıktıları içerir:

   !["Seç" eyleminden eylem çıktıları](./media/logic-apps-perform-data-operations/send-email-select-action.png)

1. Şimdi, mantık uygulamanızı manuel olarak çalıştırın. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   Kullandığınız e-posta bağlayıcısı dayanarak, elde ettiğiniz sonuçlar şunlardır:

   !["Seç" eylem sonuçları ile e-posta](./media/logic-apps-perform-data-operations/select-email-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* Logic [Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
