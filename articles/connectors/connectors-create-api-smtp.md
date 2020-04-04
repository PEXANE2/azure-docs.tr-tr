---
title: Azure Logic Apps'tan SMTP'ye bağlanın
description: Azure Logic Apps'ı kullanarak SMTP (Basit Posta Aktarım Protokolü) hesabınız üzerinden e-posta gönderen görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 1cfc53dcd730262101c0e879e0419ba3f2db4d38
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656995"
---
# <a name="send-email-from-your-smtp-account-with-azure-logic-apps"></a>Azure Logic Apps ile SMTP hesabınızdan e-posta gönderme

Azure Logic Apps ve Basit Posta Aktarım Protokolü (SMTP) bağlayıcısı ile SMTP hesabınızdan e-posta gönderen otomatik görevler ve iş akışları oluşturabilirsiniz. SMTP eylemlerinden çıktı kullanan başka eylemler de olabilir. Örneğin, SMTP'niz bir e-posta gönderdikten sonra, Slack konektörüyle Slack'teki ekibinizi bilgilendirebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* SMTP hesabınız ve kullanıcı kimlik bilgileriniz

  Kimlik bilgileriniz, bir bağlantı oluşturmak ve SMTP hesabınıza erişmek için mantık uygulamanıza yetki vetir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* SMTP hesabınıza erişmek istediğiniz mantık uygulaması. Bir SMTP eylemi kullanmak için, bir Salesforce hesabınız varsa, mantık uygulamanızı Salesforce tetikleyicisi gibi bir tetikleyiciyle başlatın.

  Örneğin, mantık uygulamanızı **Salesforce** tetikleyicisi ne zaman bir kayıt oluşturulursa ile başlatabilirsiniz. 
  Bu tetikleyici, Salesforce'ta müşteri adayı gibi yeni bir kaydın oluşturulduğu her seferde devreye sayılsa. 
  Daha sonra SMTP **E-posta gönder** eylemi yle bu tetikleyiciyi takip edebilirsiniz. Bu şekilde, yeni kayıt oluşturulduğunda, mantık uygulamanız SMTP hesabınızdan yeni kayıt hakkında bir e-posta gönderir.

## <a name="connect-to-smtp"></a>SMTP'ye bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir SMTP eylemi eklemek istediğiniz son adımaltında, **Yeni adımı**seçin. 

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "smtp" girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

1. İstendiğinde, şu bağlantı bilgilerini sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Bağlantı Adı** | Evet | SMTP sunucunuza bağlantı için bir ad | 
   | **SMTP Sunucu Adresi** | Evet | SMTP sunucunuzun adresi | 
   | **Kullanıcı Adı** | Evet | SMTP hesabınız için kullanıcı adınız | 
   | **Parola** | Evet | SMTP hesabınız için parolanız | 
   | **SMTP Sunucu Bağlantı Noktası** | Hayır | Kullanmak istediğiniz SMTP sunucunuzda belirli bir bağlantı noktası | 
   | **SSL etkinleştirin?** | Hayır | TLS/SSL şifrelemeyi açın veya kapatın. | 
   |||| 

1. Seçtiğiniz eylem için gerekli ayrıntıları sağlayın. 

1. Mantık uygulamanızı kaydedin veya mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/smtpconnector/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin