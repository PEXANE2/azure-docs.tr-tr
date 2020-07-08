---
title: Azure Logic Apps Trello 'a bağlanma
description: Azure Logic Apps kullanarak Trello projelerinizde listeleri, panoları ve kartları izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: d1426bbe2064067b26f53a8f8e5d4aa842ede8c0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83829707"
---
# <a name="monitor-and-manage-trello-with-azure-logic-apps"></a>Azure Logic Apps Trello 'yi izleme ve yönetme

Azure Logic Apps ve Trello Bağlayıcısı sayesinde Trello listelerinizi, kartlarınızı, panoları, ekip üyelerini ve benzeri işlemleri izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilirsiniz, örneğin:

* Panolar ve listelere yeni kartlar eklendiğinde izleyin. 
* Panolar, kartlar ve listeler oluşturun, alın ve yönetin.
* Kartlara yorum ve üye ekleyin.
* Bir üye olduğunuz yerde panoları, pano etiketlerini, panoları, kart açıklamalarını, kart üyelerini, ekip üyelerini ve ekiplerini listeleyin. 
* Takımları al.

Trello hesabınızdan yanıt alan Tetikleyicileri kullanabilir ve çıktıyı diğer eylemler için kullanılabilir hale getirebilirsiniz. Trello hesabınızla görev gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca, Trello eylemlerdeki çıktıyı kullanan başka eylemlere de sahip olabilirsiniz. Örneğin, pano veya listeye yeni bir kart eklendiğinde, bolluk bağlayıcısıyla iletiler gönderebilirsiniz. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Trello hesabınız ve Kullanıcı kimlik bilgileriniz

  Kimlik bilgileriniz, mantıksal uygulamanızı bir bağlantı oluşturmak ve Trello hesabınıza erişmek için yetkilendirirsiniz.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Trello hesabınıza erişmek istediğiniz mantıksal uygulama. Trello tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Trello eylemini kullanmak için, mantıksal uygulamanızı bir tetikleyici ile başlatın, örneğin **yineleme** tetikleyicisi.

## <a name="connect-to-trello"></a>Trello 'a bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Boş Logic Apps için, arama kutusuna filtreniz olarak "Trello" yazın. Tetikleyiciler listesinde istediğiniz tetikleyiciyi seçin. 

   -veya-

   Mevcut Logic Apps için, eylem eklemek istediğiniz son adım altında **yeni adım**' ı seçin. 
   Arama kutusuna filtreniz olarak "Trello" yazın. 
   Eylemler listesi altında istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. 
   Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Trello 'da oturum açmanız istenirse, mantıksal uygulamanız için erişimi yetkilendirin ve oturum açın.

1. Seçtiğiniz tetikleyici veya eyleminiz için gerekli ayrıntıları sağlayın ve mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/trello/)gözden geçirin.

## <a name="get-support"></a>Destek alma

* Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-logic-apps.html)ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin