---
title: İş akışlarına koşullu deyimler ekleme
description: Azure Logic Apps'ta iş akışlarında eylemleri denetleyen koşullar oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: fe79cf5af86e1f303e4735214b993d8db4488a25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74793259"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Azure Logic Apps'ta iş akışı eylemlerini kontrol eden koşullu deyimler oluşturma

Yalnızca belirli bir koşulu geçtikten sonra mantık uygulamanızda belirli eylemleri çalıştırmak için *koşullu*bir ifade ekleyin. Bu denetim yapısı, iş akışınızdaki verileri belirli değerlerveya alanlar ile karşılaştırır. Daha sonra, verilerin koşulu karşılayıp karşılamadığını temel alan farklı eylemler belirtebilirsiniz. Koşulları birbiriniz içiniçleyebilir.

Örneğin, bir web sitesinin RSS akışında yeni öğeler göründüğünde çok fazla e-posta gönderen bir mantık uygulamanız olduğunu varsayalım. Yalnızca yeni öğe belirli bir dize içerdiğinde e-posta göndermek için koşullu bir ifade ekleyebilirsiniz. 

> [!TIP]
> Farklı belirli değerlere dayalı farklı adımlar çalıştırmak için, bunun yerine bir [*anahtar deyimi*](../logic-apps/logic-apps-control-flow-switch-statement.md) kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Bu makaledeki örneği izlemek için, Outlook.com veya Office 365 Outlook hesabı içeren [bu örnek mantık uygulamasını oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="add-condition"></a>Koşul ekleme

1. Azure <a href="https://portal.azure.com" target="_blank">portalında</a>mantık uygulamanızı Logic App Designer'da açın.

1. İstediğinize konumda bir koşul ekleyin. 

   Adımlar arasında bir koşul eklemek için, durumu eklemek istediğiniz okun üzerine işaretçiyi taşıyın. Görünen **artı işaretini** (**+**) seçin, ardından **eylem ekle'yi**seçin. Örnek:

   ![Adımlar arasında eylem ekleme](./media/logic-apps-control-flow-conditional-statement/add-action.png)

   İş akışınızın sonuna bir koşul eklemek istediğinizde, mantık uygulamanızın en altında, **Yeni adım** > **bir eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "koşul" girin. Bu eylemi seçin: **Koşul - Denetim**

   ![Koşul ekleme](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

1. **Durum** kutusunda, durumunuzu oluşturun. 

   1. Sol kutuda, karşılaştırmak istediğiniz verileri veya alanı belirtin.

      Sol kutunun içini tıklattığınızda, dinamik içerik listesi görüntülenir, böylece mantık uygulamanızdaki önceki adımlardan çıktıları seçebilirsiniz. 
      Bu örnekte, RSS özetini seçin.

      ![Durumunuzu oluşturun](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   1. Orta kutuda, gerçekleştirmek için işlemi seçin. 
   Bu örnek için , " "**seçeneğini belirleyin" içerir.** 

   1. Sağ kutuda, ölçütleriniz olarak bir değer veya alan belirtin. 
   Bu örnekte, şu dizeyi belirtin: **Microsoft**

   İşte tam durum:

   ![Tamamlanan koşul](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

   Durumunuza başka bir satır eklemek için**Ekle satırını** **seçin.** >  
   Alt koşullara sahip bir grup eklemek için**Ekle grubu'nı** **seçin.** >  
   Varolan satırları gruplandırmak için, bu satırların onay kutularını seçin, herhangi bir satır için elips (...) düğmesini seçin ve ardından **grubu yap'ı**seçin.

1. **Altında Eğer true** ve **False,** koşul karşılanıp karşılanmadığına bağlı olarak gerçekleştirmek için adımları ekleyin. Örnek:

   !["Eğer doğruysa" ve "Yanlışsa" yollarında koşul](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Varolan eylemleri If **true** ve **If false yollarına** sürükleyebilirsiniz.

1. Mantıksal uygulamanızı kaydedin.

Bu mantık uygulaması artık yalnızca RSS akışındaki yeni öğeler durumunuzu karşıladığında posta gönderir.

## <a name="json-definition"></a>JSON tanımı

Koşullu bir deyimin arkasındaki üst düzey kod tanımı aşağıda verilmiştir:

``` json
"actions": {
  "Condition": {
    "type": "If",
    "actions": {
      "Send_an_email": {
        "inputs": {},
        "runAfter": {}
    },
    "expression": {
      "and": [ 
        { 
          "contains": [ 
            "@triggerBody()?['summary']", 
            "Microsoft"
          ]
        } 
      ]
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özellikler ve öneriler göndermek veya oy kullanmak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Farklı değerlere dayalı adımları çalıştırma (geçiş deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Adımları çalıştırma ve yineleme (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplanmış eylem durumuna (kapsamlara) göre adımları çalıştırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
