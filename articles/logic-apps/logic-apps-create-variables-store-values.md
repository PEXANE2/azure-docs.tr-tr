---
title: Değerleri depolamak ve aktarmak için değişkenler oluşturma ve yönetme
description: Azure Logic Apps ile oluşturduğunuz otomatik görevlerinizdeki ve iş akışınızdaki değişkenleri kullanarak değerleri nasıl depoladığınızı, yöneteceklerinizi, kullanacağınızı ve geçeceğinizi öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 55984082a6b287e9f7cdca005a24ef3c18032491
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456687"
---
# <a name="store-and-manage-values-by-using-variables-in-azure-logic-apps"></a>Azure Mantık Uygulamaları'ndaki değişkenleri kullanarak değerleri depolama ve yönetme

Bu makalede, mantık uygulamanızda değerleri depolamak için kullandığınız değişkenlerin nasıl oluşturulup bunlarla çalışacağı gösterilmektedir. Örneğin, değişkenler bir döngünün kaç kez çalıştığını izlemenize yardımcı olabilir. Bir dizi üzerinde yeniden doğrulamak veya belirli bir öğe için bir diziyi denetlemek için, her dizi öğesi için dizin numarasına başvurmak için bir değişken kullanabilirsiniz.

Tamsayı, float, boolean, string, dizi ve nesne gibi veri türleri için değişkenler oluşturabilirsiniz. Bir değişken oluşturduktan sonra, örneğin diğer görevleri gerçekleştirebilirsiniz:

* Değişkenin değerini alın veya başvurun.
* Değişkeni, artış ve *decrement* olarak da bilinen *decrement*sabit bir değerle artırma veya azaltma.
* Değişkene farklı bir değer atayın.
* Değişkenin *append* değerini bir dize veya diziye son kez ekle yat.

Değişkenler var ve yalnızca bunları oluşturan mantık uygulaması örneği içinde küreseldir. Ayrıca, bir mantık uygulaması örneği içinde herhangi bir döngü yinelemeler üzerinde devam. Bir değişkene başvururken, eylemin çıktılarına başvurmanın olağan yolu olan eylemin adını değil, belirteci olarak değişkenin adını kullanın.

> [!IMPORTANT]
> Varsayılan olarak, "Her biri için" döngüsü paralel olarak çalışır. Döngüler halinde değişkenler kullandığınızda, değişkenlerin öngörülebilir sonuçlar döndürür diye döngüyü [sırayla](../logic-apps/logic-apps-control-flow-loops.md#sequential-foreach-loop) çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, ücretsiz [bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Değişkeni oluşturmak istediğiniz mantık uygulaması

  Mantık uygulamalarında yeniyseniz, [Azure Mantık Uygulamaları nedir?ve](../logic-apps/logic-apps-overview.md) [Quickstart'ı inceleyin: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Mantık uygulamanızın ilk adımı olarak bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts)

  Değişkenoluşturmak ve değişkenlerle çalışmak için eylemler eklemeden önce, mantık uygulamanızın bir tetikleyiciyle başlaması gerekir.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Değişkeni başlatma

Bir değişken oluşturabilir ve veri türünü ve başlangıç değerini mantık uygulamanızda tek bir eylem içinde bildirebilirsiniz. Değişkenleri kapsamlar, koşullar ve döngüler içinde değil, yalnızca genel düzeyde bildirebilirsiniz.

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın.

   Bu örnekte Azure portalı ve varolan bir tetikleyiciye sahip bir mantık uygulaması kullanır.

1. Mantık uygulamanızda, değişken eklemek istediğiniz adımın altında aşağıdaki adımlardan birini izleyin: 

   * Son adımın altında bir eylem eklemek için **Yeni adımı**seçin.

     ![Eylem ekleme](./media/logic-apps-create-variables-store-values/add-action.png)

   * Adımlar arasında bir eylem eklemek için farenizi bağlantı okunun üzerine**+** taşıyın, böylece artı işareti ( ) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Bir **eylem seçin**altında, arama `variables` kutusuna filtreniz olarak girin. Eylemler listesinden, **Değişkeni Başlatma'yı**seçin.

   ![Eylem seçin](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

1. Değişkeniniz hakkında aşağıda açıklandığı şekilde bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer |  Açıklama |
   |----------|----------|-------|--------------|
   | **Adı** | Evet | <*değişken adı*> | Artış değişkeninin adı |
   | **Tür** | Evet | <*değişken tipi*> | Değişkeniçin veri türü |
   | **Değer** | Hayır | <*başlangıç değeri*> | Değişkeninizin ilk değeri <p><p>**İpucu**: İsteğe bağlı olsa da, değişkeninizin başlangıç değerini her zaman bilmeniz için bu değeri en iyi yöntem olarak ayarlayın. |
   |||||

   Örnek:

   ![Değişkeni başlatma](./media/logic-apps-create-variables-store-values/initialize-variable.png)

1. Şimdi istediğiniz eylemleri eklemeye devam edin. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

Tasarımcıdan kod görünümü düzenleyicisine geçerseniz, JavaScript Nesne Gösterimi (JSON) biçiminde olan mantıksal uygulama tanımınızda **Initialize değişken** eyleminin görünme şekli aşağıda verilmiştir:

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   }
},
```

> [!NOTE]
> **Initialize değişken** ieylemi `variables` bir dizi olarak yapılandırılmış bir bölüme sahip olsa da, eylem aynı anda yalnızca bir değişken oluşturabilir. Her yeni değişken, ayrı bir **Başlangıç değişken** eylemi gerektirir.

Diğer bazı değişken türleri için örnekler aşağıda verilmiştir:

*String değişkeni*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myStringVariable",
               "type": "String",
               "value": "lorem ipsum"
          } ]
      },
      "runAfter": {}
   }
},
```

*Boolean değişkeni*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myBooleanVariable",
               "type": "Boolean",
               "value": false
          } ]
      },
      "runAfter": {}
   }
},
```

*Tümseklerle dizi*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": [1, 2, 3]
          } ]
      },
      "runAfter": {}
   }
},
```

*Dizeleri ile dizi*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": ["red", "orange", "yellow"]
          } ]
      },
      "runAfter": {}
   }
},
```

<a name="get-value"></a>

## <a name="get-the-variables-value"></a>Değişkenin değerini alma

Bir değişkenin içeriğini almak veya başvurmak için, Logic App Designer ve kod görünümü düzenleyicisindeki [değişkenler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#variables) de kullanabilirsiniz. Bir değişkene başvururken, eylemin çıktılarına başvurmanın olağan yolu olan eylemin adını değil, belirteci olarak değişkenin adını kullanın.

Örneğin, bu ifade `variables()` işlevi kullanarak bu makalede daha önce [oluşturulan](#append-value) dizi değişkeninden öğeleri alır. İşlev, `string()` değişkenin içeriğini dize biçiminde döndürür:`"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Artış değişkeni 

Bir değişkeni sabit bir değerle artırmak veya *artırmak* için, mantık uygulamanıza **Artış değişkeni** eylemini ekleyin. Bu eylem yalnızca tamsayı ve float değişkenleriyle çalışır.

1. Mantık Uygulama Tasarımcısı'nda, varolan bir değişkeni artırmak istediğiniz adımın altında **Yeni adımı**seçin. 

   Örneğin, bu mantık uygulaması zaten bir tetikleyici ve bir değişken oluşturulan bir eylem vardır. Bu nedenle, bu adımların altında yeni bir eylem ekleyin:

   ![Eylem ekleme](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Varolan adımlar arasında eylem eklemek için farenizi bağlantı okunun üzerine taşıyın, böylece artı işareti (+) görünür. Artı işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "artış değişkeni" girin. Eylemler listesinde, **Artış değişkenini**seçin.

   !["Artış değişkeni" eylemini seçin](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

1. Değişkeninizi artımiçin bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer |  Açıklama |
   |----------|----------|-------|--------------|
   | **Adı** | Evet | <*değişken adı*> | Artış değişkeninin adı |
   | **Değer** | Hayır | <*artış değeri*> | Değişkeni artımiçin kullanılan değer. Varsayılan değer birdir. <p><p>**İpucu**: İsteğe bağlı olsa da, değişkeninizi artım için belirli değeri her zaman bilmeniz için bu değeri en iyi uygulama olarak ayarlayın. |
   ||||

   Örnek:

   ![Artış değeri örneği](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

Tasarımcıdan kod görünümü düzenleyicisine geçerseniz, JSON biçiminde olan mantık uygulama tanımınızda **Artış değişken** ieylemi şu şekilde görünür:

```json
"actions": {
   "Increment_variable": {
      "type": "IncrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

## <a name="example-create-loop-counter"></a>Örnek: Döngü sayacı oluşturma

Değişkenler genellikle bir döngünün kaç kez çalıştığını saymak için kullanılır. Bu örnek, e-postadaki ekleri sayan bir döngü oluşturarak bu görev için değişkenleri nasıl oluşturduğunuzu ve kullandığınızı gösterir.

1. Azure portalında boş bir mantık uygulaması oluşturun. Yeni e-posta ve ekleri denetleyen bir tetikleyici ekleyin.

   Bu örnek, **yeni bir e-posta geldiğinde**Office 365 Outlook tetikleyicisini kullanır. Bu tetikleyiciyi yalnızca e-postanın ekleri olduğunda ateş leşecek şekilde ayarlayabilirsiniz. Ancak, Outlook.com bağlayıcısı gibi ekleri olan yeni e-postaları kontrol eden tüm bağlayıcıları kullanabilirsiniz.

1. Tetikleyicide, ekleri denetlemek ve bu ekleri mantık uygulamanızın iş akışına geçirmek için, bu özellikler için **Evet'i** seçin:

   * **Eki Var**
   * **Ekleri Dahil Et**

   ![Ekleri kontrol edin ve ekleyin](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

1. [ **Initialize değişken** eylemini](#create-variable)ekleyin. Sıfır başlangıç değeri olan `Count` bir tamsayı değişkeni oluşturun.

   !["Değişkeni Başlatma" için eylem ekleme](./media/logic-apps-create-variables-store-values/initialize-variable.png)

1. Her ekte geçiş yapmak *için her* döngü için bir tane ekleyin.

   1. **İlkle etkin bir** hareket altında **Yeni adım'ı**seçin.

   1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna, `for each` arama filtreniz olarak girin ve **her biri için.**

      !["Her biri için" döngüsü ekleme](./media/logic-apps-create-variables-store-values/add-loop.png)

1. Döngüde, **önceki adımlardan çıktı seç** kutusunun içini tıklatın. Dinamik içerik listesi göründüğünde **Ekler'i**seçin.

   !["Ekler"i seçin](./media/logic-apps-create-variables-store-values/select-attachments.png)

   **Ekler** özelliği, tetikleyicinin çıktısından e-posta eklerini içeren bir diziyi döngünüze geçirir.

1. Her döngü **için** **eylem ekle'yi**seçin.

   !["Eylem ekle" seçeneğini belirleyin](./media/logic-apps-create-variables-store-values/add-action-2.png)

1. Arama kutusuna filtreniz olarak "artış değişkeni" girin. Eylemler listesinden **Artış değişkenini**seçin.

   > [!NOTE]
   > **Artış değişken** işlemlerinin döngü nüzünde göründüğünden emin olun. Eylem döngünün dışında görünüyorsa, eylemi döngüye sürükleyin.

1. **Ad** listesinden **Artış değişkeni** **eyleminde, Count** değişkenini seçin.

   !["Count" değişkenini seçin](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

1. Döngünün altında, ek sayısını gönderen herhangi bir eylem ekleyin. Eyleminize, **Örneğin, Count** değişkeninden gelen değeri ekleyin:

   ![Sonuç gönderen bir eylem ekleme](./media/logic-apps-create-variables-store-values/send-email-results.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

1. Mantık uygulamanız etkin değilse, mantık uygulaması menüsünde **Genel Bakış'ı**seçin. Araç çubuğunda **Etkinleştir'i**seçin.

1. Logic App Designer araç çubuğunda **Çalıştır'ı**seçin. Bu adım mantık uygulamanızı el ile başlatır.

1. Bu örnekte kullandığınız e-posta hesabına bir veya daha fazla ek içeren bir e-posta gönderin.

   Bu adım, mantık uygulamanızın iş akışı için bir örnek oluşturan ve çalıştıran mantık uygulamasının tetikleyicisini çalıştırAr. Sonuç olarak, mantık uygulaması size gönderdiğiniz e-postadaki ek sayısını gösteren bir ileti veya e-posta gönderir.

Tasarımcıdan kod görünümü düzenleyicisine geçerseniz, her döngü **için** JSON formatında olan mantık uygulama tanımınızdaki **Artış değişken** eylemiyle birlikte görünen yol burada dır.

```json
"actions": {
   "For_each": {
      "type": "Foreach",
      "actions": {
         "Increment_variable": {
           "type": "IncrementVariable",
            "inputs": {
               "name": "Count",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['Attachments']",
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="decrement-value"></a>

## <a name="decrement-variable"></a>Decrement değişkeni

Bir değişkeni sabit bir değerle azaltmak veya *azaltmak* için, bunun yerine Kararname **değişkeni eylemini** bulmanız ve seçmeniz dışında [bir değişkeni artırmak](#increment-value) için adımları izleyin. Bu eylem yalnızca tamsayı ve float değişkenleriyle çalışır.

**Kararname değişken** eyleminin özellikleri şunlardır:

| Özellik | Gerekli | Değer |  Açıklama |
|----------|----------|-------|--------------|
| **Adı** | Evet | <*değişken adı*> | Decrement için değişkenin adı | 
| **Değer** | Hayır | <*artış değeri*> | Değişkeni düşürme değeri. Varsayılan değer birdir. <p><p>**İpucu**: İsteğe bağlı olsa da, değişkeninizi düşürmenin belirli değerini her zaman bilmeniz için bu değeri en iyi yöntem olarak ayarlayın. |
||||| 

Tasarımcıdan kod görünümü düzenleyicisine geçerseniz, **Burada Decrement değişken** eylem JSON biçiminde olan mantık uygulaması tanımı içinde görünür yoludur.

```json
"actions": {
   "Decrement_variable": {
      "type": "DecrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

<a name="assign-value"></a>

## <a name="set-variable"></a>Değişkeni ayarlama

Varolan bir değişkene farklı bir değer atamak için, siz dışında [bir değişkeni artırmak](#increment-value) için aşağıdakiadımları izleyin:

1. Bunun yerine **Değişken Ayarla** eylemini bul ve seçin.

1. Atamak istediğiniz değişken adını ve değerini sağlayın. Hem yeni değer hem de değişken aynı veri türüne sahip olmalıdır. Bu eylemin varsayılan bir değeri olmadığından değer gereklidir.

**Set değişken** eyleminin özellikleri şunlardır:

| Özellik | Gerekli | Değer |  Açıklama |
|----------|----------|-------|--------------|
| **Adı** | Evet | <*değişken adı*> | Değişecek değişkenin adı |
| **Değer** | Evet | <*yeni değer*> | Değişkeni atamak istediğiniz değer. Her ikisi de aynı veri türüne sahip olmalıdır. |
||||| 

> [!NOTE]
> Değişkenleri artımlı veya azalan değilseniz, döngülerin içindeki değişkenleri değiştirmek, döngüler varsayılan olarak paralel veya aynı anda çalıştığı için beklenmeyen sonuçlar *oluşturabilir.* Bu gibi durumlarda, döngünüzün sırayla çalışacak şekilde ayarlanın. Örneğin, döngü içindeki değişken değerine başvurmak ve döngü örneğinin başında ve sonunda aynı değeri beklemek istediğinizde, döngünün nasıl çalıştığını değiştirmek için aşağıdaki adımları izleyin: 
>
> 1. Döngünüzün sağ üst köşesinde elipsis (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.
> 
> 2. **Eşzamanlılık Denetimi**altında, **Geçersiz Kılma Varsayılan** ayarını **Açık**olarak değiştirin.
>
> 3. **Paralellik kaydırıcısının derecesini** **1'e**sürükleyin.

Tasarımcıdan kod görünümü düzenleyicisine geçerseniz, **JSON** biçiminde olan mantıksal uygulama tanımınızda Set değişken eyleminin görünme şekli aşağıda verilmiştir. Bu örnek, `Count` değişkenin geçerli değerini başka bir değere değiştirir.

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   },
   "Set_variable": {
      "type": "SetVariable",
      "inputs": {
         "name": "Count",
         "value": 100
      },
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="append-value"></a>

## <a name="append-to-variable"></a>Değişkene ek

Dizeleri veya dizileri depolayan değişkenler için, bir değişkenin değerini bu dizelerveya dizilerde son öğe olarak ekleyebilir veya *ekleyebilirsiniz.* Bunun yerine aşağıdaki adımları izlemeniz dışında [bir değişkeni artırmak](#increment-value) için adımları izleyebilirsiniz: 

1. Değişkeninizin bir dize mi yoksa dizi mi olduğuna bağlı olarak bu eylemlerden birini bulun ve seçin: 

   * **String değişkenine ek**
   * **Dizi değişkenine ek** 

1. Dize veya dizideki son öğe olarak eklemek için değer sağlayın. Bu değer gereklidir.

İşte **Append için** özellikleri ... eylemler:

| Özellik | Gerekli | Değer |  Açıklama |
|----------|----------|-------|--------------|
| **Adı** | Evet | <*değişken adı*> | Değişecek değişkenin adı |
| **Değer** | Evet | <*ek değeri*> | Herhangi bir türe sahip olabilecek, eklemek istediğiniz değer |
|||||

Tasarımcıdan kod görünümü düzenleyicisine geçerseniz, Burada dizi değişken eylem **Append** JSON biçiminde olan mantık uygulaması tanımı içinde görünür yoludur. Bu örnek bir dizi değişkeni oluşturur ve dizideki son öğe olarak başka bir değer ekler. Sonucunuz, bu diziyi içeren güncelleştirilmiş bir değişkendir:`[1,2,3,"red"]`

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
            "name": "myArrayVariable",
            "type": "Array",
            "value": [1, 2, 3]
         } ]
      },
      "runAfter": {}
   },
   "Append_to_array_variable": {
      "type": "AppendToArrayVariable",
      "inputs": {
         "name": "myArrayVariable",
         "value": "red"
      },
      "runAfter": {
        "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

## <a name="next-steps"></a>Sonraki adımlar

* Logic [Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
