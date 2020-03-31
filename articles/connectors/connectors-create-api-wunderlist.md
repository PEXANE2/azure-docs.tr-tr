---
title: Azure Logic Apps'tan Wunderlist'e bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak Wunderlist hesabınızda listeleri, görevleri, anımsatıcıları ve daha fazlasını izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 5ac13595bd77238aaede5fa3bdc3a35ef69e8504
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789129"
---
# <a name="monitor-and-manage-wunderlist-by-using-azure-logic-apps"></a>Azure Mantık Uygulamalarını kullanarak Wunderlist'i izleyin ve yönetin

Azure Logic Apps ve Wunderlist bağlayıcısı ile, wunderlist hesabınızda liste, görev, anımsatıcı ve daha fazlasını izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilir ve örneğin diğer eylemlerle birlikte:

* Yeni görevler oluşturulduğunda, görevlerin zamanı geldiğinde veya anımsatıcılar olduğunda izleyin.
* Listeler, notlar, görevler, alt görevler ve daha fazlasını oluşturun ve yönetin.
* Anımsatıcılar ayarlayın.
* Listeleri, görevleri, alt görevleri, anımsatıcıları, dosyaları, notları, yorumları ve daha fazlasını alın.

[Wunderlist,](https://www.wunderlist.com/) projelerinizi, yapılacaklar listelerinizi ve görevlerinizi herhangi bir cihazda, her yerde planlamanıza, yönetmenize ve bitirmenize yardımcı olan bir hizmettir. Wunderlist hesabınızdan yanıt alan ve çıktıyı diğer eylemleriçin kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. Wunderlist hesabınızla görevleri gerçekleştiren eylemleri kullanabilirsiniz. Wunderlist eylemlerinden çıktıyı başka eylemlerin kullanmasını da sağlayabilirsiniz. Örneğin, yeni görevlerin vadesi geldiğinde, Slack bağlayıcısıyla ileti gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Wunderlist hesabınız ve kullanıcı kimlik bilgileriniz

   Kimlik bilgileriniz, bir bağlantı oluşturmak ve Wunderlist hesabınıza erişmek için mantık uygulamanıza yetki vetir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Yammer hesabınıza erişmek istediğiniz mantık uygulaması. Wunderlist tetikleyicisiyle başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Wunderlist eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-wunderlist"></a>Wunderlist'e bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "wunderlist" girin. 
   Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Mevcut mantık uygulamaları için: 
   
     * Eylem eklemek istediğiniz son adımaltında Yeni **adımı**seçin. 

       -veya-

     * Eylem eklemek istediğiniz adımlar arasında, işaretçinizin üzerine adımların arasında ilerleyin. 
     Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.
     
       Arama kutusuna filtreniz olarak "wunderlist" girin. 
       Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Wunderlist'te oturum açmanız istenirse, erişime izin verebilmek için hemen oturum açın.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/wunderlist/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin