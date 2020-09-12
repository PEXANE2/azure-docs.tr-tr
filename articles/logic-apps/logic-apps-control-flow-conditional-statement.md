---
title: İş akışlarına koşullu deyimler ekleme
description: Azure Logic Apps iş akışlarında eylemleri denetleyen koşullar oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: 19f42270e37c42cf56bbde04f73c01027a56ff63
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657388"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Azure Logic Apps iş akışı eylemlerini denetleyen koşullu deyimler oluşturma

Mantıksal uygulamanızda belirli eylemleri yalnızca belirtilen bir koşulu geçirdikten sonra çalıştırmak için, *koşullu bir ifade*ekleyin. Bu denetim yapısı, iş akışınızdaki verileri belirli değerlerle veya alanlarla karşılaştırır. Daha sonra, verilerin koşulu karşılayıp karşılamadığını temel alarak çalışan farklı eylemler belirtebilirsiniz. Koşulları birbirine iç içe yerleştirebilirsiniz.

Örneğin, bir Web sitesinin RSS akışında yeni öğeler görüntülendiğinde çok fazla e-posta gönderen bir mantıksal uygulamanız olduğunu varsayalım. Yalnızca yeni öğe belirli bir dize içerdiğinde e-posta göndermek için koşullu bir ifade ekleyebilirsiniz. 

> [!TIP]
> Farklı belirli değerlere göre farklı adımlar çalıştırmak için bunun yerine [*switch ifadesini*](../logic-apps/logic-apps-control-flow-switch-statement.md) kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Bu makaledeki örneği izlemek için [Bu örnek mantıksal uygulamayı](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir Outlook.com hesabı ya da bir iş veya okul hesabıyla oluşturun.

## <a name="add-condition"></a>Koşul ekleme

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a>mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. İstediğiniz konuma bir koşul ekleyin. 

   Adımlar arasında bir koşul eklemek için işaretçiyi koşulu eklemek istediğiniz oka taşıyın. Görüntülenen **artı işaretini** () seçin ve **+** ardından **Eylem Ekle**' yi seçin. Örneğin:

   ![Adımlar arasında eylem ekleme](./media/logic-apps-control-flow-conditional-statement/add-action.png)

   İş akışınızın sonuna bir koşul eklemek istediğinizde, mantıksal uygulamanızın en altında  **yeni adım** > **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak "koşul" yazın. Şu eylemi seçin: **koşul denetimi**

   ![Koşul ekleme](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

1. **Koşul** kutusunda koşullarınızı oluşturun. 

   1. Sol taraftaki kutuda, karşılaştırmak istediğiniz verileri veya alanı belirtin.

      Sol kutunun içine tıkladığınızda, mantıksal uygulamanızdaki önceki adımlardan çıkış ' ı seçebilmeniz için dinamik içerik listesi görüntülenir. 
      Bu örnek için RSS Akış Özeti ' ni seçin.

      ![Koşullarınızı oluşturun](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   1. Ortadaki kutuda gerçekleştirilecek işlemi seçin. 
   Bu örnek için "**içerir**" seçeneğini belirleyin. 

   1. Sağ taraftaki kutuda ölçüt olarak bir değer veya alan belirtin. 
   Bu örnek için şu dizeyi belirtin: **Microsoft**

   İşte bu koşulların tümü:

   ![Tamamlanan koşul](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

   Koşulunuz için başka bir satır eklemek üzere **Add**  >  **satır ekle**Ekle ' yi seçin. 
   Alt koşullara sahip bir **Grup eklemek için**  >  **Grup Ekle**' yi seçin. 
   Mevcut satırları gruplandırmak için, bu satırların onay kutularını seçin, herhangi bir satır için üç nokta (...) düğmesini seçin ve ardından **Grup Oluştur**' u seçin.

1. **True ise** ve **false ise**, koşulun karşılanıp karşılanmadığını temel alarak gerçekleştirilecek adımları ekleyin. Örneğin:

   !["If true" ve "If false" yollarında koşul](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Var olan eylemleri **true, true** , ise **false** olarak sürükleyebilirsiniz.

1. Mantıksal uygulamanızı kaydedin.

Bu mantıksal uygulama artık yalnızca RSS akışındaki yeni öğeler koşullarınızı karşılıyorsa e-posta gönderir.

## <a name="json-definition"></a>JSON tanımı

Bir koşullu deyimin arkasındaki üst düzey kod tanımı aşağıda verilmiştir:

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

* Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](/answers/topics/azure-logic-apps.html)ziyaret edin.
* Özellikleri ve önerileri göndermek veya Oylamak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Farklı değerlere göre adımları Çalıştır (Switch deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştır ve Yinele adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplanmış eylem durumu (kapsamlar) temelinde adımları Çalıştır](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
