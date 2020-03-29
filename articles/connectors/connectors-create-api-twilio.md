---
title: Azure Logic Apps'tan Twilio'ya bağlanın
description: Azure Mantık Uygulamalarını kullanarak Twilio hesabınız üzerinden küresel SMS, MMS ve IP mesajlarını yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: e5b218efd9c8cfaad99d76d8118d181390a977c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789112"
---
# <a name="manage-messages-in-twilio-with-azure-logic-apps"></a>Azure Mantık Uygulamaları ile Twilio'da iletileri yönetme

Azure Logic Apps ve Twilio konektörü ile Twilio'da küresel SMS, MMS ve IP iletileri içeren iletileri alan, gönderen ve listeleyen otomatik görevler ve iş akışları oluşturabilirsiniz. Bu eylemleri, Twilio hesabınızla görevleri gerçekleştirmek için kullanabilirsiniz. Diğer eylemlerin Twilio eylemlerinden çıktı kullanmasını da sağlayabilirsiniz. Örneğin, yeni bir ileti geldiğinde, ileti içeriğini Slack bağlayıcısıyla gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Tarafından [Twilio](https://www.twilio.com/): 

  * Twilio hesap kimliğiniz ve twilio panonunızda bulabileceğiniz [kimlik doğrulama belirteciniz](https://support.twilio.com/hc/en-us/articles/223136027-Auth-Tokens-and-How-to-Change-Them)

    Kimlik bilgileriniz, bir bağlantı oluşturmak ve mantık uygulamanızdan Twilio hesabınıza erişmek için mantık uygulamanıza yetki vetir. 
    Bir Twilio deneme hesabı kullanıyorsanız, SMS'i yalnızca *doğrulanmış* telefon numaralarına gönderebilirsiniz.

  * SMS gönderebilen doğrulanmış bir Twilio telefon numarası

  * SMS alabilen doğrulanmış bir Twilio telefon numarası

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Twilio hesabınıza erişmek istediğiniz mantık uygulaması. Bir Twilio eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-twilio"></a>Twilio'ya bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

     * Eylem eklemek istediğiniz son adımaltında Yeni **adımı**seçin. 

       -veya-

     * Eylem eklemek istediğiniz adımlar arasında, işaretçinizin üzerine adımların arasında ilerleyin. 
     Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.
     
       Arama kutusuna filtreniz olarak "twilio" girin. 
       Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Bağlantınız için gerekli ayrıntıları sağlayın ve ardından **Oluştur'u**seçin:

   * Bağlantınız için kullanılacak ad
   * Twilio hesap kimliğiniz 
   * Twilio erişim (kimlik doğrulama) belirteciniz

1. Seçtiğiniz eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/twilio/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin