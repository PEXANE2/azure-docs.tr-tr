---
title: Mantık uygulaması JSON iş akışı tanımları oluşturma, yeniden oluşturma veya genişletme
description: Azure Mantık Uygulamaları'nda mantık uygulamanızın JSON iş akışı tanımlarını yazma, değiştirme ve genişletme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/01/2018
ms.openlocfilehash: 0f5f01c757bf651beddaa76fc3eb8046b21b31eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979388"
---
# <a name="create-edit-or-extend-json-for-logic-app-workflow-definitions-in-azure-logic-apps"></a>Azure Logic Apps'ta mantık uygulaması iş akışı tanımları için JSON oluşturma, değiştirme veya genişletme

[Azure Logic Apps'ta](../logic-apps/logic-apps-overview.md)otomatik leştirilmiş iş akışlarıyla kurumsal tümleştirme çözümleri oluşturduğunuzda, temel mantık uygulaması tanımları, açıklamaları ve doğrulamaları için İş Akışı Tanımı [Dili (WDL) şemasıyla](../logic-apps/logic-apps-workflow-definition-language.md) birlikte basit ve bildirimsel JavaScript Nesne Gösterimi (JSON) kullanır. Bu biçimler, mantık uygulama tanımlarının kod hakkında çok şey bilmeden okunmasını ve anlaşılmasını kolaylaştırır.
Mantıksal uygulamalar oluşturmayı ve dağıtmayı otomatikleştirmek istediğinizde, mantıksal uygulama tanımlarını [Azure Kaynak Yöneticisi şablonlarına](../azure-resource-manager/templates/overview.md) [Azure kaynakları](../azure-resource-manager/management/overview.md) olarak ekleyebilirsiniz.
Mantık uygulamaları oluşturmak, yönetmek ve dağıtmak için [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md)veya [Azure Logic Apps REST API'lerini](https://docs.microsoft.com/rest/api/logic/)kullanabilirsiniz.

JSON'da mantık uygulama tanımlarıyla çalışmak için, Azure portalında veya Visual Studio'da çalışırken Code View düzenleyicisini açın veya tanımı istediğiniz herhangi bir düzenleyiciye kopyalayın.
Mantık uygulamalarında yeniyseniz, [ilk mantık uygulamanızı nasıl oluşturabileceğinizi](../logic-apps/quickstart-create-first-logic-app-workflow.md)gözden geçirin.

> [!NOTE]
> Mantık uygulama tanımlarında parametreleri ve birden çok tetikleyiciyi tanımlama gibi bazı Azure Mantık Uygulamaları özellikleri, Logic Apps Tasarımcısı'nda değil, yalnızca JSON'da kullanılabilir.
> Bu nedenle, bu görevler için Code View veya başka bir düzenleyicide çalışmanız gerekir.

## <a name="edit-json---azure-portal"></a>JSON'u edit - Azure portalı

1. <a href="https://portal.azure.com" target="_blank">Azure portalında</a>oturum açın.

2. Sol menüden **Tüm hizmetleri**seçin.
Arama kutusunda "mantık uygulamalarını" bulun ve sonuçlardan mantık uygulamanızı seçin.

3. Mantık uygulamanızın menüsünde, **Geliştirme Araçları**altında, **Mantık Uygulama Kodu Görünümü'nü**seçin.

   Code View düzenleyicisi, mantık uygulama tanımınızı JSON formatında açar ve gösterir.

## <a name="edit-json---visual-studio"></a>JSON'u edit - Visual Studio

Visual Studio'da mantık uygulama tanımıüzerinde çalışmadan [önce, gerekli araçları yüklediğinizden](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites)emin olun.
Visual Studio ile bir mantık uygulaması oluşturmak için [Quickstart'ı gözden geçirin: Azure Logic Apps - Visual Studio ile görevleri ve işlemleri otomatikleştirin.](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)

Visual Studio'da, doğrudan Azure portalından veya Visual Studio'dan Azure Kaynak Yöneticisi projeleri olarak oluşturulan ve dağıtılan mantıksal uygulamaları açabilirsiniz.

1. Mantık uygulamanızı içeren Visual Studio çözümünüzü veya [Azure Kaynak Grubu](../azure-resource-manager/management/overview.md) projesini açın.

2. **LogicApp.json**adlı [bir Kaynak Yöneticisi şablonunda](../azure-resource-manager/templates/overview.md)varsayılan olarak görünen mantık uygulamanızın tanımını bulun ve açın.
Bu şablonu farklı ortamlara dağıtım için kullanabilir ve özelleştirebilirsiniz.

3. Mantık uygulama tanımı ve şablonu için kısayol menüsünü açın.
**Mantıksal Uygulama Tasarımcısı ile Aç**’ı seçin.

   ![Visual Studio çözümünde mantık uygulamasını açın](./media/logic-apps-author-definitions/open-logic-app-designer.png)

   > [!TIP]
   > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

4. Tasarımcının alt kısmında, **Kod Görünümü'ni**seçin.

   Code View düzenleyicisi, mantık uygulama tanımınızı JSON formatında açar ve gösterir.

5. Tasarımcı görünümüne dönmek için, Code View düzenleyicisinin altındaki **Tasarım'ı**seçin.

## <a name="parameters"></a>Parametreler

Dağıtım yaşam döngüsü genellikle geliştirme, test, evreleme ve üretim için farklı ortamlara sahiptir. Mantık uygulamanız boyunca hardcoding yapmadan veya dağıtım gereksinimlerinize bağlı olarak değişen değerlere sahip olduğunuzda, mantık uygulaması dağıtımını otomatikleştirebilmeniz için iş akışı tanımınız için bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/management/overview.md) oluşturabilirsiniz.

Parametreleri *parametreleştirmek*veya parametreleri tanımlamak ve kullanmak için bu genel adımları izleyin. Daha sonra, bu değerleri şablonunuza aktaran ayrı bir parametre dosyasındaki değerleri sağlayabilirsiniz. Bu şekilde, mantık uygulamanızı güncellemek ve yeniden dağıtmak zorunda kalmadan bu değerleri daha kolay değiştirebilirsiniz. Tüm ayrıntılar için [bkz: Genel Bakış: Azure Kaynak Yöneticisi şablonları ile mantıksal uygulamalar için dağıtımı otomatikleştirin.](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)

1. Şablonunuzda, sırasıyla dağıtım ve çalışma zamanında kullanılacak değerleri kabul etmek için şablon parametreleri ve iş akışı tanımı parametrelerini tanımlayın.

   Şablon parametreleri iş akışı tanımınızın dışındaki bir parametre bölümünde, iş akışı tanımı parametreleri ise iş akışı tanımınızın içindeki bir parametre bölümünde tanımlanır.

1. Bu parametrelere başvuran ifadeler ile hardcoded değerleri değiştirin. Şablon ifadeleri, iş akışı tanımı ifadelerinden farklı olan sözdizimini kullanır.

   Dağıtımda değerlendirilen şablon ifadelerini çalışma zamanında değerlendirilen iş akışı tanımı ifadelerinin içinde kullanmayarak kodunuzu karmaşık hale getirmekten kaçının. Yalnızca iş akışı tanımınızın dışındaki şablon ifadelerini kullanın. İş akışı tanımınızda yalnızca iş akışı tanımı ifadeleri kullanın.

   İş akışı tanım parametrelerinizin değerlerini belirtirken, iş akışı tanımınızın dışında olan ancak yine de mantık uygulamanızın kaynak tanımının içindeki parametreler bölümünü kullanarak şablon parametrelerine başvuruda bulunabilirsiniz. Bu şekilde, şablon parametre değerlerini iş akışı tanımı parametrelerinize geçirebilirsiniz.

1. Parametrelerinizin değerlerini ayrı bir [parametre dosyasında](../azure-resource-manager/templates/parameter-files.md) depolayın ve bu dosyayı dağıtımınıza ekleyin.

## <a name="process-strings-with-functions"></a>İşlevli dizeleri işleme

Logic Apps dizeleri ile çalışmak için çeşitli işlevleri vardır.
Örneğin, bir şirket adını bir siparişten başka bir sisteme geçirmek istediğinizi varsayalım.
Ancak, karakter kodlama için uygun işleme hakkında emin değilseniz.
Bu dizeüzerinde base64 kodlama sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağısı yerine birkaç karakter değiştirebilirsiniz. Ayrıca, ilk beş karakter kullanılmadığından, yalnızca şirket adı için bir alt dize gerekir.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "companyName": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "Request",
      "kind": "Http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Bu adımlar, bu örneğin içeriden dışarıya doğru çalışarak bu dizeyi nasıl işlediğini açıklar:

```
"uri": "https://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. [`length()`](../logic-apps/logic-apps-workflow-definition-language.md) Şirket adını alın, böylece toplam karakter sayısını elde elabilirsiniz.

2. Daha kısa bir dize `5`almak için, çıkar.

3. Şimdi bir [`substring()`](../logic-apps/logic-apps-workflow-definition-language.md).
Dizinle `5`başlayın ve dizenin geri kalanına gidin.

4. Bu alt dizeyi bir [`base64()`](../logic-apps/logic-apps-workflow-definition-language.md) dize dönüştürün.

5. Şimdi [`replace()`](../logic-apps/logic-apps-workflow-definition-language.md) karakterleri `+` olan `-` tüm karakterler.

6. Son [`replace()`](../logic-apps/logic-apps-workflow-definition-language.md) olarak, `/` karakterleri `_` olan tüm karakterler.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Öğeleri özellik değerlerine göre eşleyin, ardından haritaları parametre olarak kullanın

Bir özelliğin değerini temel alan farklı sonuçlar elde etmek için, her özellik değerini bir sonuca eşleştirebilen bir harita oluşturabilir ve ardından bu haritayı parametre olarak kullanabilirsiniz.

Örneğin, bu iş akışı bazı kategorileri parametre olarak tanımlar ve bu kategorileri belirli bir URL ile eşleştiren bir harita olarak tanımlar.
İlk olarak, iş akışı makalelerin bir listesini alır. Ardından, iş akışı, her makaleiçin kategoriyle eşleşen URL'yi bulmak için haritayı kullanır.

*   İşlev, [`intersection()`](../logic-apps/logic-apps-workflow-definition-language.md) kategorinin bilinen tanımlanmış bir kategoriyle eşleşip eşleşmediğini denetler.

*   Eşleşen bir kategori aldıktan sonra, örnek kare ayraçlar kullanarak öğeyi haritadan çeker:`parameters[...]`

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "https://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=https://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

## <a name="get-data-with-date-functions"></a>Tarih işlevleriyle veri alma

*Tetikleyicileri*doğal olarak desteklemeyen bir veri kaynağından veri almak için, bunun yerine saat ve tarihlerle çalışmak için Tarih işlevlerini kullanabilirsiniz.
Örneğin, bu ifade, bu iş akışının adımlarının içeriden dışarıya doğru çalışma adımlarının ne kadar sürdüğünü bulur:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. `order` Eylemden, `startTime`ayıklayın.
2. Geçerli saati ' `utcNow()`ile alın.
3. Bir saniye çıkarın:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md)

   Diğer zaman birimlerini kullanabilirsiniz, `hours`gibi. `minutes`

3. Şimdi, bu iki değeri karşılaştırabilirsiniz.

   İlk değer ikinci değerden küçükse, siparişin ilk yerleştirildiğinden bu yana bir saniyeden fazla zaman geçmiştir.

Tarihleri biçimlendirmek için string formatters'ı kullanabilirsiniz. Örneğin, RFC1123 almak için. [`utcnow('r')`](../logic-apps/logic-apps-workflow-definition-language.md)
[Tarih biçimlendirme](../logic-apps/logic-apps-workflow-definition-language.md)hakkında daha fazla bilgi edinin.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder-id"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "https://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Koşula dayalı adımları çalıştırma (koşullu ifadeler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere dayalı adımları çalıştırma (geçiş deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Adımları çalıştırma ve yineleme (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplanmış eylem durumuna (kapsamlara) göre adımları çalıştırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Azure Logic [Apps için İş Akışı Tanımı Dili şeması](../logic-apps/logic-apps-workflow-definition-language.md) hakkında daha fazla bilgi edinin
* [Azure Logic Apps için iş akışı eylemleri ve tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md) hakkında daha fazla bilgi edinin
