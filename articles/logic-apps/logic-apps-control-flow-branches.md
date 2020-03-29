---
title: İş akışlarındaki eylemler için paralel dallar oluşturma veya birleştirme
description: Azure Logic Apps'ta bağımsız iş akışı eylemleri için paralel çalışan dallar oluşturma veya birleştirme hakkında bilgi edinin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: c0b1519992ba930382a1987aed185ef3c92eded4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75453425"
---
# <a name="create-or-join-parallel-branches-for-workflow-actions-in-azure-logic-apps"></a>Azure Logic Apps'ta iş akışı eylemleri için paralel dallar oluşturma veya birleştirme

Varsayılan olarak, mantık uygulaması iş akışlarındaki eylemleriniz sırayla çalışır. Bağımsız eylemleri aynı anda gerçekleştirmek için [paralel dallar](#parallel-branches)oluşturabilir ve daha sonra akışınızda [bu dalları birleştirebilirsiniz.](#join-branches) 

> [!TIP] 
> Bir dizi alan bir tetikleyiciniz varsa ve her dizi öğesi için bir iş akışı çalıştırmak istiyorsanız, bu diziyi [ **SplitOn** tetikleyici özelliğiyle](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) *ayırabilirsiniz.*

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

<a name="parallel-branches"></a>

## <a name="add-parallel-branch"></a>Paralel dal ekleme

Bağımsız adımları aynı anda çalıştırmak için, varolan bir adımın yanına paralel dallar ekleyebilirsiniz. 

![Adımları paralel olarak çalıştırma](media/logic-apps-control-flow-branches/parallel.png)

Mantık uygulamanız, iş akışına devam etmeden önce tüm dalların tamamlanmasını bekler. Paralel dallar `runAfter` yalnızca özellik değerleri tamamlanan üst adımın durumuyla eşleştiğinde çalışır. Örneğin, her `branchAction1` `branchAction2` ikisi de ve yalnızca `parentAction` durum `Succeeded` ile tamamlandığında çalışacak şekilde ayarlanır.

> [!NOTE]
> Başlamadan önce, mantık uygulamanızın zaten paralel dallar ekleyebileceğiniz bir adım alabilmeleri gerekir.

1. Azure <a href="https://portal.azure.com" target="_blank">portalında</a>mantık uygulamanızı Logic App Designer'da açın.

1. İşaretçini, paralel dallar eklemek istediğiniz adımın üzerindeki okun üzerine taşıyın. Görünen **artı** işaretini (**+**) seçin ve sonra paralel dal **ekle'yi**seçin. 

   ![Paralel dal ekleme](media/logic-apps-control-flow-branches/add-parallel-branch.png)

1. Arama kutusunda, istediğiniz eylemi bulun ve seçin.

   ![İstediğiniz eylemi bulun ve seçin](media/logic-apps-control-flow-branches/find-select-parallel-action.png)

   Seçtiğiniz eylem şimdi paralel dalda görünür, örneğin:

   ![İstediğiniz eylemi bulun ve seçin](media/logic-apps-control-flow-branches/added-parallel-branch.png)

1. Şimdi, her paralel dalda, istediğiniz adımları ekleyin. Bir dala başka bir eylem eklemek için, işaretçinizin sıralı eylem eklemek istediğiniz eylemin altına taşıyın. Görünen **artı** **+**( ) işaretini seçin ve ardından **eylem ekle'yi**seçin.

   ![Paralel dala sıralı eylem ekleme](media/logic-apps-control-flow-branches/add-sequential-action.png)

1. Arama kutusunda, istediğiniz eylemi bulun ve seçin.

   ![Ardışık eylemi bulma ve seçme](media/logic-apps-control-flow-branches/find-select-sequential-action.png)

   Seçili eyleminiz artık geçerli dalda görünür, örneğin:

   ![Ardışık eylemi bulma ve seçme](media/logic-apps-control-flow-branches/added-sequential-action.png)

Dalları tekrar birleştirmek için [paralel dallarıbirleştirin.](#join-branches) 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Paralel dal tanımı (JSON)

Kod görünümünde çalışıyorsanız, mantık uygulamanızın JSON tanımındaki paralel yapıyı tanımlayabilirsiniz, örneğin:

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

## <a name="join-parallel-branches"></a>Paralel dalları birleştirme

Paralel dalları birleştirmek için, tüm dalların altına bir adım eklemeniz gerekiyor. Bu adım, tüm paralel dalları çalışan bittikten sonra çalışır.

![Paralel dalları birleştirme](media/logic-apps-control-flow-branches/join.png)

1. Azure [portalında](https://portal.azure.com)Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı bulun ve açın. 

1. Katılmak istediğiniz paralel dalların altında **Yeni adımı**seçin. 

   ![Katılmak için adım ekleme](media/logic-apps-control-flow-branches/add-join-step.png)

1. Arama kutusunda, dalları birleştiren adım olarak istediğiniz eylemi bulun ve seçin.

   ![Paralel dalları birleştiren eylemi bulma ve seçme](media/logic-apps-control-flow-branches/join-steps.png)

   Paralel dallarınız artık birleştirildi.

   ![Birleştirilmiş şubeler](media/logic-apps-control-flow-branches/joined-branches.png)

<a name="join-json"></a>

## <a name="join-definition-json"></a>Birleştirme tanımı (JSON)

Kod görünümünde çalışıyorsanız, mantık uygulamanızın JSON tanımında birleştirme yapısını tanımlayabilirsiniz, örneğin:

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

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özellikler ve öneriler göndermek veya oy kullanmak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Koşula dayalı adımları çalıştırma (koşullu ifadeler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere dayalı adımları çalıştırma (geçiş deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Adımları çalıştırma ve yineleme (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Gruplanmış eylem durumuna (kapsamlara) göre adımları çalıştırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
