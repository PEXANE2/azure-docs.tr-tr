---
title: İş akışı hatalarının sorunlarını giderme ve tanılama
description: Azure Logic Apps 'teki iş akışlarınızda sorunları, hataları ve hataları nasıl giderebileceğinizi ve tanılamanıza öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: 79cc9d1bf7aa9e8848197525646b0a3646a558d2
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666814"
---
# <a name="troubleshoot-and-diagnose-workflow-failures-in-azure-logic-apps"></a>Azure Logic Apps iş akışı hatalarının sorunlarını giderme ve tanılama

Mantıksal uygulamanız uygulamanızdaki sorunları tanılamanıza ve hata ayıklamanıza yardımcı olabilecek bilgiler oluşturur. Azure portal aracılığıyla iş akışındaki her adımı inceleyerek bir mantıksal uygulamayı tanılayabilirsiniz. Ya da çalışma zamanı hata ayıklaması için bir iş akışına bazı adımlar ekleyebilirsiniz.

## <a name="review-trigger-history"></a>Tetikleyici geçmişini gözden geçirme

Her mantıksal uygulama tetikleyici ile başlar. Tetikleyici başlamazsa, önce tetikleyici geçmişini kontrol edin. Bu geçmiş, mantıksal uygulamanızın yaptığı tüm tetikleyici girişimlerini ve her tetikleyici denemesine yönelik girişler ve çıktılar hakkında ayrıntıları listeler.

1. Tetikleyicinin tetikleyip tetiklenmediğini denetlemek için, mantıksal uygulama menüsünde **genel bakış**' ı seçin. **Tetikleyici geçmişi**altında, tetikleyicinin durumunu gözden geçirin.

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

   ![Tetikleyici geçmişini gözden geçirme](./media/logic-apps-diagnosing-failures/logic-app-trigger-history-overview.png)

   > [!TIP]
   > * Beklediğiniz verileri bulamazsanız, araç çubuğunda **Yenile** ' yi seçmeyi deneyin.
   > * Listede çok sayıda tetikleme denemesi görünüyorsa ve istediğiniz girişi bulamazsanız, listeyi filtrelemeyi deneyin.

   Tetikleyici girişimi için olası durumlar şunlardır:

   | Durum | Açıklama | 
   | ------ | ----------- | 
   | **Baarı** | Tetikleyici bitiş noktasını denetledi ve kullanılabilir verileri buldu. Genellikle, bu durum yanında "tetiklenen" bir durum da görünür. Aksi takdirde, tetikleyici tanımında karşılanmayan bir koşul veya `SplitOn` komutu olabilir. <p>Bu durum el ile tetikleyici, yineleme tetikleyicisi veya yoklama tetikleyicisi için uygulanabilir. Bir tetikleyici başarıyla çalıştırılabilir, ancak eylemler işlenmeyen hatalar üretmediğinde, çalıştırma yine de başarısız olabilir. | 
   | **Atlandı** | Tetikleyici bitiş noktasını denetledi ancak hiç veri bulmadı. | 
   | **Başarısız** | Bir hata oluşmuştur. Başarısız bir tetikleyici için oluşturulan hata iletilerini gözden geçirmek için, bu tetikleyici denemesini seçin ve **çıktılar**' i seçin. Örneğin, geçerli olmayan girişler bulabilirsiniz. | 
   ||| 

   Mantıksal uygulamanız birden çok öğe bulduğunda gerçekleşen aynı tarih ve saate sahip birden çok tetikleyici girişi olabilir. 
   Tetikleyici her tetiklendiğinde Logic Apps altyapısı, iş akışınızı çalıştırmak için bir mantıksal uygulama örneği oluşturur. Varsayılan olarak, her örnek paralel olarak çalışır, böylece bir çalıştırması başlatmadan önce herhangi bir iş akışının beklemesi gerekmez.

   > [!TIP]
   > Bir sonraki tekrarmayı beklemeden Tetikleyiciyi yeniden denetleyebilirsiniz. Genel Bakış araç çubuğunda **tetikleyiciyi Çalıştır**' ı seçin ve sonra da bir denetimi zorlayan tetikleyiciyi seçin. Ya da Logic Apps Tasarımcı araç çubuğunda **Çalıştır** ' ı seçin.

