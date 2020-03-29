---
title: Azure Logic Apps'tan Trello'ya bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak Trello projelerinizdeki listeleri, panoları ve kartları izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 5c4fcb9b4fea1a4d982b5cf665564599d371b7cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789146"
---
# <a name="monitor-and-manage-trello-with-azure-logic-apps"></a>Azure Logic Apps ile Trello'u izleyin ve yönetin

Azure Logic Apps ve Trello konektörü ile Trello listelerinizi, kartlarınızı, panolarınızı, ekip üyelerinizi ve benzeri kişileri izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilirsiniz:

* Panolara ve listelere yeni kartlar eklendiğinde izleyin. 
* Panoları, kartları ve listeleri oluşturun, alın ve yönetin.
* Kartlara yorum ve üye ekleyin.
* Liste panoları, pano etiketleri, panodaki kartlar, kart yorumları, kart üyeleri, ekip üyeleri ve üye olduğunuz takımlar. 
* Takımlar getirin.

Trello hesabınızdan yanıt alan ve çıktıyı diğer eylemleriçin kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. Trello hesabınızla görevleri gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca diğer eylemler Trello eylemleri çıktı kullanmak olabilir. Örneğin, karta veya listeye yeni bir kart eklendiğinde, Slack bağlayıcısıyla ileti gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Trello hesabınız ve kullanıcı kimlik bilgileriniz

  Kimlik bilgileriniz, bir bağlantı oluşturmak ve Trello hesabınıza erişmek için mantık uygulamanıza yetki verebilgilidir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Trello hesabınıza erişmek istediğiniz mantık uygulaması. Trello tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Trello eylemini kullanmak için mantık uygulamanızı örneğin **Yineleme** tetikleyicisi gibi bir tetikleyiciyle başlatın.

## <a name="connect-to-trello"></a>Trello'ya bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "trello" girin. Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

   -veya-

   Varolan mantık uygulamaları için, eylem eklemek istediğiniz son adım altında **Yeni adımı**seçin. 
   Arama kutusuna filtreniz olarak "trello" girin. 
   Eylemler listesinin altında, istediğiniz eylemi seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Trello'da oturum açmanız istenirse, mantık uygulamanız için erişim yetkisi ve oturum açın.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/trello/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin