---
title: Sunucusuz senaryo-Azure hizmetleri ile müşteri öngörüleri panosu oluşturma | Microsoft Docs
description: Azure Logic Apps ve Azure Işlevleri ile bir müşteri panosu oluşturarak müşteri geri bildirimlerini, sosyal medya verilerini ve daha fazlasını yönetin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: estfan, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.topic: article
ms.date: 03/15/2018
ms.openlocfilehash: 6f0176253d0f1953e9c779a317cfe7bf453acb8f
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385438"
---
# <a name="create-streaming-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Azure Logic Apps ve Azure Işlevleri ile akış müşteri öngörüleri panosu oluşturma

Azure, altyapı hakkında düşünmenize gerek kalmadan, uygulamaları bulutta hızlı bir şekilde oluşturup barındırmanıza yardımcı olan [sunucusuz](https://azure.microsoft.com/solutions/serverless/) araçlar sunar. Bu öğreticide, müşteri geri bildirimlerinden tetiklenen, Machine Learning ile geri bildirimleri çözümleyen ve Power BI veya Azure Data Lake gibi bir kaynakta Öngörüler yayımlayan bir pano oluşturabilirsiniz.

Bu çözüm için, bu anahtar Azure bileşenlerini sunucusuz uygulamalar için kullanırsınız: [Azure işlevleri](https://azure.microsoft.com/services/functions/) ve [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).
Azure Logic Apps, bulutta sunucusuz bir iş akışı altyapısı sağlar ve böylece sunucusuz bileşenler arasında düzenleme yapabilir ve 200 + hizmet ve API 'lere bağlanabilirsiniz. Azure Işlevleri, bulutta sunucusuz bilgi işlem sağlar. Bu çözüm, önceden tanımlanmış anahtar sözcüklere göre müşteri bazında ara ve bayrak eklemek için Azure Işlevlerini kullanır.

Bu senaryoda, müşterilerden geri bildirim bulmayı tetikleyen bir mantıksal uygulama oluşturursunuz. Müşteri geri bildirimlerine yanıt vermenize yardımcı olan bazı bağlayıcılar, Outlook.com, Office 365, araştırma maymun, Twitter ve [bir Web formundan gelen http isteği](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/)içerir. Oluşturduğunuz iş akışı Twitter 'da bir diyez etiketi izler.

[Tüm çözümü Visual Studio 'da](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) oluşturabilir ve [çözümü Azure Resource Manager şablonuyla dağıtabilirsiniz](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md). Bu çözümün nasıl oluşturulacağını gösteren bir video kılavuzu için, [Bu Channel 9 videosunu izleyin](https://aka.ms/logicappsdemo). 

## <a name="trigger-on-customer-data"></a>Müşteri verilerinde Tetikle

1. Azure portal veya Visual Studio 'da boş bir mantıksal uygulama oluşturun. 

   Logic Apps 'e yeni başladıysanız, Azure portal veya [Visual Studio hızlı](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)başlangıcı [için hızlı](../logic-apps/quickstart-create-first-logic-app-workflow.md) başlangıcı inceleyin.

2. Mantıksal uygulama Tasarımcısı ' nda, bu eyleme sahip Twitter tetikleyicisini bulun ve ekleyin: **Yeni bir tweet gönderildiğinde**

3. Bir anahtar sözcük veya diyez etiketi temelinde çok fazla ara için dinleme yapmak üzere tetikleyiciyi ayarlayın.

   Twitter tetikleyicisi gibi yoklama tabanlı tetikleyicilerle, yinelenme özelliği mantıksal uygulamanın yeni öğeleri ne sıklıkta denetleyeceğini belirler.

   ![Twitter tetikleyicisi örneği][1]

Bu mantıksal uygulama artık tüm yeni çişetler üzerinde ateşlenir. Daha sonra, ifade edilen duyguları daha iyi anlayabilmeniz için Tweet verilerini alıp çözümleyebilirsiniz. 

## <a name="analyze-tweet-text"></a>Tweet metnini çözümle

Bazı metinlerin arkasındaki yaklaşımı algılamak için Azure bilişsel [Hizmetler](https://azure.microsoft.com/services/cognitive-services/)' i kullanabilirsiniz.

1. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici altında **yeni adım**' ı seçin.

2. **Metin analizi** bağlayıcısını bulun.

3. Yaklaşımı **Algıla** eylemini seçin.

4. İstenirse Metin Analizi hizmeti için geçerli bir bilişsel hizmetler anahtarı sağlayın.

5. **Istek gövdesi**altında, analiz için giriş olarak Tweet metnini sağlayan **Tweet metin** alanını seçin.

Tweet hakkında tweet verilerini ve öngörülerini aldıktan sonra, artık diğer ilgili bağlayıcıları ve bunların eylemlerini kullanabilirsiniz:

* **Power BI-akış veri kümesine satır ekleme**: Power BI panosunda gelen bağlı alanı görüntüleyin.
* **Azure Data Lake-ekleme dosyası**: Analiz işlerine dahil etmek için bir Azure Data Lake veri kümesine müşteri verileri ekleyin.
* **SQL-satır ekle**: Verileri daha sonra almak üzere bir veritabanında depolayın.
* **Bolluk-Ileti gönder**: Eylem gerektirebilecek olumsuz geri bildirimler hakkında bir bolluk kanalına bildirim gönderin.

Ayrıca, verileriniz üzerinde özel işlem gerçekleştirebilmeniz için bir Azure Işlevi de oluşturabilirsiniz. 

## <a name="process-data-with-azure-functions"></a>Azure Işlevleri ile verileri işleme

Bir işlev oluşturmadan önce, Azure aboneliğinizde bir işlev uygulaması oluşturun. Ayrıca, mantıksal uygulamanızın bir işlevi doğrudan çağırması için, işlevin bir HTTP tetikleyici bağlaması olması gerekir, örneğin, **Httptrigger** şablonunu kullanın. [Azure Portal ilk işlev uygulamanızı ve işlevinizi oluşturmayı](../azure-functions/functions-create-first-azure-function-azure-portal.md)öğrenin.

Bu senaryo için, Azure işlevinizin istek gövdesi olarak Tweet metnini kullanın. İşlev kodunuzda, Tweet metninin bir anahtar sözcük mi yoksa tümcecik mi içerdiğini belirleyen mantığı tanımlayın. İşlevi senaryo için gereken şekilde basit veya karmaşık tutun.
İşlevin sonunda, mantıksal uygulamaya bazı verilerle bir yanıt döndürün, örneğin `containsKeyword` veya karmaşık bir nesne gibi basit bir Boole değeri.

> [!TIP]
> Mantıksal uygulamadaki bir işlevden karmaşık bir yanıta erişmek için **JSON Ayrıştır** eylemini kullanın.

İşiniz bittiğinde, işlevini kaydedin ve sonra oluşturduğunuz mantıksal uygulamada işlevi bir eylem olarak ekleyin.

## <a name="add-azure-function-to-logic-app"></a>Logic App 'e Azure işlevi ekleme

1. Mantıksal uygulama Tasarımcısı ' nda, yaklaşımı **Algıla** eyleminin altında **yeni adım**' ı seçin.

2. **Azure işlevleri** bağlayıcısını bulun ve ardından oluşturduğunuz işlevi seçin.

3. **Istek gövdesi**altında **Tweet metin**' i seçin.

![Azure Işlev adımı yapılandırıldı][2]

## <a name="run-and-monitor-your-logic-app"></a>Mantıksal uygulamanızı çalıştırma ve izleme

Mantıksal uygulamanıza yönelik geçerli veya önceki çalıştırmaları gözden geçirmek için, Azure portal, Visual Studio 'da veya Azure REST API 'Leri ve SDK 'Ları aracılığıyla Azure Logic Apps sağladığı zengin hata ayıklama ve izleme yeteneklerini kullanabilirsiniz.

Mantıksal uygulamanızı kolayca test etmek için mantıksal uygulama Tasarımcısı 'nda **tetikleyiciyi Çalıştır**' ı seçin. Bu tetikleyici, ölçütlerinizi karşılayan bir tweet bulunana kadar, belirtilen zamanlamaya göre ara değer olarak doldurulabilir. Çalıştırma ilerledikçe tasarımcı bu çalıştırma için canlı bir görünüm gösterir.

Visual Studio 'da veya Azure portal önceki çalıştırma geçmişlerini görüntülemek için: 

* Visual Studio Cloud Explorer 'ı açın. Mantıksal uygulamanızı bulun, uygulamanın kısayol menüsünü açın. **Çalıştırma geçmişini aç**' ı seçin.

  > [!TIP]
  > Visual Studio 2019 ' de bu komuta sahip değilseniz, Visual Studio için en son güncelleştirmelere sahip olup olmadığınızı kontrol edin.

* Azure portal mantıksal uygulamanızı bulun. Mantıksal uygulamanızın menüsünde **genel bakış**' ı seçin. 

## <a name="create-automated-deployment-templates"></a>Otomatik dağıtım şablonları oluşturma

Bir mantıksal uygulama çözümü oluşturduktan sonra, uygulamanızı, dünyanın herhangi bir Azure bölgesine [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-overview.md#template-deployment) olarak yakalayabilir ve dağıtabilirsiniz. Bu özelliği, uygulamanızın farklı sürümlerini oluşturmak ve çözümünüzü Azure Pipelines tümleştirmek için parametreleri değiştirmek üzere kullanabilirsiniz. Tüm çözümü tek bir şablon olarak tüm bağımlılıklarla yönetebilmeniz için dağıtım şablonunuza Azure Işlevleri de ekleyebilirsiniz. [Mantıksal uygulama dağıtımını otomatikleştirme](logic-apps-azure-resource-manager-templates-overview.md)hakkında bilgi edinin.

Azure işlevi içeren örnek bir dağıtım şablonu için [Azure hızlı başlangıç şablonu deposuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps yönelik diğer örnekleri ve senaryoları bulun](logic-apps-examples-and-scenarios.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png
