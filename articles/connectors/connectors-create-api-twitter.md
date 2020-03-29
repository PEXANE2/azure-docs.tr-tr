---
title: Azure Logic Apps'tan Twitter'a bağlanın
description: Azure Logic Apps'ı kullanarak tweetleri izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin ve ayrıca takipçileriniz, takip eden kullanıcılarınız, diğer kullanıcılar, zaman çizelgeleri ve daha fazlası hakkında twitter hesabınızdan veri alın
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 8ffd0fd558cf759fadd912de9dff4acf49d9659f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789095"
---
# <a name="monitor-and-manage-twitter-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak Twitter'ı izleyin ve yönetin

Azure Logic Apps ve Twitter bağlayıcısı ile, twitter'da önemsediğiniz tweetler, takipçiler, kullanıcılar ve takip edilen kullanıcılar, zaman çizelgeleri ve daha fazlası gibi verileri izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilir ve örneğin diğer eylemlerle birlikte şunları yapabilirsiniz:

* Tweetleri izleyin, gönderin ve arayın.
* Takipçiler, takip edilen kullanıcılar, zaman çizelgeleri ve daha fazlası gibi veriler alın.

Twitter hesabınızdan yanıt alan ve çıktıyı diğer eylemleriçin kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. Twitter hesabınızla görevleri gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca diğer eylemler Twitter eylemleri çıkış kullanmak olabilir. Örneğin, belirli bir hashtag'e sahip yeni bir tweet göründüğünde, Slack bağlayıcısıyla ileti gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Twitter hesabınız ve kullanıcı kimlik bilgileriniz

   Kimlik bilgileriniz, bir bağlantı oluşturmak ve Twitter hesabınıza erişmek için mantık uygulamanıza yetki vetir.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Twitter hesabınıza erişmek istediğiniz mantık uygulaması. Bir Twitter tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bir Twitter eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, örneğin, **Yineleme** tetikleyicisi.

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "twitter" girin. 
   Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Mevcut mantık uygulamaları için: 
   
     * Eylem eklemek istediğiniz son adımaltında Yeni **adımı**seçin. 

       -veya-

     * Eylem eklemek istediğiniz adımlar arasında, işaretçinizin üzerine adımların arasında ilerleyin. 
     Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.
     
       Arama kutusuna filtreniz olarak "twitter" girin. 
       Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Twitter'da oturum açmanız istenirse, mantık uygulamanız için erişim yetkisi verebilmek için hemen oturum açın.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

### <a name="twitter-trigger-when-a-new-tweet-is-posted"></a>Twitter tetikleyicisi: Yeni bir tweet yayınlandığında

Bu tetikleyici, tetikleyici yeni bir tweet algıladığında bir mantık uygulaması iş akışı başlatır, örneğin, hashtag ile, #Seattle. Örneğin, bu tweetler bulunduğunda, Dropbox konektörünü kullanarak Dropbox hesabı gibi tweet'lerin içeriğini içeren bir dosyayı depolama alanına ekleyebilirsiniz. 

İsteğe bağlı olarak, uygun tweet'lerin en az sayıda takipçisi olan kullanıcılardan gelmesi gereken bir koşul ekleyebilirsiniz.

**Kurumsal örnek**: Bu tetikleyiciyi şirketinizle ilgili tweetleri izlemek ve tweetlerin içeriğini bir SQL veritabanına yüklemek için kullanabilirsiniz.

### <a name="twitter-action-post-a-tweet"></a>Twitter eylem: Bir tweet gönder

Bu eylem bir tweet gönderir, ancak tweet'in daha önce açıklanan tetikleyici tarafından bulunan tweetlerin içeriğini içerebileceği şekilde eylemi ayarlayabilirsiniz. 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/twitterconnector/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
