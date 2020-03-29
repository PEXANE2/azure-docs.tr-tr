---
title: Azure Mantık Uygulamalarından Yammer'a bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak Yammer'da iletileri, akışları ve daha fazlasını izleyen, gönderen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 194c08d034d44ba0a4472b3b516fc45d1d262d28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789078"
---
# <a name="monitor-and-manage-your-yammer-account-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak Yammer hesabınızı izleyin ve yönetin

Azure Logic Apps ve Yammer bağlayıcısı ile, örneğin diğer eylemlerle birlikte Yammer hesabınızda iletileri, akışları ve daha fazlasını izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilirsiniz:

* İzlenen akışlarda ve gruplarda yeni iletilerin ne zaman görünününü izleyin.
* İletileri, grupları, ağları, kullanıcı ayrıntılarını ve daha fazlasını alın.
* Mesaj gönderin ve mesajları beğenin.

Yammer hesabınızdan yanıt alan ve çıktıyı diğer eylemleriçin kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. Yammer hesabınızla görevleri gerçekleştiren eylemleri kullanabilirsiniz. Diğer eylemlerin Yammer eylemlerinden çıktı kullanmasını da sağlayabilirsiniz. Örneğin, akışlarda veya gruplarda yeni iletiler göründüğünde, bu iletileri Slack bağlayıcısıyla paylaşabilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Yammer hesabınız ve kullanıcı kimlik bilgileriniz

   Kimlik bilgileriniz, bir bağlantı oluşturmak ve Yammer hesabınıza erişmek için mantık uygulamanıza yetki vetir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Yammer hesabınıza erişmek istediğiniz mantık uygulaması. Yammer tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bir Yammer eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-yammer"></a>Yammer'a bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "yammer" girin. 
   Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Mevcut mantık uygulamaları için: 
   
     * Eylem eklemek istediğiniz son adımaltında Yeni **adımı**seçin. 

       -veya-

     * Eylem eklemek istediğiniz adımlar arasında, işaretçinizin üzerine adımların arasında ilerleyin. 
     Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.
     
       Arama kutusuna filtreniz olarak "yammer" girin. 
       Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Yammer'da oturum açmanız istenirse, erişime izin verebilmek için hemen oturum açın.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/yammer/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin