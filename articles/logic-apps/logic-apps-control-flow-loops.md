---
title: Eylemleri yinelemek için döngüler ekleme
description: Azure Logic Apps’te iş akışı eylemlerini veya işlem dizilerini tekrarlayacak döngüler oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/05/2019
ms.openlocfilehash: 5f6c04c9a57dc8c250d99f2fa944203d2d73c404
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270582"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Azure Logic Apps’te iş akışı eylemlerini veya işlem dizilerini tekrarlayacak döngüler oluşturma

Mantık uygulamanızdaki bir diziyi işlemek için bir ["Foreach" döngüsü](#foreach-loop)oluşturabilirsiniz. Bu döngü, dizideki her öğe üzerinde bir veya daha fazla eylemi yineler. "Foreach" döngülerinin işlebildiği dizi öğesi sayısının sınırları [için, Sınırlar ve yapılandırmaya](../logic-apps/logic-apps-limits-and-config.md)bakın. 

Bir koşul karşılanana veya durum değişene kadar eylemleri yinelemek için bir ["Until" döngüsü](#until-loop)oluşturabilirsiniz. Mantık uygulamanız önce döngü içindeki tüm eylemleri çalıştırır ve ardından durumu veya durumu denetler. Koşul karşılanırsa, döngü durur. Aksi takdirde, döngü yineler. Bir mantık uygulaması nda "Until" döngülerinin sayısının sınırları [için, Sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md)ya bakın. 

> [!TIP]
> Bir dizi alan bir tetikleyiciniz varsa ve her dizi öğesi için bir iş akışı çalıştırmak istiyorsanız, bu diziyi [ **SplitOn** tetikleyici özelliğiyle](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) *ayırabilirsiniz.* 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>"Foreach" döngüsü

"Foreach döngüsü" her dizi öğesinde bir veya daha fazla eylemi yineler ve yalnızca diziler üzerinde çalışır. "Foreach" döngüsündeki yinelemeler paralel olarak çalışır. Ancak, sıralı bir ["Foreach" döngüsü](#sequential-foreach-loop)ayarlayarak yinelemeleri teker teker çalıştırabilirsiniz. 

"Foreach" döngülerini kullandığınızda dikkat edilmesi gereken bazı noktalar şunlardır:

* İç içe döngülerde yinelemeler her zaman paralel olarak değil, sırayla çalışır. İç içe bir döngüdeki öğeler için işlemleri paralel olarak çalıştırmak için bir alt mantık uygulaması oluşturun ve [çağırın.](../logic-apps/logic-apps-http-endpoint.md)

* Her döngü yinelemesi sırasında değişkenler üzerindeki işlemlerden öngörülebilir sonuçlar almak için, bu döngüleri sırayla çalıştırın. Örneğin, aynı anda çalışan bir döngü sona erdiğinde, değişken işlemlere ek artış, decrement ve ek tahmin edilebilir sonuçlar döndürür. Ancak, aynı anda çalışan döngüdeki her yineleme sırasında, bu işlemler öngörülemeyen sonuçlar döndürebilir. 

* "Foreach" döngüsündeki eylemler,[`@item()`](../logic-apps/workflow-definition-language-functions-reference.md#item) 
dizideki her öğeyi referans ve işleme ifadesi. Dizide olmayan verileri belirtirseniz, mantık uygulaması iş akışı başarısız olur. 

Bu örnek mantık uygulaması, bir web sitesi RSS akışı için günlük bir özet gönderir. Uygulama, her yeni öğe için bir e-posta gönderen bir "Foreach" döngüsü kullanır.

1. [Bu örnek mantık uygulamasını](../logic-apps/quickstart-create-first-logic-app-workflow.md) Outlook.com veya Office 365 Outlook hesabıyla oluşturun.

2. RSS tetikleyicisi ve e-posta eylemi göndermek arasında bir "Foreach" döngüsü ekleyin. 

   1. Adımlar arasında bir döngü eklemek için işaretçinizin bu adımlar arasında okun üzerine taşıyın. 
   Görünen **artı işaretini** (**+**) seçin, ardından **eylem ekle'yi**seçin.

      !["Eylem ekle" seçeneğini belirleyin](media/logic-apps-control-flow-loops/add-for-each-loop.png)

   1. Arama kutusunun **altında, Tümü'ni**seçin. Arama kutusuna filtreniz olarak "her biri için" yazın. Eylemler listesinden şu eylemi seçin: **Her biri için - Denetim**

      !["Her biri için" döngüsü ekle](media/logic-apps-control-flow-loops/select-for-each.png)

3. Şimdi döngüyü oluşturun. **Dinamik içerik** ekle listesi görüntüledikten sonra **önceki adımlardan bir çıktı seçin** altında, RSS tetikleyicisinden çıktı olan Feed **bağlantıları** dizisini seçin. 

   ![Dinamik içerik listesinden seçim](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Yalnızca önceki adımdan *dizi* çıktılarını seçebilirsiniz.

   Seçili dizi şimdi burada görünür:

   ![Dizi seçin](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Her dizi öğesi üzerinde bir eylem çalıştırmak için, **bir e-posta gönder** eylemini döngüye sürükleyin. 

   Mantık uygulamanız şu örneğe benzer:

   !["Foreach" döngüsüne adımlar ekleme](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Mantıksal uygulamanızı kaydedin. Mantık uygulamanızı tasarımcı araç çubuğunda el ile test etmek için **Çalıştır'ı**seçin.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>"Foreach" döngü tanımı (JSON)

Mantık uygulamanız için kod görünümünde çalışıyorsanız, mantık `Foreach` uygulamanızın JSON tanımındaki döngüyü tanımlayabilirsiniz, örneğin:

``` json
"actions": {
   "myForEachLoopName": {
      "type": "Foreach",
      "actions": {
         "Send_an_email": {
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "@{item()}",
                  "Subject": "New CNN post @{triggerBody()?['publishDate']}",
                  "To": "me@contoso.com"
               },
               "host": {
                  "api": {
                     "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/office365"
                  },
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['links']",
      "runAfter": {}
   }
}
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>"Foreach" döngüsü: Sıralı

Varsayılan olarak, "Foreach" döngüsü paralel olarak çalışır. Her döngüyü sırayla çalıştırmak için döngünün **Sıralı** seçeneğini ayarlayın. "Foreach" döngüleri, öngörülebilir sonuçlar beklediğiniz döngülerin veya değişkenlerin içine içe geçtiyseniz sırayla çalıştırılmalıdır. 

1. Döngünün sağ üst köşesinde, **elipsleri** seçin (**...**) **> Ayarları**.

   !["Foreach" döngüsünde "Ayarlar" > "..." seçeneğini belirleyin](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

1. **Eşzamanlılık Denetimi**altında, **Eşzamanlılık Denetimi** ayarını **Açık'a**çevirin. **Paralellik derecesini** **1'e**taşıyın ve **Bitti'yi**seçin.

   ![Eşzamanlılık denetimini açma](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Mantık uygulamanızın JSON tanımıyla çalışıyorsanız, örneğin parametreyi `Sequential` `operationOptions` ekleyerek seçeneği kullanabilirsiniz:

``` json
"actions": {
   "myForEachLoopName": {
      "type": "Foreach",
      "actions": {
         "Send_an_email": { }
      },
      "foreach": "@triggerBody()?['links']",
      "runAfter": {},
      "operationOptions": "Sequential"
   }
}
```

<a name="until-loop"></a>

## <a name="until-loop"></a>"Until" döngüsü
  
Bir koşul karşılanana veya durum değişene kadar eylemleri çalıştırmak ve yinelemek için, bu eylemleri bir "Until" döngüsüne koyun. Mantık uygulamanız önce döngü içindeki tüm eylemleri çalıştırır ve ardından durumu veya durumu denetler. Koşul karşılanırsa, döngü durur. Aksi takdirde, döngü yineler.

"Until" döngüsü kullanabileceğiniz bazı yaygın senaryolar şunlardır:

* İstediğinbir son nokta çağırın.

* Veritabanında bir kayıt oluşturun. O kayıttaki belirli bir alanın onaylanmasını bekleyin. İşleme devam edin. 

Her gün SABAH 8:00'den itibaren bu örnek mantık uygulaması, değişkenin değeri 10'a eşit olana kadar bir değişkeni arttirıyor. Mantık uygulaması daha sonra geçerli değeri doğrulayan bir e-posta gönderir. 

> [!NOTE]
> Bu adımlar Office 365 Outlook'u kullanır, ancak Logic Apps'ın desteklediği herhangi bir e-posta sağlayıcısını kullanabilirsiniz. 
> [Konektörler listesini buradan kontrol edin.](https://docs.microsoft.com/connectors/) Başka bir e-posta hesabı kullanıyorsanız, genel adımlar aynı kalır, ancak uI'niz biraz farklı görünebilir. 

1. Boş bir mantıksal uygulama oluşturma. Mantık App Designer, arama kutusunun altında, **Tüm**seçin. "Yineleme"yi arayın. 
   Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Yineleme - Zamanlama**

   !["Yineleme - Zamanlama" tetikleyicisi ekleme](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

1. Günün aralığını, sıklığını ve saatini ayarlayarak tetikleyicinin ne zaman ateşlediğini belirtin. Saati ayarlamak için **gelişmiş seçenekleri göster'i**seçin.

   ![Yineleme zamanlamasını ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Özellik | Değer |
   | -------- | ----- |
   | **Interval** | 1 | 
   | **Frequency** | Gün |
   | **Şu saatlerde** | 8 |
   ||| 

1. Tetikleyicinin altında **Yeni adımı**seçin. 
   "Değişkenleri" arayın ve bu eylemi seçin: **Değişkeni başlatma - Değişkenler**

   !["Değişkeni başlatma - Değişkenler" eylemini ekleme](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

1. Değişkeninizi bu değerlerle ayarlayın:

   ![Değişken özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Özellik | Değer | Açıklama |
   | -------- | ----- | ----------- |
   | **Adı** | Sınır | Değişkeninizin adı | 
   | **Tür** | Tamsayı | Değişkeninizin veri türü | 
   | **Değer** | 0 | Değişkeninizin başlangıç değeri | 
   |||| 

1. **Initialize değişken** eylemi altında **Yeni adımı**seçin. 

1. Arama kutusunun **altında, Tümü'ni**seçin. "Until" arama ve bu eylemi seçin: **Kadar - Denetim**

   !["Until" döngüsü ekle](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

1. **Limit** değişkenini seçerek döngünün çıkış koşulunu oluşturun ve eşit **işleçtir.** 
   Karşılaştırma değeri olarak **10** girin.

   ![Döngüyü durdurmak için çıkış koşulu oluşturma](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

1. Döngü içinde **eylem ekle'yi**seçin. 

1. Arama kutusunun **altında, Tümü'ni**seçin. "Değişkenler" için arama yapın ve bu eylemi seçin: **Artış değişkeni - Değişkenler**

   ![Artan değişken için eylem ekleme](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

1. **Ad**için Limit **değişkenini** seçin. **Değer**için "1" girin. 

     ![Artış "Limit" ile 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

1. Döngünün dışında ve altında, **Yeni adımı**seçin. 

1. Arama kutusunun **altında, Tümü'ni**seçin. 
     E-posta gönderen bir eylem bulma ve ekleme, örneğin: 

     ![E-posta gönderen eylem ekleme](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

1. İstenirse, e-posta hesabınızda oturum açın.

1. E-posta eyleminin özelliklerini ayarlayın. Konuya **Limit** değişkenini ekleyin. Bu şekilde, değişkenin geçerli değerinin belirtilen koşulun karşılığını onaylayabilirsiniz, örneğin:

      ![E-posta özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

      | Özellik | Değer | Açıklama |
      | -------- | ----- | ----------- | 
      | **Hedef** | *\<e-posta\@adresi etki alanı>* | Alıcının e-posta adresi. Test etmek için kendi e-posta adresinizi kullanın. | 
      | **Konu** | "Limit" için geçerli değer **Limittir** | E-posta konusunu belirtin. Bu örnekte, **Sınır** değişkenini eklediğinizden emin olun. | 
      | **Gövde** | <*e-posta içeriği*> | Göndermek istediğiniz e-posta iletisi içeriğini belirtin. Bu örnekte, istediğiniz metni girin. | 
      |||| 

1. Mantıksal uygulamanızı kaydedin. Mantık uygulamanızı tasarımcı araç çubuğunda el ile test etmek için **Çalıştır'ı**seçin.

      Mantığınız çalışmaya başladıktan sonra, belirttiğiniz içeriği içeren bir e-posta alırsınız:

      ![Alınan e-posta](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Sonsuz döngüleri önleme

"Kadar" döngüsü, bu koşullardan herhangi biri gerçekleşirse yürütmeyi durduran varsayılan sınırlara sahiptir:

| Özellik | Varsayılan değer | Açıklama | 
| -------- | ------------- | ----------- | 
| **Sayısı** | 60 | Döngü çıkmadan önce çalışan en yüksek döngü sayısı. Varsayılan değer 60 döngüdür. | 
| **Zaman aşımı** | PT1H | Döngü çıkmadan önce bir döngüçalıştırmak için en fazla zaman. Varsayılan değer bir saattir ve ISO 8601 biçiminde belirtilir. <p>Zaman açıkçağı değiştirme değiştirilir. Döngüdeki herhangi bir eylem zaman aşımı sınırından daha uzun sürüyorsa, geçerli döngü durmuyor. Ancak, sınır koşulu karşılanmadığından sonraki döngü başlatılamıyor. | 
|||| 

Bu varsayılan sınırları değiştirmek için döngü eylem şeklinde **gelişmiş seçenekleri göster'i** seçin.

<a name="until-json"></a>

## <a name="until-definition-json"></a>"Until" tanımı (JSON)

Mantık uygulamanız için kod görünümünde çalışıyorsanız, mantık `Until` uygulamanızın JSON tanımında bir döngü tanımlayabilirsiniz, örneğin:

``` json
"actions": {
   "Initialize_variable": {
      // Definition for initialize variable action
   },
   "Send_an_email": {
      // Definition for send email action
   },
   "Until": {
      "type": "Until",
      "actions": {
         "Increment_variable": {
            "type": "IncrementVariable",
            "inputs": {
               "name": "Limit",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "expression": "@equals(variables('Limit'), 10)",
      // To prevent endless loops, an "Until" loop 
      // includes these default limits that stop the loop. 
      "limit": { 
         "count": 60,
         "timeout": "PT1H"
      },
      "runAfter": {
         "Initialize_variable": [
            "Succeeded"
         ]
      }
   }
}
```

Bu örnek "Kadar" döngüsü, bir kaynak oluşturan bir HTTP bitiş noktası çağırır. HTTP yanıt gövdesi durumla birlikte `Completed` döndüğünde döngü durur. Sonsuz döngüleri önlemek için, bu koşullardan herhangi biri gerçekleşirse döngü de durur:

* Döngü öznitelik tarafından `count` belirtilen 10 kat koştu. Varsayılan değer 60 kattır. 

* Döngü, ISO 8601 `timeout` biçimindeöz tarafından belirtildiği şekilde iki saat boyunca çalıştırılabı. Varsayılan değer bir saattir.
  
``` json
"actions": {
   "myUntilLoopName": {
      "type": "Until",
      "actions": {
         "Create_new_resource": {
            "type": "Http",
            "inputs": {
               "body": {
                  "resourceId": "@triggerBody()"
               },
               "url": "https://domain.com/provisionResource/create-resource",
               "body": {
                  "resourceId": "@triggerBody()"
               }
            },
            "runAfter": {},
            "type": "ApiConnection"
         }
      },
      "expression": "@equals(triggerBody(), 'Completed')",
      "limit": {
         "count": 10,
         "timeout": "PT2H"
      },
      "runAfter": {}
   }
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Azure [Logic Apps kullanıcı geri bildirim sitesi,](https://aka.ms/logicapps-wish)özellikleri ve önerileri göndermek veya oylamak için.

## <a name="next-steps"></a>Sonraki adımlar

* [Koşula dayalı adımları çalıştırma (koşullu ifadeler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere dayalı adımları çalıştırma (geçiş deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplanmış eylem durumuna (kapsamlara) göre adımları çalıştırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
