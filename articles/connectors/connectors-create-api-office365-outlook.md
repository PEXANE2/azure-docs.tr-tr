---
title: Office 365 Outlook'a bağlanma
description: Azure Mantık Uygulamaları'nı kullanarak Office 365 Outlook'ta e-postaları, kişileri ve takvimleri yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/08/2020
tags: connectors
ms.openlocfilehash: b0f2b8b9c369fdb42c7e0e7f77fc090424ae3729
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75732738"
---
# <a name="manage-email-contacts-and-calendars-in-office-365-outlook-by-using-azure-logic-apps"></a>Azure Logic Apps’i kullanarak Office 365 Outlook’ta e-postayı, kişileri ve takvimleri yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Office [365 Outlook bağlayıcısı](/connectors/office365connector/)ile mantık uygulamaları oluşturarak Office 365 hesabınızı yöneten otomatik görevler ve iş akışları oluşturabilirsiniz. Örneğin, bu görevleri otomatikleştirirsiniz:

* E-postayı alın, gönderin ve yanıtlayın. 
* Takviminizde toplantı zamanlayın.
* Kişileri ekleyin ve edin. 

İş akışınızı başlatmak için, örneğin yeni bir e-posta geldiğinde, bir takvim öğesi güncelleştirildiğinde veya Salesforce gibi bir fark hizmetinde bir olay gerçekleştiğinde, herhangi bir tetikleyiciyi kullanabilirsiniz. Tetikleyici olaya yanıt veren eylemler kullanabilir, örneğin, e-posta gönderebilir veya yeni bir takvim olayı oluşturabilirsiniz. 

> [!NOTE]
> Bir @outlook.com veya @hotmail.com hesabın görevlerini otomatikleştirmek için Outlook.com [bağlayıcısını](../connectors/connectors-create-api-outlook.md)kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Bir [Office 365 hesabı](https://www.office.com/)

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Office 365 Outlook hesabınıza erişmek istediğiniz mantık uygulaması. İş akışınızı Bir Office 365 Outlook tetikleyicisiyle başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)sahip olmanız gerekir. İş akışınıza bir Office 365 Outlook eylemi eklemek için mantık uygulamanızın zaten bir tetikleyicisi olması gerekir.

## <a name="add-a-trigger"></a>Tetikleyici ekleme

[Tetikleyici,](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantık uygulamanızdaki iş akışını başlatan bir olaydır. Bu örnek mantık uygulaması, belirtilen aralık ve sıklık temel alınarak e-posta hesabınızdaki güncelleştirilmiş takvim olaylarını denetleyen bir "yoklama" tetikleyicisi kullanır.

1. Azure [portalında,](https://portal.azure.com)Boş mantık uygulamanızı Logic App Designer'da açın.

1. Arama kutusuna filtreniz olarak girin. `office 365 outlook` Bu örnek, **yaklaşan bir olayın yakında ne zaman başlayacağını**seçer.
   
   ![Mantık uygulamanızı başlatmak için tetikleyiciyi seçin](./media/connectors-create-api-office365-outlook/office365-trigger.png)

1. Oturum açmanız istenirse, mantık uygulamanızın hesabınıza bağlanabilmesi için Office 365 kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, tetikleyicinin özellikleri için bilgi sağlayın.

   Bu örnek, tetikleyicinin denetlediği takvimi seçer, örneğin:

   ![Tetikleyicinin özelliklerini yapılandırma](./media/connectors-create-api-office365-outlook/select-calendar.png)

1. **Tetikleyicide, Sıklık** ve **Aralık** değerlerini ayarlayın. **Saat dilimi**gibi kullanılabilir diğer tetikleyici özellikleri eklemek için, bu özellikleri **yeni parametre** ekle listesinden seçin.

   Örneğin, tetikleyicinin takvimi 15 dakikada bir denetlemesini istiyorsanız, **Sıklığı** **Dakikaya**ayarlayın ve **Interval'ı** da 'ya `15`ayarlayın. 

   ![Tetikleyici için frekans ve aralık ayarlama](./media/connectors-create-api-office365-outlook/calendar-settings.png)

1. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Şimdi tetikleyici yangınları sonra çalışan bir eylem ekleyin. Örneğin, bir takvim etkinliği 15 dakika içinde başladığında metin gönderen Twilio **İleti Gönder** eylemini ekleyebilirsiniz.

## <a name="add-an-action"></a>Eylem ekleme

[Eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantık uygulamanızdaki iş akışı tarafından yürütülen bir işlemdir. Bu örnek mantık uygulaması Office 365 Outlook'ta yeni bir kişi oluşturur. İlgili kişi oluşturmak için başka bir tetikleyiciden veya eylemden çıktı kullanabilirsiniz. Örneğin, mantık uygulamanızın Dynamics 365 tetikleyicisini kullandığını varsayalım, **bir kayıt oluşturulduğunda.** Office 365 Outlook Kişi Oluşturma eylemini ekleyebilir ve yeni **bir kişi** oluşturmak için SalesForce tetikleyicisinden çıktıları kullanabilirsiniz.

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. İş akışınızdaki son adım olarak bir eylem eklemek için **Yeni adım'ı**seçin. 

   Adımlar arasında bir eylem eklemek için, işaretçinizin bu adımlar arasında okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak girin. `office 365 outlook` Bu örnek, **kişi oluştur'u**seçer.

   ![Mantık uygulamanızda çalışacak eylemi seçin](./media/connectors-create-api-office365-outlook/office365-actions.png) 

1. Oturum açmanız istenirse, mantık uygulamanızın hesabınıza bağlanabilmesi için Office 365 kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, eylemin özellikleri için bilgi sağlayın.

   Bu örnek, eylemin yeni kişi oluşturduğu kişiler klasörünü seçer, örneğin:

   ![Eylemin özelliklerini yapılandırma](./media/connectors-create-api-office365-outlook/select-contacts-folder.png)

   Kullanılabilir diğer eylem özellikleri eklemek **için, yeni parametre** ekle listesinden bu özellikleri seçin.

1. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Tetikleyiciler, eylemler ve sınırlamalar hakkında bağlayıcının Swagger dosyasında açıklandığı gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/office365connector/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
