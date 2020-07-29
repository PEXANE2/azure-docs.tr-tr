---
title: Office 365 Outlook'a bağlanma
description: Azure Logic Apps kullanarak Office 365 Outlook 'ta e-posta, kişi ve takvimleri yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: article
ms.date: 07/27/2020
tags: connectors
ms.openlocfilehash: aa5be3d58ee1a0a40d0b817e1f5999ccd4bc423d
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319244"
---
# <a name="manage-email-contacts-and-calendars-in-office-365-outlook-by-using-azure-logic-apps"></a>Azure Logic Apps’i kullanarak Office 365 Outlook’ta e-postayı, kişileri ve takvimleri yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Office 365 Outlook Bağlayıcısı](/connectors/office365connector/)sayesinde, Logic Apps oluşturarak Office 365 hesabınızı yöneten otomatik görevler ve iş akışları oluşturabilirsiniz. Örneğin, bu görevleri otomatikleştirin:

* E-posta alın, gönderin ve yanıtlayın. 
* Takviminizde toplantılar zamanlayın.
* Kişiler ekleyin ve düzenleyin. 

İş akışınızı başlatmak için herhangi bir tetikleyiciyi kullanabilirsiniz. Örneğin, yeni bir e-posta geldiğinde, bir takvim öğesi güncelleştirilirken veya fark hizmetinde bir olay gerçekleştiğinde Salesforce gibi. Tetikleyici olayına yanıt veren eylemleri (örneğin, e-posta gönder veya yeni bir takvim olayı oluşturma) kullanabilirsiniz. 

> [!NOTE]
> Bir veya hesabının görevlerini otomatikleştirmek @outlook.com için @hotmail.com [Outlook.com bağlayıcısını](../connectors/connectors-create-api-outlook.md)kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

* [Office 365 hesabı](https://www.office.com/)

* Office 365 Outlook hesabınıza erişmek istediğiniz mantıksal uygulama. İş akışınızı Office 365 Outlook tetikleyicisi ile başlatmak için [boş bir mantıksal uygulamanız](../logic-apps/quickstart-create-first-logic-app-workflow.md)olması gerekir. İş akışınıza Office 365 Outlook eylemi eklemek için mantıksal uygulamanızın zaten bir tetikleyicisi olması gerekir.

## <a name="add-a-trigger"></a>Tetikleyici ekleme

[Tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) , mantıksal uygulamanızda iş akışını başlatan bir olaydır. Bu örnek mantıksal uygulama, belirtilen Aralık ve sıklık temelinde e-posta hesabınızda güncelleştirilmiş herhangi bir takvim olayını denetleyen bir "yoklama" tetikleyicisi kullanır.

1. [Azure Portal](https://portal.azure.com), mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Arama kutusuna `office 365 outlook` filtreniz olarak yazın. Bu örnek **yakında yaklaşan bir olayın ne zaman başlaması**gerektiğini belirler.
   
   ![Mantıksal uygulamanızı başlatmak için tetikleyiciyi seçin](./media/connectors-create-api-office365-outlook/office365-trigger.png)

1. Oturum açmanız istenirse, mantıksal uygulamanızın hesabınıza bağlanabilmesi için Office 365 kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, tetikleyicisinin özelliklerine ilişkin bilgileri belirtin.

   > [!NOTE]
   > Oturum açma kimlik bilgilerinizi değiştirseniz bile, bağlantınızın kullanım süreleri dolmaz. Daha fazla bilgi için [Azure Active Directory 'de yapılandırılabilir belirteç yaşam süreleri](../active-directory/active-directory-configurable-token-lifetimes.md)bölümüne bakın.

   Bu örnek, tetikleyicinin denetlediği takvimi seçer, örneğin:

   ![Tetikleyicinin özelliklerini yapılandırın](./media/connectors-create-api-office365-outlook/select-calendar.png)

1. Tetikleyicide **Sıklık** ve **Aralık** değerlerini ayarlayın. **Saat dilimi**gibi diğer kullanılabilir tetikleyici özelliklerini eklemek için **yeni parametre Ekle** listesinden bu özellikleri seçin.

   Örneğin, tetikleyicinin takvimi 15 dakikada bir denetlemesini istiyorsanız **Sıklık** değerini **dakika**olarak ayarlayın ve **aralığı** olarak ayarlayın `15` . 

   ![Tetikleyici için sıklık ve Aralık ayarlama](./media/connectors-create-api-office365-outlook/calendar-settings.png)

1. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

Şimdi tetikleyici tetiklendiğinde çalışan bir eylem ekleyin. Örneğin, bir takvim olayı 15 dakika içinde başladığında bir metin gönderen Twilio **Ileti gönder** eylemini ekleyebilirsiniz.

## <a name="add-an-action"></a>Eylem ekleme

[Eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) , mantıksal uygulamanızdaki iş akışı tarafından çalıştırılan bir işlemdir. Bu örnek mantıksal uygulama, Office 365 Outlook 'ta yeni bir kişi oluşturur. Kişiyi oluşturmak için başka bir tetikleyiciden veya eylemden çıktıyı kullanabilirsiniz. Örneğin, mantıksal uygulamanızın **bir kayıt oluşturulduğunda**Dynamics 365 tetikleyicisini kullandığını varsayalım. Office 365 Outlook **kişi oluştur** eylemini ekleyebilir ve yeni kişiyi oluşturmak için Salesforce tetikleyicisinden çıktıları kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. İş akışınızda son adım olarak bir eylem eklemek için **yeni adım**' ı seçin. 

   Adımlar arasında bir eylem eklemek için, işaretçinizi Bu adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna `office 365 outlook` filtreniz olarak yazın. Bu örnek, **kişi oluştur**öğesini seçer.

   ![Mantıksal uygulamanızda çalıştırılacak eylemi seçin](./media/connectors-create-api-office365-outlook/office365-actions.png) 

1. Oturum açmanız istenirse, mantıksal uygulamanızın hesabınıza bağlanabilmesi için Office 365 kimlik bilgilerinizi sağlayın. Aksi takdirde, bağlantınız zaten varsa, eylemin özelliklerine ilişkin bilgileri belirtin.

   > [!NOTE]
   > Oturum açma kimlik bilgilerinizi değiştirseniz bile, bağlantınızın kullanım süreleri dolmaz. Daha fazla bilgi için [Azure Active Directory 'de yapılandırılabilir belirteç yaşam süreleri](../active-directory/active-directory-configurable-token-lifetimes.md)bölümüne bakın.

   Bu örnek, eylemin yeni kişiyi oluşturduğu kişiler klasörünü seçer, örneğin:

   ![Eylemin özelliklerini yapılandırın](./media/connectors-create-api-office365-outlook/select-contacts-folder.png)

   Diğer kullanılabilir eylem özelliklerini eklemek için bu özellikleri **yeni parametre Ekle** listesinden seçin.

1. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında, bağlayıcının Swagger dosyasında açıklandığı şekilde Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/office365/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
