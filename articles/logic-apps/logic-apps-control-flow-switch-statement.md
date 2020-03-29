---
title: İş akışlarına anahtar ifadeleri ekleme
description: Azure Logic Apps'taki belirli değerlere dayalı iş akışı eylemlerini kontrol eden anahtar bildirimleri oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 5c40feec2dca65e4bc9617a71a6d0a8e4c872a3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74793243"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Azure Logic Apps'taki belirli değerlere göre iş akışı eylemleri çalıştıran anahtar bildirimleri oluşturma

Nesnelerin, ifadelerin veya belirteçlerin değerlerini temel alan belirli eylemleri çalıştırmak için bir *anahtar* deyimi ekleyin. Bu yapı nesneyi, ifadeyi veya belirteci değerlendirir, sonuçla eşleşen büyük/küçük harf seçer ve yalnızca bu servis talebi için belirli eylemleri çalıştırRın. Anahtar deyimi çalıştığında, yalnızca bir servis talebi sonucu eşleştirmelidir.

Örneğin, e-postada seçilen bir seçeneğe göre farklı adımlar atlayan bir mantık uygulaması istediğinizi varsayalım. Bu örnekte, mantık uygulaması yeni içerik için bir web sitesinin RSS akışını denetler. RSS akışında yeni bir öğe göründüğünde, mantık uygulaması onaylayana e-posta gönderir. Onaylayanın "Onayla" veya "Reddet" seçeneğini seçip seçmediğine bağlı olarak, mantık uygulaması farklı adımları izler.

> [!TIP]
> Tüm programlama dilleri gibi, geçiş deyimleri yalnızca eşitlik işleçlerini destekler. "Büyük"ten daha büyük gibi başka ilişkisel işleçler gerekiyorsa, [koşullu](../logic-apps/logic-apps-control-flow-conditional-statement.md)bir deyim kullanın.
> Deterministik yürütme davranışı sağlamak için, servis talepleri dinamik belirteçleri veya ifadeler yerine benzersiz ve statik bir değer içermelidir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bu makaledeki örneği izlemek için, Outlook.com veya Office 365 Outlook hesabı içeren [bu örnek mantık uygulamasını oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

  1. E-posta göndermek için eylemi eklediğinizde, bunun yerine bu eylemi bulun ve seçin: **Onay e-postası gönderin**

     !["Onay e-postası gönder" seçeneğini belirleyin](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  1. Onay e-postasını alan kişi için e-posta adresi gibi gerekli alanları sağlayın. 
  **Kullanıcı Seçenekleri**altında "Onayla, Reddet" girin.

     ![E-posta ayrıntılarını girin](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-switch-statement"></a>Anahtar ekstresi ekleme

1. Bu örnek için, örnek iş akışınızın sonuna bir anahtar bildirimi ekleyin. Son adımdan sonra **Yeni adımı**seçin.

   Adımlar arasına bir geçiş deyimi eklemek istediğinizde, işaretçiyi anahtar deyimini eklemek istediğiniz okun üzerine taşıyın. Görünen **artı işaretini** (**+**) seçin, ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "switch" girin. Bu eylemi seçin: **Geçiş - Denetim**

   ![Anahtar ekle](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Bir anahtar deyimi, bir büyük/küçük harf ve varsayılan bir servis talebiyle görüntülenir. 
   Varsayılan olarak, bir anahtar deyimi en az bir büyük/küçük harf artı varsayılan durum gerektirir. 

   ![Boş varsayılan anahtar deyimi](./media/logic-apps-control-flow-switch-statement/empty-switch.png)

1. Dinamik içerik listesinin görünmesi için **A'daki** kutusunun içini tıklatın. Bu listeden, çıktısı gerçekleştirilecek eylemi belirleyen **SelectedOption** alanını seçin. 

   !["SelectedOption" seçeneğini belirleyin](./media/logic-apps-control-flow-switch-statement/select-selected-option.png)

1. Onaylayanın seçtiği `Approve` servis taleplerini işlemek `Reject`için veya **Büyük/Küçük Harf** ile **Varsayılan**arasına başka bir servis talebi ekleyin. 

   ![Başka bir servis talebi ekleme](./media/logic-apps-control-flow-switch-statement/switch-plus.png)

1. Bu eylemleri ilgili servis taleplerine ekleyin:

   | Durumda # | **SelectedOption** | Eylem |
   |--------|--------------------|--------|
   | Örnek Olay 1 | **Onaylama** | Outlook'u ekle RSS öğesi hakkında ayrıntılı bilgi göndermek için **Approve**yalnızca onaylayan Onayla'yı seçtiğinde **bir e-posta** eylemi gönderin. |
   | Büyük/Küçük Harf 2 | **Reddet** | Outlook RSS öğesinin reddedildiğini diğer onaylayanlara bildirmek için **bir e-posta** eylemi gönder'i ekleyin. |
   | Varsayılan | None | Herhangi bir işlem gerekli değil. Bu örnekte, **SelectedOption** yalnızca iki seçeneği olduğundan **Varsayılan** servis talebi boştur. |
   |||

   ![Bitmiş anahtar deyimi](./media/logic-apps-control-flow-switch-statement/finished-switch.png)

1. Mantıksal uygulamanızı kaydedin. 

   Bu örneği el ile sınamak için, mantık uygulaması yeni bir RSS öğesi bulana ve onay e-postası gönderene kadar **Çalıştır'ı** seçin. 
   Sonuçları gözlemlemek için **Onayla'yı** seçin.

## <a name="json-definition"></a>JSON tanımı

Artık bir anahtar deyimi kullanarak bir mantık uygulaması oluşturduğunuza göre, anahtar deyiminin arkasındaki üst düzey kod tanımına bakalım.

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
| `"Switch"`         | Okunabilirlik için yeniden adlandırabileceğiniz anahtar deyiminin adı |
| `"type": "Switch"` | Eylemin bir geçiş deyimi olduğunu belirtir |
| `"expression"`     | Bu örnekte, her davaya karşı değerlendirilen onaylayanın seçili seçeneğini, tanımda daha sonra beyan edildiği şekilde belirtir |
| `"cases"` | Herhangi bir sayıda servis etme vakası tanımlar. Her servis `"Case_*"` talebi için, okunabilirlik için yeniden adlandırabileceğiniz bu servis talebinin varsayılan adıdır |
| `"case"` | Anahtar deyiminin karşılaştırma için kullandığı sabit ve benzersiz bir değer olması gereken servis talebinin değerini belirtir. Geçiş ifadesi sonucuyla eşleşmezse, `"default"` bölümdeki eylemler çalıştırılır. | 
| | | 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özellikler veya öneriler göndermek veya oy kullanmak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Koşula dayalı adımları çalıştırma (koşullu ifadeler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Adımları çalıştırma ve yineleme (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplanmış eylem durumuna (kapsamlara) göre adımları çalıştırma](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
