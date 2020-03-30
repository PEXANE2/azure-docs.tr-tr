---
title: Outlook.com'ye bağlanın
description: Azure Mantık Uygulamalarını kullanarak Outlook.com e-postaları, takvimleri ve kişileri yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/18/2016
tags: connectors
ms.openlocfilehash: 8d3b180b6f1e9dc4ec4b09dd81786cc81e8588da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75707195"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-by-using-azure-logic-apps"></a>Azure Mantık Uygulamaları'nı kullanarak Outlook.com e-postaları, takvimleri ve kişileri Outlook.com yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Outlook.com [bağlayıcısı](/connectors/outlook/)ile, mantık uygulamaları oluşturarak hesabınızı @outlook.com veya @hotmail.com hesabınızı yöneten otomatik görevler ve iş akışları oluşturabilirsiniz. Örneğin, bu görevleri otomatikleştirirsiniz:

* E-postayı alın, gönderin ve yanıtlayın.
* Takviminizde toplantı zamanlayın.
* Kişileri ekleyin ve edin.

İş akışınızı başlatmak için, örneğin yeni bir e-posta geldiğinde, takvim öğesi güncelleştirildiğinde veya bir olay farklı bir hizmette gerçekleştiğinde kullanabilirsiniz. Tetikleyici olaya yanıt veren eylemler kullanabilir, örneğin, e-posta gönderebilir veya yeni bir takvim olayı oluşturabilirsiniz.

> [!NOTE]
> Microsoft iş hesabının @fabrikam.onmicrosoft.comgörevlerini otomatikleştirmek için [Office 365 Outlook bağlayıcısını](../connectors/connectors-create-api-office365-outlook.md)kullanın.

## <a name="prerequisites"></a>Ön koşullar

* [Outlook.com hesabı](https://outlook.live.com/owa/)

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Outlook.com hesabınıza erişmek istediğiniz mantık uygulaması. İş akışınızı Outlook.com bir tetikleyiciyle başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)sahip olmanız gerekir. İş akışınıza Outlook.com bir eylem eklemek için mantık uygulamanızın zaten bir tetikleyicisi olması gerekir.

## <a name="add-a-trigger"></a>Tetikleyici ekleme

[Tetikleyici,](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantık uygulamanızdaki iş akışını başlatan bir olaydır. Bu örnek mantık uygulaması, belirtilen aralık ve sıklık temel alınarak e-posta hesabınızdaki yeni e-postaları kontrol eden bir "yoklama" tetikleyicisi kullanır.

1. Azure [portalında,](https://portal.azure.com)Boş mantık uygulamanızı Logic App Designer'da açın.

1. Arama kutusuna filtreniz olarak "outlook.com" girin. Bu örnekte, **yeni bir e-posta geldiğinde**seçin.

1. Oturum açmanız istenirse, mantık uygulamanızın hesabınıza bağlanabilmesi için Outlook.com kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, tetikleyici özellikleri için bilgileri sağlayın:

1. **Tetikleyicide, Sıklık** ve **Aralık** değerlerini ayarlayın.

   Örneğin, tetikleyicinin her 15 dakikada bir yoklaması yapmak istiyorsanız, **Frekans'ı** **Dakikaya**ayarlayın ve **Aralığı** **15**olarak ayarlayın.

1. Tasarımcı araç çubuğunda, mantık uygulamanızı kaydeden **Kaydet'i**seçin.

Tetikleyiciye yanıt vermek için başka bir eylem ekleyin. Örneğin, bir e-posta geldiğinde metin gönderen Twilio **Mesaj Gönder** eylemini ekleyebilirsiniz.

## <a name="add-an-action"></a>Eylem ekleme

[Eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantık uygulamanızdaki iş akışı tarafından yürütülen bir işlemdir. Bu örnek mantık uygulaması Outlook.com hesabınızdan bir e-posta gönderir. Eylemi doldurmak için başka bir tetikleyiciden çıktı kullanabilirsiniz. Örneğin, bir nesne tetikleyici **oluşturulduğunda** mantık uygulamanızın SalesForce'u kullandığını varsayalım. **E-posta** eylemi gönderme Outlook.com ekleyebilir ve e-postadaki SalesForce tetikleyicisinden çıktıları kullanabilirsiniz.

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. İş akışınızdaki son adım olarak bir eylem eklemek için **Yeni adım'ı**seçin. 

   Adımlar arasında bir eylem eklemek için, işaretçinizin bu adımlar arasında okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "outlook.com" girin. Bu örnekiçin, **e-posta gönder'i**seçin. 

1. Oturum açmanız istenirse, mantık uygulamanızın hesabınıza bağlanabilmesi için Outlook.com kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, eylem özellikleri için bilgileri sağlayın.

1. Tasarımcı araç çubuğunda, mantık uygulamanızı kaydeden **Kaydet'i**seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için, bağlayıcının Swagger dosyasında açıklandığı gibi, [bağlayıcının başvuru sayfasına](/connectors/outlook/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
