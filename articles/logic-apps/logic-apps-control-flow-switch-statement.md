---
title: İş akışlarına Switch deyimleri ekleme
description: Azure Logic Apps içindeki belirli değerlere göre iş akışı eylemlerini denetleyen Switch deyimleri oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 5c40feec2dca65e4bc9617a71a6d0a8e4c872a3a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74793243"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Azure Logic Apps içindeki belirli değerlere göre iş akışı eylemlerini çalıştıran Switch deyimleri oluşturma

Nesnelerin, ifadelerin veya belirteçlerin değerlerine göre belirli eylemleri çalıştırmak için bir *Switch* deyimi ekleyin. Bu yapı, nesne, ifade veya belirteci değerlendirir, sonuçla eşleşen durumu seçer ve yalnızca o durumda belirli eylemleri çalıştırır. Switch deyimleri çalıştırıldığında, yalnızca bir örnek sonuçla eşleşmelidir.

Örneğin, e-postada seçilen bir seçeneğe göre farklı adımlar alan bir mantıksal uygulama istediğinizi varsayalım. Bu örnekte, mantıksal uygulama yeni içerik için bir Web sitesinin RSS akışını denetler. RSS akışında yeni bir öğe göründüğünde, mantıksal uygulama bir onaylayana e-posta gönderir. Onaylayanın "Onayla" veya "Reddet" seçeneğini seçip seçmeksizin mantıksal uygulamanın farklı adımları takip etmemesine göre.

> [!TIP]
> Tüm programlama dilleri gibi, Switch deyimleri yalnızca eşitlik işleçlerini destekler. "Büyüktür" gibi diğer ilişkisel işleçlere ihtiyacınız varsa, [koşullu bir ifade](../logic-apps/logic-apps-control-flow-conditional-statement.md)kullanın.
> Belirleyici yürütme davranışının güvence altına almak için, durumların dinamik belirteçler veya ifadeler yerine benzersiz ve statik bir değer içermesi gerekir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bu makaledeki örneği izlemek için, Outlook.com veya Office 365 Outlook hesabıyla [Bu örnek mantıksal uygulamayı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md) .

  1. E-posta gönder eylemini eklediğinizde bunun yerine bu eylemi bulun ve seçin: **onay e-postası gönder**

     !["Onay e-postası gönder" i seçin](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  1. Onay e-postasını alan kişinin e-posta adresi gibi gerekli alanları sağlayın. 
  **Kullanıcı seçenekleri**altında "Onayla, Reddet" yazın.

     ![E-posta ayrıntılarını girin](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-switch-statement"></a>Switch açıklaması ekle

1. Bu örnek için, örnek iş akışınızı sonuna bir switch açıklaması ekleyin. Son adımdan sonra **yeni adım**' ı seçin.

   Adımlar arasında bir switch açıklaması eklemek istediğinizde, işaretçiyi switch ifadesini eklemek istediğiniz okun üzerine taşıyın. Görüntülenen **artı işaretini** (**+**) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak "Switch" yazın. Şu eylemi seçin: **Switch-Control**

   ![Anahtar Ekle](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Bir switch ifadesi, bir Case ve bir varsayılan durum ile görüntülenir. 
   Varsayılan olarak, switch ifadesinin en az bir Case ve varsayılan Case olması gerekir. 

   ![Boş varsayılan anahtar ekstresi](./media/logic-apps-control-flow-switch-statement/empty-switch.png)

1. Dinamik içerik listesinin görünmesi için **on** kutusunun içine tıklayın. Bu listeden, çıktısı gerçekleştirilecek eylemi belirleyen **SelectedOption** alanını seçin. 

   !["SelectedOption" öğesini seçin](./media/logic-apps-control-flow-switch-statement/select-selected-option.png)

1. `Approve` Onaylayanın veya `Reject`seçtiği durumları Işlemek Için, **büyük/küçük harf** ve **varsayılan**arasında başka bir durum ekleyin. 

   ![Başka bir durum ekleyin](./media/logic-apps-control-flow-switch-statement/switch-plus.png)

1. Bu eylemleri ilgili servis taleplerine ekleyin:

   | Harflerini # | **SelectedOption** | Eylem |
   |--------|--------------------|--------|
   | Durum 1 | **Onaylama** | Outlook 'U, RSS öğesiyle ilgili ayrıntıları göndermek için yalnızca **onaylayan seçili olduğunda** **bir e-posta gönder** eylemi ekleyin. |
   | Durum 2 | **Reddet** | Outlook 'un RSS öğesinin reddedildiğini diğer onaylayanlara bildirmek için **e-posta gönder** eylemini ekleyin. |
   | Varsayılan | Hiçbiri | Herhangi bir işlem gerekli değil. Bu örnekte, **SelectedOption** yalnızca iki seçeneğe sahip olduğundan, **varsayılan** durum boştur. |
   |||

   ![Tamamlanan Switch ekstresi](./media/logic-apps-control-flow-switch-statement/finished-switch.png)

1. Mantıksal uygulamanızı kaydedin. 

   Bu örneği el ile test etmek için Logic App yeni bir RSS öğesi bulana ve bir onay e-postası gönderene kadar **Çalıştır** ' ı seçin. 
   Sonuçları gözlemlemek için **Onayla** ' yı seçin.

## <a name="json-definition"></a>JSON tanımı

Artık bir switch ifadesini kullanarak bir mantıksal uygulama oluşturduğunuza göre, switch ifadesinin arkasındaki üst düzey kod tanımına bakalım.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {}
         },
         "case" : "Approve"
      },
      "Case_2": {
         "actions": {
           "Send_an_email_2": {}
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

| Etiketle | Açıklama |
|-------|-------------|
| `"Switch"`         | Okunabilirlik için yeniden adlandırabileceği switch ifadesinin adı |
| `"type": "Switch"` | Eylemin bir switch ifadesinin olduğunu belirtir |
| `"expression"`     | Bu örnekte, daha sonra tanımda bildirildiği gibi her bir durumda değerlendirilen onaylayanın seçili seçeneğini belirtir |
| `"cases"` | Herhangi bir sayıda durumu tanımlar. Her durumda, `"Case_*"` bu durumda okunabilirlik için yeniden adlandırılabileceği varsayılan addır. |
| `"case"` | Anahtar ifadesinin karşılaştırma için kullandığı sabit ve benzersiz bir değer olması gereken Case değerini belirtir. Anahtar ifadesi sonucuyla eşleşen hiçbir durum yoksa, `"default"` bölümündeki eylemler çalıştırılır. | 
| | | 

## <a name="get-support"></a>Destek alma

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özellikleri veya önerileri göndermek veya Oylamak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre adımları çalıştırın (koşullu deyimler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Çalıştır ve Yinele adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplanmış eylem durumu (kapsamlar) temelinde adımları Çalıştır](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
