---
title: Azure Logic Apps'tan SendGrid'e bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak SendGrid'de e-posta gönderen görevleri ve iş akışlarını otomatikleştirin ve SendGrid'de posta listelerini yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 998020c5e39c8d50e8a14c74c43b7b435752f43d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789316"
---
# <a name="send-emails-and-manage-mailing-lists-in-sendgrid-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak SendGrid'de e-posta gönderme ve posta listelerini yönetme

Azure Logic Apps ve SendGrid bağlayıcısı ile, e-posta gönderen ve alıcı listelerinizi yöneten otomatik görevler ve iş akışları oluşturabilirsiniz:

* E-posta gönderin.
* Alıcıları listelere ekleyin.
* Küresel bastırmayı alın, ekleyin ve yönetin.

Bu görevleri gerçekleştirmek için mantık uygulamalarınızdaki SendGrid eylemlerini kullanabilirsiniz. SendGrid eylemlerinin çıktısını başka eylemlerin kullanmasını da sağlayabilirsiniz. 

Bu bağlayıcı yalnızca eylemler sağlar, bu nedenle mantık uygulamanızı başlatmak **için, Yineleme** tetikleyicisi gibi ayrı bir tetikleyici kullanın. Örneğin, listelerinize düzenli olarak alıcı eklerseniz, Office 365 Outlook bağlayıcısını veya Outlook.com bağlayıcısını kullanarak alıcılar ve listeler hakkında e-posta gönderebilirsiniz.
Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [SendGrid hesabı](https://www.sendgrid.com/) ve [SendGrid API anahtarı](https://sendgrid.com/docs/ui/account-and-settings/api-keys/)

   API anahtarınız, bir bağlantı oluşturmak ve SendGrid hesabınıza erişmek için mantık uygulamanıza yetki vetir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* SendGrid hesabınıza erişmek istediğiniz mantık uygulaması. SendGrid eylemini kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-sendgrid"></a>SendGrid’e bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Eylem eklemek istediğiniz son adımaltında Yeni **adımı**seçin. 

     -veya-

   * Eylem eklemek istediğiniz adımlar arasında, işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "sendgrid" girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Bağlantınıza bir ad verin. 

1. SendGrid API anahtarınızı girin ve ardından **Oluştur'u**seçin.

1. Seçtiğiniz eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/sendgrid/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin