---
title: Office 365 Outlook 'a bağlanma
description: Office 365 REST API 'Leri ve Azure Logic Apps e-posta, kişi ve takvimleri yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/18/2016
tags: connectors
ms.openlocfilehash: 858366947fe21a20d6f112fc51899d1533a36472
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789590"
---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Office 365 Outlook bağlayıcısını kullanmaya başlama
Office 365 Outlook Bağlayıcısı, Office 365 ' de Outlook ile etkileşime izin verebilir. Kişileri ve takvim öğelerini oluşturmak, düzenlemek ve güncelleştirmek için bu bağlayıcıyı kullanın, ayrıca e-posta alın, gönderin ve yanıtlayın.

Office 365 Outlook ile şunları yapabilirsiniz:

* Office 365 ' deki e-posta ve takvim özelliklerini kullanarak iş akışınızı oluşturun. 
* Yeni bir e-posta olduğunda, bir takvim öğesi güncelleştirilirken ve daha fazlası olduğunda iş akışınızı başlatmak için Tetikleyicileri kullanın.
* Bir e-posta göndermek, yeni bir takvim olayı oluşturmak ve daha fazlasını yapmak için eylemleri kullanın. Örneğin, Salesforce (bir tetikleyici) içinde yeni bir nesne olduğunda, Office 365 Outlook (bir eylem) için bir e-posta gönderin. 

Bu makalede, Office 365 Outlook bağlayıcısının bir mantıksal uygulamada nasıl kullanılacağı gösterilir ve ayrıca Tetikleyiciler ve eylemler listelenir.

> [!NOTE]
> Makalenin bu sürümü, genel kullanıma yönelik Logic Apps için geçerlidir (GA).
> 
> 

Logic Apps hakkında daha fazla bilgi edinmek için bkz. [Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-office-365"></a>Office 365’e bağlanın
Mantıksal uygulamanızın herhangi bir hizmete erişebilmesi için önce hizmete bir *bağlantı* oluşturmanız gerekir. Bir bağlantı, mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, Office 365 Outlook 'a bağlanmak için önce bir Office 365 *bağlantısına*ihtiyacınız vardır. Bağlantı oluşturmak için, bağlanmak istediğiniz hizmete erişmek için normalde kullandığınız kimlik bilgilerini girin. Bu nedenle, Office 365 Outlook ile bağlantıyı oluşturmak için Office 365 hesabınızın kimlik bilgilerini girin.

## <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Tetikleyici kullanma
Tetikleyici, bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılabilen bir olaydır. Hizmeti, istediğiniz zaman aralığında ve sıklıkta "yoklamayı" tetikler. [Tetikleyiciler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Mantıksal uygulamada, tetikleyicilerin bir listesini almak için "Office 365" yazın:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Yakında **gerçekleşecek bir olay yakında başlatıldığında Office 365 Outlook**' u seçin. Zaten bir bağlantı varsa, açılan listeden bir takvim seçin.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Oturum açmanız istenirse, bağlantıyı oluşturmak için oturum açma ayrıntılarını girin. Bu konuda [bağlantı oluşturma](connectors-create-api-office365-outlook.md#create-the-connection) adımları listelenir. 
   
   > [!NOTE]
   > Bu örnekte, mantıksal uygulama bir takvim olayı güncelleştirildikten sonra çalışır. Bu tetikleyicinin sonuçlarını görmek için size bir kısa mesaj Gönderen başka bir eylem ekleyin. Örneğin, takvim olayı 15 dakika içinde başlatıldığında size metin veren Twilio *Send ileti* eylemini ekleyin. 
   > 
   > 
3. **Düzenle** düğmesini seçin ve **Sıklık** ve **Aralık** değerlerini ayarlayın. Örneğin, tetikleyicinin 15 dakikada bir yoklamasını istiyorsanız, **sıklığı** **dakika**olarak ayarlayın ve **aralığı** **15**olarak ayarlayın. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. Değişikliklerinizi **kaydedin** (araç çubuğunun sol üst köşesi). Mantıksal uygulamanız kaydedilir ve otomatik olarak etkinleştirilebilir.

## <a name="use-an-action"></a>Eylem kullanma
Eylem, mantıksal uygulamada tanımlanan iş akışı tarafından yürütülen bir işlemdir. [Eylemler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçenek görürsünüz: **eylem ekleme**, **koşul ekleme**veya **daha fazla** seçenekten biri.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. **Eylem Ekle**' yi seçin.
3. Tüm kullanılabilir eylemlerin listesini almak için metin kutusuna "Office 365" yazın.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. Örneğimizde **Office 365 Outlook-kişi oluştur**' u seçin. Zaten bir bağlantı varsa, **klasör kimliğini**, **verilen adı**ve diğer özellikleri seçin:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Bağlantı bilgileri istenirse, bağlantıyı oluşturmak için ayrıntıları girin. Bu konuda [bağlantı oluşturmak için](connectors-create-api-office365-outlook.md#create-the-connection) bu özellikler açıklanmaktadır. 
   
   > [!NOTE]
   > Bu örnekte, Office 365 Outlook 'ta yeni bir kişi oluşturacağız. İlgili kişiyi oluşturmak için başka bir tetikleyiciden çıkış kullanabilirsiniz. Örneğin, *bir nesne oluşturulduğunda* Salesforce ekleyin. Ardından, yeni kişiyi Office 365 ' de oluşturmak için SalesForce alanlarını kullanan Office 365 Outlook *kişi oluştur* eylemini ekleyin. 
   > 
   > 
5. Değişikliklerinizi **kaydedin** (araç çubuğunun sol üst köşesi). Mantıksal uygulamanız kaydedilir ve otomatik olarak etkinleştirilebilir.

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/office365connector/)tüm limitleri inceleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin