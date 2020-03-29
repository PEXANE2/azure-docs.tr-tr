---
title: Azure Mantık Uygulamalarından Bolluğu'na bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak Slack hesabınızdaki dosyaları ve kanalları, grupları ve iletileri izleyen görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 5f61009ee7b43be618e37acb4a783a54dbf11e55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789180"
---
# <a name="monitor-and-manage-slack-with-azure-logic-apps"></a>Azure Logic Apps ile Slack'i izleyin ve yönetin

Azure Logic Apps ve Slack konektörü ile, Slack dosyalarınızı izleyen ve Örneğin Slack kanallarınızı, iletilerinizi, gruplarınızı ve benzeri kişileri yöneten otomatik görevler ve iş akışları oluşturabilirsiniz:

* Yeni dosyalar oluşturulduğunda izleyin.
* Kanallar oluşturma, listele ve birleştirme 
* Mesaj gönderin.
* Gruplar oluşturun ve ayarlayın rahatsız etmeyin.

Slack hesabınızdan yanıt alan ve çıktıyı diğer eylemler için kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. Slack hesabınızla görevleri gerçekleştiren eylemleri kullanabilirsiniz. Diğer eylemlerin Slack eylemlerinden çıktı kullanmasını da sağlayabilirsiniz. Örneğin, yeni bir dosya oluşturulduğunda, Office 365 Outlook bağlayıcısı ile e-posta gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Slack](https://slack.com/) hesabınız ve kullanıcı kimlik bilgileriniz

  Kimlik bilgileriniz, bir bağlantı oluşturmak ve Slack hesabınıza erişmek için mantık uygulamanıza yetki vetir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Slack hesabınıza erişmek istediğiniz mantık uygulaması. Bir Slack tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bir Slack eylemi kullanmak için, mantık uygulamanızı Slack tetikleyicisi veya **Yineleme** tetikleyicisi gibi başka bir tetikleyici yle başlatın.

## <a name="connect-to-slack"></a>Bolluk'a bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "bolluk" girin. Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

   -veya-

   Varolan mantık uygulamaları için, eylem eklemek istediğiniz son adım altında **Yeni adımı**seçin. 
   Arama kutusuna filtreniz olarak "bolluk" girin. 
   Eylemler listesinin altında, istediğiniz eylemi seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Slack'te oturum açmanız istenirse, Slack çalışma alanınızda oturum açın. 

   ![Slack çalışma alanında oturum açın](./media/connectors-create-api-slack/slack-sign-in-workspace.png)

1. Mantık uygulamanız için erişim yetkisi.

   ![Slack'e erişimi yetkilendirme](./media/connectors-create-api-slack/slack-authorize-access.png)

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın. Mantık uygulamanızın iş akışını oluşturmaya devam etmek için daha fazla eylem ekleyin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/slack/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
