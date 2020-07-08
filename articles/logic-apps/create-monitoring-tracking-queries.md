---
title: Azure Izleyici günlüklerinde Logic Apps için sorguları görüntüleme ve oluşturma
description: Azure Logic Apps için Azure Izleyici günlüklerinde sorgular görüntüleyin ve oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 46989ed2468469443d4e91a1834bc20b12c25a1e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76908078"
---
# <a name="view-and-create-queries-for-monitoring-and-tracking-in-azure-monitor-logs-for-azure-logic-apps"></a>Azure Logic Apps için Azure Izleyici günlüklerinde izleme ve izleme sorguları görüntüleyin ve oluşturun

[Azure izleyici günlüklerinden](../log-analytics/log-analytics-overview.md) sonuçları üreten temel sorguları görüntüleyebilir ve belirli ölçütleriniz temelinde sonuçları filtreleyen sorgular oluşturabilirsiniz. Örneğin, belirli bir değişim denetim numarasına göre iletileri bulabilirsiniz. Sorgular, farklı sonuçları görüntülemek istiyorsanız düzenleyebileceğiniz [kusto sorgu dilini](https://aka.ms/LogAnalyticsLanguageReference)kullanır. Daha fazla bilgi için bkz. [Azure izleyici günlük sorguları](../azure-monitor/log-query/query-language.md).

## <a name="prerequisites"></a>Ön koşullar

* Log Analytics çalışma alanı. Log Analytics çalışma alanınız yoksa, [Log Analytics çalışma alanı oluşturmayı](../azure-monitor/learn/quick-create-workspace.md)öğrenin.

* Azure Izleyici günlüğe kaydetme ile ayarlanan ve bu bilgileri bir Log Analytics çalışma alanına Gönderen bir mantıksal uygulama. [Mantıksal uygulamanız Için Azure izleyici günlüklerini ayarlamayı](../logic-apps/monitor-logic-apps.md)öğrenin.

* Tümleştirme hesabı kullanıyorsanız, bu bilgileri bir Log Analytics çalışma alanına göndermek için Azure Izleyici günlüğü ile hesabı ayarladığınızdan emin olun. [Tümleştirme hesabınız Için Azure izleyici günlük kaydını ayarlamayı](../logic-apps/monitor-b2b-messages-log-analytics.md)öğrenin.

## <a name="view-queries-behind-results"></a>Sonuçların arkasında bulunan sorguları görüntüle

Çalışma alanı özetinizdeki sonuçları üreten sorguyu görüntülemek veya düzenlemek için aşağıdaki adımları izleyin:

1. Herhangi bir sonuç sayfasında, alt kısımdaki **Tümünü göster**' i seçin.

   ![Tüm sonuçları görüntüle](./media/create-monitoring-tracking-queries/logic-app-see-all.png)

   Günlükler sayfası açılır ve önceki sonuçlar sayfasının arkasındaki sorguyu gösterir.

   ![Günlükler sayfası-sorgu görünümü](./media/create-monitoring-tracking-queries/view-query-behind-results.png)

1. **Günlükler** sayfasında, şu seçenekleri belirleyebilirsiniz:

   * Sorgu sonuçlarını tablo olarak görüntülemek için sorgu Düzenleyicisi altında **tablo**' yı seçin.

   * Sorguyu değiştirmek için sorgu dizesini güncelleştirin ve sonuçları tabloda görüntülemek için **Çalıştır** ' ı seçin.

## <a name="create-your-own-query"></a>Kendi sorgunuzu oluşturun

Belirli özellikleri veya değerleri temel alarak sonuçları bulmak veya filtrelemek için, boş bir sorgudan başlatarak veya var olan bir sorguyu kullanarak kendi sorgunuzu oluşturabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgularını kullanmaya başlama](../azure-monitor/log-query/get-started-queries.md).

1. [Azure Portal](https://portal.azure.com), Log Analytics çalışma alanınızı bulun ve seçin.

1. Çalışma alanı menünüzde **genel**altında **Günlükler**' i seçin.

1. Boş bir sorgudan veya var olan mevcut sorgulardan başlayın.

   * Mevcut sorguların mevcut olup olmadığını denetlemek için sorgu araç çubuğunda, **Samples queries**  >  Önceki sorgu çalıştırmalarının sorgularını gösteren örnek sorgu**geçmişi**' ni seçin veya önceden oluşturulmuş sorguları gösteren **sorgu Gezgini**' ni seçin.

     Örneğin, Logic Apps B2B çözümü şu önceden oluşturulmuş sorguları sağlar:

     !["Logic Apps B2B" çözümü önceden oluşturulmuş sorgularla başlayın](./media/create-monitoring-tracking-queries/b2b-prebuilt-queries.png)

   * Boş bir sorgudan başlamak için sorgu Düzenleyicisi 'nde sorgunuz için [kusto sorgu dilini](../azure-monitor/log-query/query-language.md) yazmaya başlayın.

     ![Boş sorguyla başla](./media/create-monitoring-tracking-queries/create-query-from-blank.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel izleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)