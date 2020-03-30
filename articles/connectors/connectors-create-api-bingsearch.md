---
title: Bing Arama'ya bağlan
description: Azure Mantık Uygulamaları'nı kullanarak Bing Arama'da sonuç bulan görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: e547ae59f7b3260f46756825bca2bef1c10bcc97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665896"
---
# <a name="find-results-in-bing-search-by-using-azure-logic-apps"></a>Azure Mantık Uygulamalarını kullanarak Bing Arama'da sonuçları bulma

Bu makalede, Bing Arama bağlayıcısı ile bir mantık uygulaması nın içinden Bing Arama aracılığıyla haberleri, videoları ve diğer öğeleri nasıl bulabileceğiniz gösterilmektedir. Bu şekilde, arama sonuçlarını işlemek için görevleri ve iş akışlarını otomatikleştiren ve bu öğeleri diğer eylemler için kullanılabilir hale getiren mantık uygulamaları oluşturabilirsiniz. 

Örneğin, arama ölçütlerine göre haber öğeleri bulabilir ve Twitter'ın bu öğeleri Twitter akışınızda tweet olarak yayınlamasını sağlayabilirsiniz.

Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)
Bağlayıcıya özel teknik bilgiler için [Bing Arama bağlayıcısı referansına](https://docs.microsoft.com/connectors/bingsearch/)bakın.

## <a name="prerequisites"></a>Ön koşullar

* [Bilişsel Hizmetler hesabı](../cognitive-services/cognitive-services-apis-create-account.md)

* Mantık uygulamanızdan Bing Arama API'lerine erişim sağlayan Bing [Arama API anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api)

* Etkinlik Hub'ınıza erişmek istediğiniz mantık uygulaması. Bir Bing Arama tetikleyicisi ile mantık uygulamabaşlatmak için, boş bir [mantık uygulaması](../logic-apps/quickstart-create-first-logic-app-workflow.md)gerekir.

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>Bing Arama tetikleyicisi ekleme

Azure Logic Apps'ta, her mantık uygulaması, belirli bir olay olduğunda veya belirli bir koşul yerine getirildiğinde ateş eden bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her ateşleninher yerinde, Logic Apps motoru bir mantık uygulaması örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

1. Azure portalında veya Visual Studio'da, Logic App Designer'ı açan boş bir mantık uygulaması oluşturun. Bu örnekte Azure portalı kullanır.

2. Arama kutusuna filtreniz olarak "Bing araması" girin. Tetikleyiciler listesinden istediğiniz tetikleyiciyi seçin.

   Bu örnekte şu tetikleyici kullanır: **Bing Arama - Yeni haber makalesi**

   ![Bing Arama tetikleyicisi bul](./media/connectors-create-api-bing-search/add-trigger.png)

3. Bağlantı ayrıntıları için istenirseniz, [Bing Arama bağlantınızı şimdi oluşturun.](#create-connection)
Veya bağlantınız zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnekte, Bing Arama'dan eşleşen haber makalelerini döndürmek için ölçütler sağlayın.

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | Arama Sorgusu | Evet | <*arama sözcükleri*> | Kullanmak istediğiniz arama anahtar kelimelerini girin. |
   | Pazar | Evet | <*Yerel ayar*> | Arama yerel. Varsayılan değer "en-US", ancak başka bir değer seçebilirsiniz. |
   | Güvenli Arama | Evet | <*arama düzeyi*> | Yetişkinlere uygun içeriği hariç tetmek için filtre düzeyi. Varsayılan değer "Orta" dır, ancak başka bir düzey seçersiniz. |
   | Sayı | Hayır | <*sonuç sayısı*> | Belirtilen sonuç sayısını döndürün. Varsayılan değer 20'dir, ancak başka bir değer belirtebilirsiniz. Döndürülen sonuçların gerçek sayısı belirtilen sayıdan daha az olabilir. |
   | Uzaklık | Hayır | <*atlama değeri*> | Sonuçları döndürmeden önce atlanması gereken sonuç sayısı |
   |||||

   Örnek:

   ![Tetikleyiciyi ayarlama](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. Tetikleyicinin sonuçları denetlemesini ne sıklıkta istediğinize göre aralığı ve sıklığı seçin.

5. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

6. Şimdi, tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevler için mantık uygulamanıza bir veya daha fazla eylem eklemeye devam edin.

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>Bing Arama eylemi ekleme

Azure Logic Apps'ta [eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) iş akışınızda bir tetikleyici veya başka bir eylemi izleyen bir adımdır. Bu örnekte, mantık uygulaması, belirtilen ölçütlerle eşleşen haber makalelerini döndüren bir Bing Arama tetikleyicisiyle başlar.

1. Azure portalında veya Visual Studio'da mantık uygulamanızı Logic App Designer'da açın. Bu örnekte Azure portalı kullanır.

2. Tetikleyici veya eylemin altında Yeni **adım** > **bir eylem ekle'yi**seçin.

   Bu örnekte şu tetikleyici kullanır:

   **Bing Arama - Yeni haber makalesi**

   ![Eylem ekleme](./media/connectors-create-api-bing-search/add-action.png)

   Varolan adımlar arasında eylem eklemek için farenizi bağlanan okun üzerine taşıyın. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

3. Arama kutusuna filtreniz olarak "Bing araması" girin.
Eylemler listesinden, istediğiniz eylemi seçin.

   Bu örnekte şu eylem kullanır:

   **Bing Arama - Sorguya göre haber listesi**

   ![Bing Arama eylemini bul](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. Bağlantı ayrıntıları için istenirseniz, [Bing Arama bağlantınızı şimdi oluşturun.](#create-connection) Veya bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

   Bu örnekte, tetikleyicinin sonuçlarının bir alt kümesini döndürmek için ölçütler sağlayın.

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | Arama Sorgusu | Evet | <*arama-ifade*> | Tetikleyici sonuçlarını sorgulamak için bir ifade girin. Dinamik içerik listesindeki alanlardan seçim yapabilir veya ifade oluşturucusuyla bir ifade oluşturabilirsiniz. |
   | Pazar | Evet | <*Yerel ayar*> | Arama yerel. Varsayılan değer "en-US", ancak başka bir değer seçebilirsiniz. |
   | Güvenli Arama | Evet | <*arama düzeyi*> | Yetişkinlere uygun içeriği hariç tetmek için filtre düzeyi. Varsayılan değer "Orta" dır, ancak başka bir düzey seçersiniz. |
   | Sayı | Hayır | <*sonuç sayısı*> | Belirtilen sonuç sayısını döndürün. Varsayılan değer 20'dir, ancak başka bir değer belirtebilirsiniz. Döndürülen sonuçların gerçek sayısı belirtilen sayıdan daha az olabilir. |
   | Uzaklık | Hayır | <*atlama değeri*> | Sonuçları döndürmeden önce atlanması gereken sonuç sayısı |
   |||||

   Örneğin, kategori adı "teknoloji" sözcüğünün bulunduğu sonuçları istediğinizi varsayalım.

   1. Dinamik içerik listesinin görünmesi için **Arama Sorgusu** kutusunu tıklatın. 
   Bu listeden, ifade oluşturucugörünmesini ifade etmek için **İfade'yi** seçin. 

      ![Bing Arama tetikleyicisi](./media/connectors-create-api-bing-search/bing-search-action.png)

      Artık ifadenizi oluşturmaya başlayabilirsiniz.

   2. İşlevler listesinden, daha sonra ifade kutusunda görünen **contains()** işlevini seçin. Alan listesinin yeniden görünmesi için **Dinamik içeriği** tıklatın, ancak imlecin parantez içinde kaldığından emin olun.

      ![İşlev seçme](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. Alan listesinden, parametreye dönüştürülen **Kategori'yi**seçin. 
   İlk parametreden sonra virgül ekleyin ve virgülden sonra şu sözcüğü ekleyin:`'tech'` 

      ![Bir alan seçin](./media/connectors-create-api-bing-search/expression-select-field.png)

   4. İşiniz bittiğinde **Tamam**’ı seçin.

      İfade artık bu biçimde **Arama Sorgusu** kutusunda görünür:

      ![Tamamlanmış ifade](./media/connectors-create-api-bing-search/resolved-expression.png)

      Kod görünümünde, bu ifade bu biçimde görünür:

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>Bing Arama'ya bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Bağlantı bilgileri istendiğinde şu ayrıntıları sağlayın:

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | Bağlantı Adı | Evet | <*bağlantı adı*> | Bağlantınız için oluşturulacak ad |
   | API Sürümü | Evet | <*API sürümü*> | Varsayılan olarak, Bing Arama API sürümü geçerli sürüme ayarlanır. Gerektiğinde önceki bir sürümü seçebilirsiniz. |
   | API Anahtarı | Evet | <*API anahtarı*> | Daha önce aldığınız Bing Arama API anahtarı. Anahtarınız yoksa, [API anahtarınızı hemen](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api)alın. |  
   |||||  

   Örnek:

   ![Bağlantı oluşturma](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. İşiniz bittiğinde **Oluştur**’u seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için, bağlayıcının Swagger dosyasında açıklandığı gibi, [bağlayıcının başvuru sayfasına](/connectors/bingsearch/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
