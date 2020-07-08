---
title: Azure Logic Apps Twitter 'a bağlanma
description: Azure Logic Apps kullanarak Twitter hesabınızdan izleme ve yönetme, takip eden kullanıcılar, diğer kullanıcılar, zaman çizelgelerinizi ve daha fazlasını içeren görevleri ve iş akışlarını otomatikleştirin.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: f2db6d614c3c12cb1be87724e79d79a16769d6b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83829605"
---
# <a name="monitor-and-manage-twitter-by-using-azure-logic-apps"></a>Azure Logic Apps'i kullanarak Twitter'ı izleme ve yönetme

Azure Logic Apps ve Twitter Bağlayıcısı sayesinde, sözgelimi Twitter, takiciler, kullanıcılar ve takip eden kullanıcılar, zaman çizelgeleri ve daha fazlası gibi diğer eylemlerle birlikte istediğiniz verileri izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilirsiniz, örneğin:

* Fazla doldurulabilir, nakledin ve arama yapın.
* İzleme, takip eden kullanıcılar, zaman çizelgeleri ve daha fazlası gibi verileri alın.

Twitter hesabınızdan yanıt alan Tetikleyicileri kullanabilir ve çıktıyı diğer eylemler için kullanılabilir hale getirebilirsiniz. Twitter hesabınızla görev gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca, Twitter eylemlerinden alınan çıktıyı başka eylemlere de kullanabilirsiniz. Örneğin, belirli bir diyez etiketi ile yeni bir tweet göründüğünde, bolluk bağlayıcısıyla iletiler gönderebilirsiniz. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Twitter hesabınız ve Kullanıcı kimlik bilgileriniz

   Kimlik bilgileriniz, bir bağlantı oluşturmak ve Twitter hesabınıza erişmek için mantıksal uygulamanızı yetkilendirirsiniz.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Twitter hesabınıza erişmek istediğiniz mantıksal uygulama. Twitter tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Twitter eylemini kullanmak için, mantıksal uygulamanızı başka bir tetikleyici ile başlatın, örneğin **yineleme** tetikleyicisi.

## <a name="connect-to-twitter"></a>Twitter’a Bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Bir yol seçin: 

   * Boş Logic Apps için, arama kutusuna filtreniz olarak "Twitter" yazın. 
   Tetikleyiciler listesinde istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Mevcut mantıksal uygulamalar için: 
   
     * Eylem eklemek istediğiniz son adım altında **yeni adım**' ı seçin. 

       -veya-

     * Eylem eklemek istediğiniz adımlar arasında, işaretçinizi adımlar arasındaki oka taşıyın. 
     Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.
     
       Arama kutusuna filtreniz olarak "Twitter" yazın. 
       Eylemler listesi altında istediğiniz eylemi seçin.

1. Twitter 'da oturum açmanız istenirse, mantıksal uygulamanıza erişimi yetkilendirebilmeniz için şimdi oturum açın.

1. Seçtiğiniz tetikleyici veya eyleminiz için gerekli ayrıntıları sağlayın ve mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

### <a name="twitter-trigger-when-a-new-tweet-is-posted"></a>Twitter tetikleyicisi: yeni bir tweet gönderildiğinde

Bu tetikleyici yeni bir tweet algıladığında, örneğin diyez etiketiyle, #Seattle bir mantıksal uygulama iş akışı başlatır. Örneğin, bu ara bellek bulunduğunda Dropbox bağlayıcısını kullanarak bir Dropbox hesabı gibi depolama alanına sahip bir dosya ekleyebilirsiniz. 

İsteğe bağlı olarak, en az belirtilen takipçilerin bulunduğu kullanıcılardan, uygun olan çok sayıda sahip olan bir koşul ekleyebilirsiniz.

**Kurumsal örnek**: Bu tetikleyiciyi, şirketiniz hakkındaki bilgileri izlemek ve bu IÇERIĞI bir SQL veritabanına yüklemek için kullanabilirsiniz.

### <a name="twitter-action-post-a-tweet"></a>Twitter eylemi: bir tweet gönderin

Bu eylem bir tweet nakleder, ancak bu eylemi, Tweet 'in daha önce açıklanan tetikleyici tarafından bulunan çok fazla yer alan alanı içeriğini içermesi için ayarlayabilirsiniz. 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/twitterconnector/)gözden geçirin.

## <a name="get-support"></a>Destek alma

* Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-logic-apps.html)ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
