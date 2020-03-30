---
title: Şema güncellemeleri Haziran-1-2016
description: Azure Mantık Uygulamaları'nda mantık uygulama tanımları için 2016-06-01 güncelleştirilmiş şema sürümü
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 07/25/2016
ms.openlocfilehash: e2f65f1c52dc7dfb2e4e4bf66f5c7e82f4b802b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792881"
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Azure Logic Apps için Şema güncellemeleri - 1 Haziran 2016

Azure Logic Apps için [güncelleştirilmiş şema](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json) ve API sürümü, mantık uygulamalarını daha güvenilir ve kullanımı daha kolay hale getiren önemli geliştirmeleri içerir:

* [Kapsamlar,](#scopes) eylemleri eylemler topluluğu olarak gruplandırmanıza veya yuvalamanıza izin vermez.
* [Koşullar ve döngüler](#conditions-loops) artık birinci sınıf eylemlerdir.
* Eylemleri `runAfter` mülkle çalıştırmak için daha kesin sıralama,`dependsOn`

Mantık uygulamalarınızı 1 Ağustos 2015 önizleme şemasından 1 Haziran 2016 şemaya yükseltmek için [yükseltme bölümüne göz atın.](#upgrade-your-schema)

<a name="scopes"></a>

## <a name="scopes"></a>Kapsamlar

Bu şema, eylemleri birlikte gruplandırmanıza veya eylemleri birbirinin içine yerleştirmenize izin veren kapsamlar içerir. Örneğin, bir koşul başka bir koşul içerebilir. [Kapsam sözdizimi](../logic-apps/logic-apps-loops-and-scopes.md)hakkında daha fazla bilgi edinin veya bu temel kapsam örneğini gözden geçirin:

```json
{
   "actions": {
      "Scope": {
         "type": "Scope",
         "actions": {                
            "Http": {
               "inputs": {
                   "method": "GET",
                   "uri": "https://www.bing.com"
               },
               "runAfter": {},
               "type": "Http"
            }
         }
      }
   }
}
```

<a name="conditions-loops"></a>

## <a name="conditions-and-loops-changes"></a>Koşullar ve döngüdeğişiklikleri

Önceki şema sürümlerinde, koşullar ve döngüler tek bir eylemle ilişkili parametrelerdi. Bu şema bu sınırlamayı kaldırır, bu nedenle koşullar ve döngüler artık eylem türleri olarak kullanılabilir. [Döngüler ve kapsamlar,](../logic-apps/logic-apps-loops-and-scopes.md) [koşullar](../logic-apps/logic-apps-control-flow-conditional-statement.md)hakkında daha fazla bilgi edinin veya koşul eylemini gösteren bu temel örneği gözden geçirin:

```json
{
   "Condition - If trigger is some trigger": {
      "type": "If",
      "expression": "@equals(triggerBody(), '<trigger-name>')",
      "runAfter": {},
      "actions": {
         "Http_2": {
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com"
            },
            "runAfter": {},
            "type": "Http"
         }
      },
      "else": 
      {
         "Condition - If trigger is another trigger": {}
      }  
   }
}
```

<a name="run-after"></a>

## <a name="runafter-property"></a>'runAfter' özelliği

Önceki `runAfter` eylemlerin `dependsOn`durumuna göre eylemler için çalışma sırasını belirttiğiniz zaman daha fazla kesinlik sağlayan özellik, değiştirilir. Özellik, `dependsOn` önceki eylemin başarılı olup olmadığına, başarısız olup olmadığına veya atlanıp atlanmadığına bağlı olarak "eylemin çalışıp çalışmadığını ve başarılı olup olmadığını" belirtir - eylemi kaç kez çalıştırmak istediğinize değil. Özellik, `runAfter` nesnenin çalıştırdığı tüm eylem adlarını belirten bir nesne olarak esneklik sağlar. Bu özellik, tetikleyici olarak kabul edilebilir bir dizi durum da tanımlar. Örneğin, bir eylemin eylem A'nın başarılı olduktan sonra ve ayrıca B eyleminin `runAfter` başarılı veya başarısız olduktan sonra çalışmasını istiyorsanız, bu özelliği ayarlayın:

```json
{
   // Other parts in action definition
   "runAfter": {
      "A": ["Succeeded"],
      "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Şemanızı yükseltin

En son [şemaya](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json)yükseltmek için yalnızca birkaç adım atmanız gerekir. Yükseltme işlemi, yükseltme komut dosyasının çalıştırılmasını, yeni bir mantık uygulaması olarak kaydetmeyi ve isterseniz muhtemelen önceki mantık uygulamasının üzerine yazmayı içerir.

1. Azure portalında mantık uygulamanızı açın.

2. Genel **Bakış'a**gidin. Mantık uygulaması araç çubuğunda, **Schema'yı Güncelleştir'i**seçin.
   
   ![Schema'yı Güncelleştir'i Seçin][1]
   
   Yükseltilen tanım döndürülür ve gerekirse kaynak tanımına kopyalayıp yapıştırabilirsiniz. 

   > [!IMPORTANT]
   > Tüm bağlantı başvurularının yükseltilmiş mantık uygulamasında geçerli kalması için **Kaydet'i** seçtiğinizden *emin olun.*

3. Yükseltme bıçağı araç **çubuğunda, Farklı Kaydet'i**seçin.

4. Mantık adını ve durumunu girin. Yükseltilmiş mantık uygulamanızı dağıtmak için **Oluştur'u**seçin.

5. Yükseltilmiş mantık uygulamanızın beklendiği gibi çalıştığını doğrulayın.
   
   > [!NOTE]
   > Bir el kitabı veya istek tetikleyicisi kullanıyorsanız, geri arama URL'si yeni mantık uygulamanızda değişir. Uçuça deneyimin çalıştığından emin olmak için yeni URL'yi sınayın. Önceki URL'leri korumak için, varolan mantık uygulamanızın üzerine klonlayabilirsiniz.

6. *İsteğe bağlı* Önceki mantık uygulamanızın üzerine yeni şema sürümüyle, araç çubuğunda yazmak için, **Schema'yı Güncelleştir'in**yanındaki **Clone'u**seçin. Bu adım, yalnızca aynı kaynak kimliğini tutmak veya mantık uygulamanızın tetikleyici URL'sini istemek istiyorsanız gereklidir.

## <a name="upgrade-tool-notes"></a>Araç notlarını yükseltme

### <a name="mapping-conditions"></a>Haritalama koşulları

Yükseltilmiş tanımda, araç doğru ve yanlış dal eylemlerini kapsam olarak gruplandırmak için en iyi çabayı gösterir. Özellikle, tasarımcı deseni bir `@equals(actions('a').status, 'Skipped')` `else` eylem olarak görünür. Ancak, araç tanınmaz desenler algılarsa, araç hem doğru hem de yanlış dal için ayrı koşullar oluşturabilir. Gerekirse, yükseltmeden sonra eylemleri yeniden eşleyebilirsiniz.

#### <a name="foreach-loop-with-condition"></a>koşulu ile 'foreach' döngü

Yeni şemada, öğe başına bir koşulla her döngü **için** kullanılan deseni çoğaltmak için filtre eylemini kullanabilirsiniz. Ancak, yükseltme yaptığınızda değişiklik otomatik olarak gerçekleşir. Koşul, her döngü **için** önce görünen, yalnızca koşulla eşleşen bir dizi öğe döndüren ve bu diziyi her eylem **için** geçen bir filtre eylemi olur. Örneğin, [bkz. Döngüler ve kapsamlar.](../logic-apps/logic-apps-loops-and-scopes.md)

### <a name="resource-tags"></a>Kaynak etiketleri

Yükseltmeden sonra kaynak etiketleri kaldırılır, bu nedenle yükseltilen iş akışı için bunları sıfırlamanız gerekir.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="renamed-manual-trigger-to-request-trigger"></a>'İstek' tetikleyicisine 'el kitabı' tetikleyicisi olarak yeniden adlandırıldı

Tetikleyici `manual` türü amortismana uğradı ve `request` türüyle `http`yeniden adlandırıldı. Bu değişiklik, tetikleyici oluşturmak için kullanılan desen türü için daha fazla tutarlılık oluşturur.

### <a name="new-filter-action"></a>Yeni 'filtre' eylemi

Büyük bir diziyi daha küçük bir öğe `filter` kümesine filtrelemek için, yeni tür bir dizi ve bir koşul kabul eder, her öğe için durumu değerlendirir ve durumu karşılayan öğeleri içeren bir dizi döndürür.

### <a name="restrictions-for-foreach-and-until-actions"></a>'Foreach' ve 'until' eylemleri için kısıtlamalar

Ve `foreach` `until` döngü tek bir eylemle sınırlıdır.

### <a name="new-trackedproperties-for-actions"></a>Eylemler için yeni 'izlenen Özellikler'

Eylemler artık kardeş `trackedProperties` `runAfter` ve `type` özellikleri olarak adlandırılan ek bir özellik olabilir. Bu nesne, iş akışının bir parçası olarak yayılan Azure Tanılama telemetrisine eklemek istediğiniz belirli eylem girişlerini veya çıktılarını belirtir. Örnek:

``` json
{
   "Http": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.bing.com"
      },
      "runAfter": {},
      "type": "Http",
      "trackedProperties": {
         "responseCode": "@action().outputs.statusCode",
         "uri": "@action().inputs.uri"
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Mantık uygulamaları için iş akışı tanımları oluşturma](../logic-apps/logic-apps-author-definitions.md)
* [Mantıksal uygulama dağıtımı otomatikleştirme](logic-apps-azure-resource-manager-templates-overview.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