3. Tetikleyici denemesinin ayrıntılarını incelemek için tetikleyici **geçmişi**altında, bu tetikleyici denemesini seçin. 

   ![Tetikleyici girişimi seçin](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

4. , Tetikleyicinin oluşturduğu girişleri ve çıkışları gözden geçirin. Tetikleyici çıkışları, tetikleyiciden gelen verileri gösterir. Bu çıktılar, tüm özelliklerin beklenen şekilde döndürülüp döndürülmediğini belirlemenize yardımcı olabilir.

   > [!NOTE]
   > Anladığınızı bir içerik bulursanız Azure Logic Apps [farklı içerik türlerini nasıl işleyeceğinizi](../logic-apps/logic-apps-content-type.md)öğrenin.

   ![Tetikleme çıkışları](./media/logic-apps-diagnosing-failures/trigger-outputs.png)

## <a name="review-run-history"></a>Çalıştırma geçmişini gözden geçirme

Tetiklenen her tetikleyici bir iş akışı çalıştırması başlatır. İş akışındaki her adımın durumu ve her adımın giriş ve çıkışları dahil olmak üzere, çalışma sırasında ne olduğunu inceleyebilirsiniz.

1. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Çalışma **geçmişi**altında, tetiklenen tetikleyici için çalıştırmayı gözden geçirin.

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

   ![Çalıştırma geçmişini gözden geçirme](./media/logic-apps-diagnosing-failures/logic-app-runs-history-overview.png)

   > [!TIP]
   > * Beklediğiniz verileri bulamazsanız, araç çubuğunda **Yenile** ' yi seçmeyi deneyin.
   > * Listede birçok çalıştırma görünüyorsa ve istediğiniz girişi bulamazsanız, listeyi filtrelemeyi deneyin.

   İşte bir çalıştırma için olası durumlar şunlardır:

   | Durum | Açıklama | 
   | ------ | ----------- | 
   | **Baarı** | Tüm eylemler başarılı oldu. <p>Belirli bir eylemde herhangi bir hata meydana gelirse, iş akışında aşağıdaki bir eylem bu hatayı işledi. | 
   | **Başarısız** | En az bir eylem başarısız oldu ve hata işlemek için iş akışında sonraki hiçbir eylem ayarlanmadı. | 
   | **Yürütüldükten** | İş akışı çalışıyor ancak iptal isteği alındı. | 
   | **Çalıştıran** | İş akışı şu anda çalışıyor. <p>Bu durum, kısıtlanmış iş akışları veya geçerli fiyatlandırma planı nedeniyle gerçekleşebilir. Daha fazla bilgi için [fiyatlandırma sayfasındaki eylem sınırlarına](https://azure.microsoft.com/pricing/details/logic-apps/)bakın. [Tanılama günlüğü](../logic-apps/logic-apps-monitor-your-logic-apps.md)ayarlarsanız, gerçekleşen tüm kısıtlama olayları hakkında da bilgi edinebilirsiniz. | 
   ||| 

2. Belirli bir çalıştırmada her adımın ayrıntılarını gözden geçirin. Çalışma **geçmişi**altında, incelemek istediğiniz çalıştırmayı seçin.

   ![Çalıştırma geçmişini gözden geçirme](./media/logic-apps-diagnosing-failures/logic-app-run-history.png)

   Çalıştırmanın başarılı veya başarısız olup olmadığı, çalışma Ayrıntıları görünümü her adımı ve başarılı veya başarısız olup olmadığını gösterir.

   ![Bir mantıksal uygulama çalıştırmasının ayrıntılarını görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

3. Belirli bir adımla ilgili girişleri, çıkışları ve hata iletilerini incelemek için, şeklin genişleyeceği ve ayrıntıları gösterdiği şekilde bu adımı seçin. Örneğin:

   ![Adım ayrıntıları görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Çalışma zamanı hatalarını ayıklamayı gerçekleştir

Hata ayıklamaya yardımcı olmak için, tetikleyici ve çalıştırma geçmişini gözden geçirme ile birlikte bir iş akışına Tanılama adımları ekleyebilirsiniz. Örneğin, HTTP isteklerini incelemenize ve tam boyutunu, şeklini ve biçimini belirleyebilmeniz için [Web kancası Sınayıcısı](https://webhook.site/) hizmetini kullanan adımları ekleyebilirsiniz.

1. [Web kancası Sınayıcısı](https://webhook.site/) 'nı ziyaret edin ve oluşturulan benzersiz URL 'yi kopyalayın

2. Mantıksal uygulamanızda, test etmek istediğiniz gövde içeriğiyle bir HTTP POST eylemi ekleyin, örneğin, bir ifade veya başka bir adım çıktısı.

3. Web kancası test eden URL 'sini HTTP POST eylemine yapıştırın.

4. Logic Apps altyapısından oluşturulduğunda bir isteğin nasıl biçimlendirildiğini gözden geçirmek için mantıksal uygulamayı çalıştırın ve Ayrıntılar için Web kancası Sınayıcısı ' nı görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Mantıksal uygulamanızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md)
