---
title: Durumu izleme, geçmişi görüntüleme ve uyarıları ayarlama
description: Azure Mantık Uygulamaları'nda çalıştırma durumunu kontrol ederek, tetik geçmişini gözden geçirerek ve uyarıları etkinleştirerek mantık uygulamalarını sorun giderme
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 495877f1c839de2cf3583a37180054c91bd9f139
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76907779"
---
# <a name="monitor-run-status-review-trigger-history-and-set-up-alerts-for-azure-logic-apps"></a>Çalışma durumunu izleyin, tetikleyici geçmişi gözden geçirin ve Azure Mantık Uygulamaları için uyarılar ayarlayın

Bir [mantık uygulaması oluşturup çalıştırdıktan](../logic-apps/quickstart-create-first-logic-app-workflow.md)sonra, bu mantık uygulamasının çalışma durumunu kontrol edebilir, geçmişi, [tetikleyici geçmişi](#review-trigger-history)ve performansı [çalıştırabilirsiniz.](#review-runs-history) Hatalar veya diğer olası sorunlar la ilgili bildirimler almak için [uyarılar](#add-azure-alerts)ayarlayın. Örneğin, "bir saat içinde beşten fazla çalıştırma başarısız olduğunda" algılayan bir uyarı oluşturabilirsiniz.

Gerçek zamanlı olay izleme ve daha zengin hata ayıklama için, Azure Monitor [günlüklerini](../azure-monitor/overview.md)kullanarak mantık uygulamanız için tanılama günlüğü ayarlayın. Bu Azure hizmeti, bulut ve şirket içi ortamlarınızı izlemenize yardımcı olur, böylece kullanılabilirliklerini ve performanslarını daha kolay koruyabilirsiniz. Ardından tetikleyici olaylar, olayları çalıştırma ve eylem olayları gibi olayları bulabilir ve görüntüleyebilirsiniz. Bu bilgileri Azure [Monitor günlüklerinde](../azure-monitor/platform/data-platform-logs.md)depolayarak, bu bilgileri bulmanıza ve çözümlemenize yardımcı olacak [günlük sorguları](../azure-monitor/log-query/log-query-overview.md) oluşturabilirsiniz. Bu tanılama verilerini Azure Depolama ve Azure Etkinlik Hub'ları gibi diğer Azure hizmetleriyle de kullanabilirsiniz. Daha fazla bilgi için [Azure Monitor'u kullanarak mantık uygulamalarını izleyin'](../logic-apps/monitor-logic-apps-log-analytics.md)e bakın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

<a name="review-runs-history"></a>

## <a name="review-runs-history"></a>İnceleme geçmişi çalışır

Tetikleyici bir öğe veya olay için her yangın da yanar, Logic Apps altyapısı her öğe veya olay için ayrı bir iş akışı örneği oluşturur ve çalıştırAr. Varsayılan olarak, her iş akışı örneği paralel olarak çalışır, böylece hiçbir iş akışı çalıştırmayı başlatmadan önce beklemek zorunda değildir. İş akışındaki her adımın durumu ve her adım için giriş ve çıktılar da dahil olmak üzere, bu çalışma sırasında neler olduğunu gözden geçirebilirsiniz.

1. Azure [portalında](https://portal.azure.com)Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı bulun ve açın.

   Mantık uygulamanızı ana Azure arama kutusunda bulmak `logic apps`için Logic Apps'ı girin ve ardından **Mantık Uygulamaları'nı**seçin.

   !["Logic Apps" hizmetini bulun ve seçin](./media/monitor-logic-apps/find-your-logic-app.png)

   Azure portalı, Azure aboneliklerinizle ilişkili tüm mantık uygulamalarını gösterir. Bu listeye ad, abonelik, kaynak grubu, konum ve benzeri temellere göre filtre uygulayabilirsiniz.

   ![Aboneliklerle ilişkili mantık uygulamalarını görüntüleme](./media/monitor-logic-apps/logic-apps-list-in-subscription.png)

1. Mantık uygulamanızı seçin ve ardından **Genel Bakış'ı**seçin.

   Genel bakış bölmesinde, **Koşular geçmişi**altında, tüm geçmiş, geçerli ve mantık uygulamanız için bekleyen tüm çalışır görünür. Listede çok sayıda çalıştırma gösteriliyorsa ve istediğiniz girişi bulamıyorsanız, listeyi filtreleyi deneyin. Beklediğiniz verileri bulamazsanız, araç çubuğunda **Yenile'yi** seçmeyi deneyin.

   ![Genel bakış, geçmişi ve diğer mantık uygulama bilgilerini çalıştırın](./media/monitor-logic-apps/overview-pane-logic-app-details-run-history.png)

   Burada bir mantık uygulaması çalıştırmak için olası durumları şunlardır:

   | Durum | Açıklama |
   |--------|-------------|
   | **İptal Edildi** | İş akışı çalışıyordu, ancak iptal isteği aldı |
   | **Başarısız** | En az bir eylem başarısız oldu ve iş akışındaki daha sonraki eylemler başarısız lığı işlemek için ayarlandı |
   | **Çalışıyor** | İş akışı şu anda çalışıyor. <p>Bu durum, daraltılmış iş akışları veya geçerli fiyatlandırma planı nedeniyle de görünebilir. Daha fazla bilgi için [fiyatlandırma sayfasındaki eylem sınırlarına](https://azure.microsoft.com/pricing/details/logic-apps/)bakın. [Tanılama günlüğe kaydetmeyi](../logic-apps/monitor-logic-apps.md)ayarlarsanız, meydana gelen gaz olayları hakkında bilgi alabilirsiniz. |
   | **Başarılı oldu** | Tüm eylemler başarılı oldu. <p>**Not**: Belirli bir eylemde herhangi bir hata meydana geldiyse, iş akışındaki daha sonraki bir eylem bu hatayı işler. |
   | **Bekleniyor** | İş akışı henüz başlamadı veya duraklatıldı, örneğin, hala çalışan daha önceki bir iş akışı nedeniyle. |
   |||

1. Belirli bir çalıştırmaiçin adımları ve diğer bilgileri gözden geçirmek için, **Runs geçmişi**altında, bu çalıştırmayı seçin.

   ![İncelecek belirli bir çalıştırma seçin](./media/monitor-logic-apps/select-specific-logic-app-run.png)

   **Mantık uygulaması çalıştırma** bölmesi seçili çalıştırmadaki her adımı, her adımın çalışma durumunu ve her adımın çalışması için geçen süreyi gösterir, örneğin:

   ![Belirli bir çalıştırmadaki her eylem](./media/monitor-logic-apps/logic-app-run-pane.png)

   Bu bilgileri liste şeklinde görüntülemek **için, Mantık uygulaması çalıştır** araç çubuğunda **Ayrıntıları Çalıştır'ı**seçin.

   ![Araç çubuğunda "Ayrıntıları Çalıştır" seçeneğini belirleyin](./media/monitor-logic-apps/select-run-details-on-toolbar.png)

   Ayrıntıları Çalıştır görünümü her adımı, durumlarını ve diğer bilgileri gösterir.

   ![Çalışandaki her adımla ilgili ayrıntıları gözden geçirme](./media/monitor-logic-apps/review-logic-app-run-details.png)

   Örneğin, [Logic Apps için REST API'yi](https://docs.microsoft.com/rest/api/logic)kullandığınızda ihtiyaç duyabileceğiniz run'ın **Korelasyon Kimliği** özelliğini alabilirsiniz.

1. Belirli bir adım hakkında daha fazla bilgi almak için aşağıdaki seçenekleri seçin:

   * Mantık **uygulama çalışma** bölmesinde, şeklin genişlemesi için adımı seçin. Artık girişler, çıktılar ve bu adımda meydana gelen hatalar gibi bilgileri görüntüleyebilirsiniz, örneğin:

     ![Mantık uygulaması çalıştırma bölmesinde, başarısız adımı görüntüle](./media/monitor-logic-apps/specific-step-inputs-outputs-errors.png)

   * Mantık **uygulaması çalışma ayrıntıları** bölmesinde, istediğiniz adımı seçin.

     ![Çalıştır ayrıntıları bölmesinde, başarısız adımı görüntüle](./media/monitor-logic-apps/select-failed-step-in-failed-run.png)

     Artık bu adıma ait giriş ler ve çıktılar gibi bilgileri görüntüleyebilirsiniz, örneğin:

   > [!NOTE]
   > Tüm çalışma zamanı ayrıntıları ve olaylar Logic Apps hizmeti nde şifrelenir. Bunlar yalnızca bir kullanıcı bu verileri görüntülemek için istekte olduğunda deşifre edilir. Giriş [ve çıktıları çalışma geçmişinde gizleyebilir](../logic-apps/logic-apps-securing-a-logic-app.md#obfuscate) veya [Azure Role Tabanlı Erişim Denetimi 'ni (RBAC)](../role-based-access-control/overview.md)kullanarak kullanıcının bu bilgilere erişimini denetleyebilirsiniz.

<a name="review-trigger-history"></a>

## <a name="review-trigger-history"></a>Tetikleyici geçmişini gözden geçirme

Her mantık uygulaması çalışması bir tetikleyici ile başlar. Tetikleyici geçmişi, mantık uygulamanızın yaptığı tüm tetikleyici girişimlerini ve her tetikleyici girişimi için giriş ve çıktılar hakkındaki bilgileri listeler.

1. Azure [portalında](https://portal.azure.com)Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı bulun ve açın.

   Mantık uygulamanızı ana Azure arama kutusunda bulmak `logic apps`için Logic Apps'ı girin ve ardından **Mantık Uygulamaları'nı**seçin.

   !["Logic Apps" hizmetini bulun ve seçin](./media/monitor-logic-apps/find-your-logic-app.png)

   Azure portalı, Azure aboneliklerinizle ilişkili tüm mantık uygulamalarını gösterir. Bu listeye ad, abonelik, kaynak grubu, konum ve benzeri temellere göre filtre uygulayabilirsiniz.

   ![Aboneliklerle ilişkili mantık uygulamalarını görüntüleme](./media/monitor-logic-apps/logic-apps-list-in-subscription.png)

1. Mantık uygulamanızı seçin ve ardından **Genel Bakış'ı**seçin.

1. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin. **Özet** bölümünde, **Değerlendirme**altında, tetikleyici geçmişi gör'üne **bakın'ı**seçin.

   ![Mantık uygulamanız için tetikgeçmişini görüntüleme](./media/monitor-logic-apps/overview-pane-logic-app-details-trigger-history.png)

   Tetikleme geçmişi bölmesi, mantık uygulamanızın yaptığı tüm tetikleme girişimlerini gösterir. Tetikleyici bir öğe veya olay için her yangın da, Logic Apps altyapısı iş akışını çalıştıran ayrı bir mantık uygulaması örneği oluşturur. Varsayılan olarak, her örnek paralel olarak çalışır, böylece hiçbir iş akışı çalıştırmayı başlatmadan önce beklemek zorunda değildir. Bu nedenle, mantık uygulamanız aynı anda birden çok öğeyi tetiklerse, her öğe için aynı tarih ve saate sahip bir tetikleyici girişi görüntülenir.

   ![Farklı öğeler için birden çok tetikleyici denemesi](./media/monitor-logic-apps/logic-app-trigger-history.png)

   Tetikleyici denemesi için olası durumlar şunlardır:

   | Durum | Açıklama |
   |--------|-------------|
   | **Başarısız** | Bir hata oluşmuştur. Başarısız bir tetikleyici için oluşturulan hata iletilerini gözden geçirmek için, girişimi tetikleyen kişiyi seçin ve **Çıktılar'ı**seçin. Örneğin, geçerli olmayan girişler bulabilirsiniz. |
   | **Atlandı** | Tetikleyici bitiş noktasını denetledi ancak veri bulamadı. |
   | **Başarılı oldu** | Tetikleyici bitiş noktasını denetledi ve kullanılabilir verileri buldu. Genellikle, bir "Ateş" durumu da bu durumun yanında görünür. Değilse, tetikleyici tanımı nın karşılanmayan bir koşulu veya `SplitOn` komutu olabilir. <p>Bu durum el ile tetikleyici, yineleme tetikleyicisi veya yoklama tetikleyicisi için geçerli olabilir. Bir tetikleyici başarılı bir şekilde çalışabilir, ancak eylemler işlenmemiş hatalar oluşturduğunda çalıştırmanın kendisi yine de başarısız olabilir. |
   |||

   > [!TIP]
   > Bir sonraki yinelemeyi beklemeden tetikleyiciyi yeniden denetleyebilirsiniz. Genel bakış araç çubuğunda Çalıştır **tetiği**seçin ve denetimi zorlayan tetiği seçin. Veya Logic Apps Designer araç çubuğunda **çalıştır'ı** seçin.

1. Tetikleyici bölmede belirli bir tetikleyici girişimi yle ilgili bilgileri görüntülemek için bu tetikleyici olayı seçin. Liste çok sayıda tetikleyici denemesi gösteriyorsa ve istediğiniz girişi bulamıyorsanız, listeyi filtreleyi deneyin. Beklediğiniz verileri bulamazsanız, araç çubuğunda **Yenile'yi** seçmeyi deneyin.

   ![Belirli tetikleyici denemeyi görüntüleme](./media/monitor-logic-apps/select-trigger-event-for-review.png)

   Şimdi seçili tetikleyici olay la ilgili bilgileri gözden geçirebilirsiniz, örneğin:

   ![Belirli tetikleyici bilgileri görüntüleme](./media/monitor-logic-apps/view-specific-trigger-details.png)

<a name="add-azure-alerts"></a>

## <a name="set-up-monitoring-alerts"></a>İzleme uyarıları ayarlama

Mantık uygulamanız için belirli ölçümlere veya aşılmış eşiklere dayalı uyarılar almak için [Azure Monitor'da uyarılar](../azure-monitor/platform/alerts-overview.md)ayarlayın. [Azure'daki ölçümler](../monitoring-and-diagnostics/monitoring-overview-metrics.md)hakkında bilgi edinin. [Azure Monitor'u](../log-analytics/log-analytics-overview.md)kullanmadan uyarıları ayarlamak için aşağıdaki adımları izleyin.

1. Mantık uygulaması menüsünde, **İzleme** **altında, Uyarılar** > **Yeni uyarı kuralını**seçin.

   ![Mantık uygulamanız için uyarı ekleme](./media/monitor-logic-apps/add-new-alert-rule.png)

1. **Kaynak**altında **oluştur kuralı** bölmesinde, zaten seçilmemişse mantık uygulamanızı seçin. **Koşul**altında , uyarıyı tetikleyen koşulu tanımlayabilmek için **Ekle'yi** seçin.

   ![Kural için bir koşul ekleme](./media/monitor-logic-apps/add-condition-for-rule.png)

1. **Yapılsinyal mantığı** bölmesinde, uyarı almak istediğiniz sinyali bulun ve seçin. Arama kutusunu kullanabilir veya sinyalleri alfabetik olarak sıralamak için **Sinyal adı** sütun üstbilgisini seçebilirsiniz.

   Örneğin, bir tetikleyici başarısız olduğunda bir uyarı göndermek istiyorsanız, aşağıdaki adımları izleyin:

   1. Sinyal **adı** sütununda, **Tetikleyiciler Başarısız** sinyalini bulun ve seçin.

      ![Uyarı oluşturmak için sinyali seçin](./media/monitor-logic-apps/find-and-select-signal.png)

   1. Seçilen sinyal için açılan bilgi bölmesinde, **Uyarı mantığı**altında, örneğin durumunuzu ayarlayın:

   1. **Operatör**için , **'den büyük veya eşit' seçeneğini belirleyin.**

   1. **Toplama türü için,** **Say'ı**seçin.

   1. **Eşik değeri**için `1`, .

   1. **Koşul önizlemesi**altında, durumunuzun doğru göründüğünü onaylayın.

   1. **Değerlendirildi'ye göre,** uyarı kuralını çalıştırmak için aralığı ve sıklığı ayarlayın. **Toplama parçalı (Nokta)** için, verileri gruplandırma için dönemi seçin. **Değerlendirme sıklığı**için, durumu ne sıklıkta denetlemek istediğinizi seçin.

   1. Hazır **olduğunuzda, Bitti'yi**seçin.

   İşte bitmiş koşul:

   ![Uyarı için koşul ayarlama](./media/monitor-logic-apps/set-up-condition-for-alert.png)

   **Oluştur kuralı** sayfası artık oluşturduğunuz durumu ve bu uyarıyı çalıştırmanın maliyetini gösterir.

   !["Kural oluştur" sayfasında yeni uyarı](./media/monitor-logic-apps/finished-alert-condition-cost.png)

1. Uyarınız için bir ad, isteğe bağlı açıklama ve önem düzeyi belirtin. Oluşturma ayarı **açıkken Etkinleştir kuralını** bırakın veya kuralı etkinleştirmeye hazır olana kadar kapatın.

1. İşi **bittiğinde, uyarı kuralı nı oluştur'u**seçin.

> [!TIP]
> Bir uyarıdan bir mantık uygulaması çalıştırmak için, aşağıdaki örnekler gibi görevleri gerçekleştirmenize olanak tanıyan [istek tetikleyicisini](../connectors/connectors-native-reqres.md) iş akışınıza ekleyebilirsiniz:
> 
> * [Slack'e Gönder](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Kısa mesaj gönderme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Kuyruğa ileti ekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitör'ü kullanarak mantık uygulamalarını izleme](../logic-apps/monitor-logic-apps-log-analytics.md)