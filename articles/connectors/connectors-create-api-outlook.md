---
title: Outlook.com 'e bağlanma
description: Azure Logic Apps kullanarak e-posta, takvim ve kişileri Outlook.com içinde yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/18/2016
tags: connectors
ms.openlocfilehash: 8d3b180b6f1e9dc4ec4b09dd81786cc81e8588da
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75707195"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak e-posta, takvim ve kişileri Outlook.com içinde yönetin

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Outlook.com Bağlayıcısı](/connectors/outlook/)sayesinde, @outlook.com Logic Apps oluşturarak veya hesabınızı yöneten otomatik görevler ve iş akışları oluşturabilirsiniz @hotmail.com . Örneğin, bu görevleri otomatikleştirin:

* E-posta alın, gönderin ve yanıtlayın.
* Takviminizde toplantılar zamanlayın.
* Kişiler ekleyin ve düzenleyin.

İş akışınızı başlatmak için herhangi bir tetikleyiciyi kullanabilirsiniz. Örneğin, yeni bir e-posta geldiğinde, bir takvim öğesi güncelleştirilirken veya fark hizmetinde bir olay olduğunda. Tetikleyici olayına yanıt veren eylemleri (örneğin, e-posta gönder veya yeni bir takvim olayı oluşturma) kullanabilirsiniz.

> [!NOTE]
> Gibi bir Microsoft iş hesabına yönelik görevleri otomatikleştirmek için @fabrikam.onmicrosoft.com [Office 365 Outlook bağlayıcısını](../connectors/connectors-create-api-office365-outlook.md)kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Bir [Outlook.com hesabı](https://outlook.live.com/owa/)

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Outlook.com hesabınıza erişmek istediğiniz mantıksal uygulama. İş akışınızı bir Outlook.com tetikleyicisi ile başlatmak için [boş bir mantıksal uygulamanız](../logic-apps/quickstart-create-first-logic-app-workflow.md)olması gerekir. İş akışınıza bir Outlook.com eylemi eklemek için mantıksal uygulamanızın zaten bir tetikleyicisi olması gerekir.

## <a name="add-a-trigger"></a>Tetikleyici ekleme

[Tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) , mantıksal uygulamanızda iş akışını başlatan bir olaydır. Bu örnek mantıksal uygulama, belirtilen Aralık ve sıklık temelinde e-posta hesabınızda yeni e-postaları denetleyen "yoklama" tetikleyicisi kullanır.

1. [Azure Portal](https://portal.azure.com), mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Arama kutusuna filtreniz olarak "outlook.com" yazın. Bu örnek için **Yeni bir e-posta geldiğinde**öğesini seçin.

1. Oturum açmanız istenirse, mantıksal uygulamanızın hesabınıza bağlanabilmesi için Outlook.com kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa tetikleyici özellikleri için bilgileri sağlayın:

1. Tetikleyicide **Sıklık** ve **Aralık** değerlerini ayarlayın.

   Örneğin, tetikleyicinin 15 dakikada bir yoklamasını istiyorsanız, **sıklığı** **dakika**olarak ayarlayın ve **aralığı** **15**olarak ayarlayın.

1. Tasarımcı araç çubuğunda, mantıksal uygulamanızı kaydeden **Kaydet**' i seçin.

Tetikleyiciye yanıt vermek için başka bir eylem ekleyin. Örneğin, bir e-posta geldiğinde metin gönderen Twilio **Send ileti** eylemini ekleyebilirsiniz.

## <a name="add-an-action"></a>Eylem ekleme

[Eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) , mantıksal uygulamanızdaki iş akışı tarafından çalıştırılan bir işlemdir. Bu örnek mantıksal uygulama, Outlook.com hesabınızdan bir e-posta gönderir. Eylemi doldurmak için başka bir tetikleyiciden gelen çıktıyı kullanabilirsiniz. Örneğin, **bir nesne oluşturulduğunda** mantıksal uygulamanızın Salesforce kullandığını varsayalım. Outlook.com **e-posta gönder** eylemini ekleyebilir ve e-postadaki Salesforce tetikleyicisinden çıktıları kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. İş akışınızda son adım olarak bir eylem eklemek için **yeni adım**' ı seçin. 

   Adımlar arasında bir eylem eklemek için, işaretçinizi Bu adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak "outlook.com" yazın. Bu örnek için **e-posta gönder**' i seçin. 

1. Oturum açmanız istenirse, mantıksal uygulamanızın hesabınıza bağlanabilmesi için Outlook.com kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, eylem özellikleri için bilgileri sağlayın.

1. Tasarımcı araç çubuğunda, mantıksal uygulamanızı kaydeden **Kaydet**' i seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Swagger dosyasında açıklandığı şekilde Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/outlook/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
