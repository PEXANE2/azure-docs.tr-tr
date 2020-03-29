---
title: İş akışı hatalarını giderme ve tanılama
description: Azure Mantık Uygulamaları'nda iş akışlarınızdaki sorunları, hataları ve hataları nasıl giderecek ve tanılamayı öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/31/2020
ms.openlocfilehash: 1f83f13564a64a0d9d8a5e0144ca95af6a769d6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905022"
---
# <a name="troubleshoot-and-diagnose-workflow-failures-in-azure-logic-apps"></a>Azure Logic Apps'ta iş akışı hatalarını giderme ve tanılama

Mantık uygulamanız, uygulamanızdaki sorunları tanılamanıza ve hata ayıklamanıza yardımcı olabilecek bilgiler oluşturur. Azure portalı üzerinden iş akışındaki her adımı gözden geçirerek bir mantık uygulaması tanılayabilirsiniz. Veya, çalışma zamanı hata ayıklama için iş akışına bazı adımlar ekleyebilirsiniz.

<a name="check-trigger-history"></a>

## <a name="check-trigger-history"></a>Tetikleyici geçmişini denetleme

Her mantık uygulaması çalışması bir tetikleyici girişimiyle başlar, bu nedenle tetikleyici çalışmıyorsa aşağıdaki adımları izleyin:

1. [Tetikleyici geçmişini denetleyerek tetikleyicinin durumunu kontrol edin.](../logic-apps/monitor-logic-apps.md#review-trigger-history) Tetikleyici girişimi hakkında daha fazla bilgi görüntülemek için, örneğin şu tetikleyici olayı seçin:

   ![Tetikleyici durumunu ve geçmişini görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

1. Beklediğiniz gibi göründüklerini doğrulamak için tetikleyicinin girişlerini kontrol edin. **Girişler bağlantısı**altında, **Giriş bölmesini** gösteren bağlantıyı seçin.

   Tetikleyici girişleri, tetikleyicinin beklediği ve iş akışını başlatmak için gereken verileri içerir. Bu girdilerin gözden geçirilmesi, tetikleyici girişlerin doğru olup olmadığını ve iş akışının devam edilebilmeleri için koşulun karşılanıp karşılanmadığını belirlemenize yardımcı olabilir.

   Örneğin, buradaki `feedUrl` özelliğin yanlış bir RSS besleme değeri vardır:

   ![Hatalar için tetikleyici girişlerini gözden geçirme](./media/logic-apps-diagnosing-failures/review-trigger-inputs-for-errors.png)

1. Beklediğiniz gibi göründüklerini doğrulamak için tetikleyici çıktılarını kontrol edin. **Çıktılar bağlantısı**altında, **Çıktılar** bölmesini gösteren bağlantıyı seçin.

   Tetikleyici çıktılar, tetikleyicinin iş akışınızdaki bir sonraki adıma geçtiği verileri içerir. Bu çıktıları gözden geçirmek, örneğin, iş akışınızdaki bir sonraki adıma aktarılan doğru veya beklenen değerlerin olup olmadığını belirlemenize yardımcı olabilir:

   ![Hatalar için tetikleyici çıktıları gözden geçirme](./media/logic-apps-diagnosing-failures/review-trigger-outputs-for-errors.png)

   > [!TIP]
   > Tanımadığınız herhangi bir içerik bulursanız, Azure Mantık Uygulamaları'nda [farklı içerik türleri](../logic-apps/logic-apps-content-type.md) hakkında daha fazla bilgi edinin.

<a name="check-runs-history"></a>

## <a name="check-runs-history"></a>Çalışır geçmişini denetle

Tetikleyici bir öğe veya olay için her yangın da yanar, Logic Apps altyapısı her öğe veya olay için ayrı bir iş akışı örneği oluşturur ve çalıştırAr. Bir çalıştırma başarısız olursa, iş akışındaki her adımın durumu ve her adım için giriş ve çıktılar da dahil olmak üzere, bu çalıştırma sırasında ne olduğunu gözden geçirmek için aşağıdaki adımları izleyin.

1. Çalışma geçmişini denetleyerek iş akışının çalışma durumunu [denetleyin.](../logic-apps/monitor-logic-apps.md#review-runs-history) Başarısız bir çalıştırma hakkında daha fazla bilgi (durumlarında çalışan tüm adımlar da dahil olmak üzere) görüntülemek için, başarısız çalıştırmayı seçin.

   ![Çalıştırma geçmişini görüntüleyin ve başarısız çalıştırmayı seçin](./media/logic-apps-diagnosing-failures/logic-app-runs-history.png)

1. Çalıştırmadaki tüm adımlar göründükten sonra, ilk başarısız adımı genişletin.

   ![İlk başarısız adımı genişletme](./media/logic-apps-diagnosing-failures/logic-app-run-pane.png)

1. Başarısız adımın girişlerini kontrol edin ve bunların beklediğiniz gibi görünüp görünmediğini onaylayın.

1. Belirli bir çalıştırmadaki her adımın ayrıntılarını gözden geçirin. **Çalışır geçmişi**altında, incelemek istediğiniz çalıştır'ı seçin.

   ![İnceleme geçmişi çalışır](./media/logic-apps-diagnosing-failures/logic-app-runs-history.png)

   ![Bir mantıksal uygulama çalıştırmasının ayrıntılarını görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

1. Belirli bir adımiçin girişleri, çıktıları ve hata iletilerini incelemek için, şeklin genişlemesi ve ayrıntıları göstermesi için bu adımı seçin. Örnek:

   ![Adım ayrıntıları görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Çalışma zamanı hata ayıklamayı gerçekleştirme

Hata ayıklama ile yardımcı olmak için, tetikleyicigözden ve geçmişi çalışır ile birlikte bir mantık uygulaması iş akışı tanılama adımları ekleyebilirsiniz. Örneğin, HTTP isteklerini inceleyebilmeniz ve tam boyut, şekil ve biçimlerini belirleyebilmeniz için [Webhook Tester](https://webhook.site/) hizmetini kullanan adımlar ekleyebilirsiniz.

1. [Webhook Tester](https://webhook.site/) sitesine gidin ve oluşturulan benzersiz URL'yi kopyalayın.

1. Mantık uygulamanızda, bir HTTP POST eylemi ve örneğin bir ifade veya başka bir adım çıktısı gibi test etmek istediğiniz gövde içeriğini ekleyin.

1. URL'nizi Webhook Tester'dan HTTP POST eylemine yapıştırın.

1. Logic Apps altyapısından oluşturulduğunda bir isteğin nasıl oluşturulduğunu gözden geçirmek için mantık uygulamasını çalıştırın ve daha fazla ayrıntı için Webhook Tester sitesini yeniden ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantık uygulamanızı izleyin](../logic-apps/monitor-logic-apps.md)
