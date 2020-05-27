---
title: İş akışlarında eylemler için paralel dallar oluşturun veya ekleyin
description: Azure Logic Apps içinde bağımsız iş akışı eylemleri için paralel çalışan dallar oluşturmayı veya birleştirmeyi öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: f857449d4511b6ae0a5a25bf7aca9e1abc1ae7c0
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83833702"
---
# <a name="create-or-join-parallel-branches-for-workflow-actions-in-azure-logic-apps"></a>Azure Logic Apps iş akışı eylemleri için paralel dallar oluşturun veya ekleyin

Varsayılan olarak, mantıksal uygulama iş akışlarındaki eylemleriniz sırayla çalışır. Aynı anda bağımsız eylemler gerçekleştirmek için [paralel dallar](#parallel-branches)oluşturabilir ve ardından [Bu dalları](#join-branches) daha sonra akışınız içinde birleştirebilirsiniz. 

> [!TIP] 
> Bir diziyi alan ve her dizi öğesi için bir iş akışı çalıştırmak istediğiniz bir Tetikleyiciniz varsa, bu diziyi [ **spton** tetikleyici özelliğiyle](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) *toplu* olarak silebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

<a name="parallel-branches"></a>

## <a name="add-parallel-branch"></a>Paralel dal Ekle

Bağımsız adımları aynı anda çalıştırmak için, var olan bir adımın yanına paralel dallar ekleyebilirsiniz. 

![Adımları paralel olarak çalıştır](media/logic-apps-control-flow-branches/parallel.png)

Mantıksal uygulamanız, iş akışına devam etmeden önce tüm dalların bitmesini bekler. Paralel dallar yalnızca `runAfter` özellik değerleri tamamlanan üst adımın durumuyla eşleşiyorsa çalışır. Örneğin, her ikisi `branchAction1` de `branchAction2` yalnızca durumu tamamlandığında çalıştırılacak şekilde ayarlanır `parentAction` `Succeeded` .

> [!NOTE]
> Başlamadan önce mantıksal uygulamanızda, paralel dallar ekleyebileceğiniz bir adım zaten olmalıdır.

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a>mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. İşaretçinizi, paralel dallar eklemek istediğiniz adımın üzerindeki oka taşıyın. Görüntülenen **artı** işaretini ( **+** ) seçin ve ardından **paralel dal Ekle**' yi seçin. 

   ![Paralel dal Ekle](media/logic-apps-control-flow-branches/add-parallel-branch.png)

1. Arama kutusunda istediğiniz eylemi bulun ve seçin.

   ![İstediğiniz eylemi bulun ve seçin](media/logic-apps-control-flow-branches/find-select-parallel-action.png)

   Seçtiğiniz eylem artık paralel dalda görüntülenir, örneğin:

   ![İstediğiniz eylemi bulun ve seçin](media/logic-apps-control-flow-branches/added-parallel-branch.png)

1. Şimdi, her bir paralel dalda istediğiniz adımları ekleyin. Bir dala başka bir eylem eklemek için, işaretçinizi sıralı bir eylem eklemek istediğiniz eylemin altına taşıyın. Görüntülenen **artı** ( **+** ) işaretini seçin ve ardından **Eylem Ekle**' yi seçin.

   ![Paralel dala sıralı eylem ekleme](media/logic-apps-control-flow-branches/add-sequential-action.png)

1. Arama kutusunda istediğiniz eylemi bulun ve seçin.

   ![Sıralı eylem bulma ve seçme](media/logic-apps-control-flow-branches/find-select-sequential-action.png)

   Seçtiğiniz eylem şu anda geçerli dalı içinde görünür, örneğin:

   ![Sıralı eylem bulma ve seçme](media/logic-apps-control-flow-branches/added-sequential-action.png)

Dalları birlikte yeniden birleştirmek için, [paralel dallarınıza katın](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Paralel dal tanımı (JSON)

Kod görünümünde çalışıyorsanız, paralel yapıyı mantıksal uygulamanızın JSON tanımında tanımlayabilir, örneğin:

``` json
{
  "triggers": {
    "myTrigger": {}
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": {},
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<a name="join-branches"></a>

## <a name="join-parallel-branches"></a>Paralel dalları birleştirin

Paralel dalları birlikte birleştirmek için, tüm dallar altındaki en alta bir adım eklemeniz yeterlidir. Bu adım, tüm paralel dallar çalışmaya başladıktan sonra çalışır.

![Paralel dalları birleştirin](media/logic-apps-control-flow-branches/join.png)

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı bulun ve açın. 

1. Katmak istediğiniz paralel dallar altında **yeni adım**' ı seçin. 

   ![Birleşime adım Ekle](media/logic-apps-control-flow-branches/add-join-step.png)

1. Arama kutusunda, dalları birleştiren adım olarak istediğiniz eylemi bulun ve seçin.

   ![Paralel dalları birleştiren eylemi bulun ve seçin](media/logic-apps-control-flow-branches/join-steps.png)

   Paralel dallarınız artık birleştirilir.

   ![Birleştirilmiş dallar](media/logic-apps-control-flow-branches/joined-branches.png)

<a name="join-json"></a>

## <a name="join-definition-json"></a>JOIN tanımı (JSON)

Kod görünümünde çalışıyorsanız, bunun yerine mantıksal uygulamanızın JSON tanımında JOIN yapısını tanımlayabilirsiniz, örneğin:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "joinAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "branchAction1": [
          "Succeeded"
        ],
        "branchAction2": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="get-support"></a>Destek alın

* Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-logic-apps.html)ziyaret edin.
* Özellikleri ve önerileri göndermek veya Oylamak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre adımları çalıştırın (koşullu deyimler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere göre adımları Çalıştır (Switch deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştır ve Yinele adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Gruplanmış eylem durumu (kapsamlar) temelinde adımları Çalıştır](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
