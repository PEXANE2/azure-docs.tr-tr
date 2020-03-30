---
title: Müşteri öngörüleri panosu oluşturma
description: Azure Mantık Uygulamaları ve Azure Fonksiyonları ile müşteri panosu oluşturarak müşteri geri bildirimlerini, sosyal medya verilerini ve daha fazlasını yönetin
services: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 03/15/2018
ms.openlocfilehash: e300bf9c9aa0acf0bed6426eb73f690f9a38bd74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75980433"
---
# <a name="create-a-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Azure Logic Apps ve Azure Fonksiyonları ile akışlı müşteri öngörüleri panosu oluşturun

Azure, altyapıyı düşünmek zorunda kalmadan bulutta uygulamaları hızla oluşturmanıza ve barındırmanıza yardımcı olan [sunucusuz](https://azure.microsoft.com/solutions/serverless/) araçlar sunar. Bu eğitimde, müşteri geri bildirimlerini tetikleyen, makine öğrenimiyle geri bildirimleri analiz eden ve Power BI veya Azure Veri Gölü gibi bir kaynağa öngörüler yayınlayan bir pano oluşturabilirsiniz.

Bu çözüm için, sunucusuz uygulamalar için bu önemli Azure bileşenlerini kullanırsınız: [Azure İşlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Mantık Uygulamaları.](https://azure.microsoft.com/services/logic-apps/)
Azure Logic Apps, sunucusuz bileşenler arasında orkestrasyon oluşturabilmeniz ve 200'den fazla hizmet ve API'ye bağlanabilmeniz için bulutta sunucusuz bir iş akışı altyapısı sağlar. Azure İşlevler bulutta sunucusuz bilgi işlem sağlar. Bu çözüm, önceden tanımlanmış anahtar kelimelere dayalı müşteri tweetlerini işaretlemek için Azure Işlevlerini kullanır.

Bu senaryoda, müşterilerden geri bildirim bulmayı tetikleyen bir mantık uygulaması oluşturursunuz. Müşteri geri bildirimlerine yanıt vermenize yardımcı olan bazı bağlayıcılar arasında Outlook.com, Office 365, Survey Monkey, Twitter ve [bir web formundan bir HTTP isteği](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/)yer almaktadır. Oluşturduğunuz iş akışı Twitter'da bir hashtag izler.

Tüm [çözümü Visual Studio'da oluşturabilir](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) ve [Azure Kaynak Yöneticisi şablonu yla çözümü dağıtabilirsiniz.](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md) Bu çözümün nasıl oluşturulabildiğini gösteren bir video izlemesi için [bu Kanal 9 videosunu izleyin.](https://aka.ms/logicappsdemo) 

## <a name="trigger-on-customer-data"></a>Müşteri verilerini tetikleme

1. Azure portalında veya Visual Studio'da boş bir mantık uygulaması oluşturun. 

   Mantık uygulamalarında yeniyseniz, Azure [portalı nın hızlı başlangıcını](../logic-apps/quickstart-create-first-logic-app-workflow.md) veya [Visual Studio için hızlı başlangıcı](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)gözden geçirin.

2. Mantık App Designer, bulmak ve bu eylem var Twitter tetikleyici ekleyin: **Yeni bir tweet yayınlandığında**

3. Bir anahtar kelimeye veya hashtag'e dayalı tweetleri dinlemek için tetikleyiciyi ayarlayın.

   Twitter tetikleyicisi gibi yoklama tabanlı tetikleyicilerde, yineleme özelliği mantık uygulamasının yeni öğeleri ne sıklıkta denetlediğini belirler.

   ![Twitter tetikleyici örneği][1]

Bu mantık uygulaması şimdi tüm yeni tweets ateşler. Daha sonra tweet verilerini alabilir ve analiz edebilirsiniz, böylece ifade edilen duyguları daha iyi anlayabilirsiniz. 

## <a name="analyze-tweet-text"></a>Tweet metnini analiz edin

Bazı metinlerin arkasındaki duyarlılığı algılamak için [Azure Bilişsel Hizmetleri'ni](https://azure.microsoft.com/services/cognitive-services/)kullanabilirsiniz.

1. Mantık App Designer, tetiği altında, **Yeni adım**seçin.

2. Text **Analytics** konektörünü bulun.

3. **Algıla Eylem** eylemini seçin.

4. İstenirse, Metin Analizi hizmeti için geçerli bir Bilişsel Hizmetler anahtarı sağlayın.

5. **İstek Gövdesi**altında, tweet metnini analiz için giriş olarak sağlayan **Tweet Metni** alanını seçin.

Tweet verilerini ve tweet hakkındaki istatistikleri aldıktan sonra, artık diğer birkaç ilgili bağlayıcıyı ve bunların eylemlerini kullanabilirsiniz:

* **Power BI - Streaming Dataset'e Satır Ekleme**: Gelen tweet'leri Power BI panosunda görüntüleyin.
* **Azure Veri Gölü - Ek dosyası**: Analitik işlere dahil etmek için müşteri verilerini Azure Veri Gölü veri kümesine ekleyin.
* **SQL - Satır ekle**: Verileri daha sonra almak için bir veritabanında saklayın.
* **Bolluk - İleti gönder**: Bir Slack kanalına eylem gerektirebilecek olumsuz geri bildirimhakkında bilgi verin.

Ayrıca, verilerinizde özel işleme gerçekleştirebilmeniz için bir Azure İşlevi oluşturabilir ve bir Azure İşlevi oluşturabilirsiniz. 

## <a name="process-data-with-azure-functions"></a>Azure İşlevleriyle verileri işleme

Bir işlev oluşturmadan önce Azure aboneliğinizde bir işlev uygulaması oluşturun. Ayrıca, mantık uygulamanızın doğrudan bir işlev çağırabilmesi için, işlevin bir HTTP tetikleyici bağlaması olması gerekir, örneğin, **HttpTrigger** şablonunu kullanın. [Azure portalında ilk işlev uygulamanızı ve işlevinizi nasıl oluşturup oluşturup oluşturmayı](../azure-functions/functions-create-first-azure-function-azure-portal.md)öğrenin.

Bu senaryo için, Tweet metnini Azure İşlevinizin istek gövdesi olarak kullanın. İşlev kodunuzda, tweet metninin bir anahtar kelime veya tümcecik içerip içermediğini belirleyen mantığı tanımlayın. İşlev, senaryo için gerektiği kadar basit veya karmaşık tutun.
İşlevin sonunda, mantık uygulamasına bazı verilerle birlikte bir yanıt döndürün, `containsKeyword` örneğin basit bir boolean değeri veya karmaşık bir nesne gibi.

> [!TIP]
> Bir mantık uygulamasındaki bir işlevden karmaşık bir yanıta erişmek için **Parse JSON eylemini** kullanın.

İşi bittiğinde, işlevi kaydedin ve ardından oluşturmakta olduğunuz mantık uygulamasına işlevi eylem olarak ekleyin.

## <a name="add-azure-function-to-logic-app"></a>Mantık uygulamasına Azure işlevi ekleme

1. Mantık App Designer, **Algı Sentiment** eylem altında, **Yeni adım**seçin.

2. Azure **İşlevler** bağlayıcısını bulun ve ardından oluşturduğunuz işlevi seçin.

3. **İstek Gövdesi**altında Tweet **Metni'ni**seçin.

![Yapılandırılmış Azure İşlevi adımı][2]

## <a name="run-and-monitor-your-logic-app"></a>Mantık uygulamanızı çalıştırın ve izleyin

Mantık uygulamanız için geçerli veya önceki çalıştırmaları gözden geçirmek için, Azure Logic Apps'ın Azure portalında, Visual Studio'da veya Azure REST API'lerinde ve SDK'larında sağladığı zengin hata ayıklama ve izleme özelliklerini kullanabilirsiniz.

Mantık uygulamanızı kolayca test etmek için Logic App Designer'da **Tetikle'yi Çalıştır'ı**seçin. Ölçütlerinizi karşılayan bir tweet bulunana kadar, belirtilen zamanlamanıza göre tweet'lerin tetikleyici anketleri bulunur. Çalıştırma ilerlerken, tasarımcı bu çalışma için canlı bir görünüm gösterir.

Visual Studio veya Azure portalında önceki çalışma geçmişlerini görüntülemek için: 

* Görsel Stüdyo Bulut Explorer'ı açın. Mantık uygulamanızı bulun, uygulamanın kısayol menüsünü açın. **Çalıştır geçmişini aç'ı**seçin.

  > [!TIP]
  > Visual Studio 2019'da bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelerin olup olmadığını kontrol edin.

* Azure portalında mantık uygulamanızı bulun. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin. 

## <a name="create-automated-deployment-templates"></a>Otomatik dağıtım şablonları oluşturma

Bir mantık uygulaması çözümü oluşturduktan sonra, uygulamanızı dünyanın herhangi bir Azure bölgesine [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/templates/overview.md) olarak yakalayabilir ve dağıtabilirsiniz. Bu özelliği, hem uygulamanızın farklı sürümlerini oluşturmak hem de çözümünüzü Azure Ardışık Larına entegre etmek için parametreleri değiştirmek için kullanabilirsiniz. Tüm bağımlılıklarla tüm çözümü tek bir şablon olarak yönetebilmeniz için dağıtım şablonuna Azure İşlevlerini de ekleyebilirsiniz. Mantık uygulaması dağıtımını nasıl [otomatikleştirebilirsiniz](logic-apps-azure-resource-manager-templates-overview.md)öğrenin.

Azure işlevine sahip örnek dağıtım şablonu için Azure hızlı başlatma şablon deposunu kontrol [edin.](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Mantık Uygulamaları için diğer örnekleri ve senaryoları bulma](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
