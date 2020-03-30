---
title: Yinelenen görevleri ve iş akışlarını zamanlama
description: Azure Mantık Uygulamaları'nda Yinelenen Otomatik görevleri ve iş akışlarını Yineleme tetikleyicisiyle zamanlama ve çalıştırma
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: a9c167c5767a4156147e13a1e4ae21162e506474
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445868"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-recurrence-trigger-in-azure-logic-apps"></a>Azure Mantık Uygulamalarında Yinelenen Tetikleme ile yinelenen görevler ve iş akışları oluşturun, zamanlama ve çalıştırın

Görevleri, işlemleri veya işleri belirli bir zamanlamada düzenli olarak çalıştırmak için, yerleşik **Yineleme - Zamanlama** tetikleyicisi ile mantık uygulaması iş akışınızı başlatabilirsiniz. İş akışını başlatmak için bir tarih ve saatin yanı sıra bir saat dilimi ve bu iş akışını yinelemek için bir yineleme ayarlayabilirsiniz. Yinelemeler herhangi bir nedenle kaçırılırsa, bu tetikleyici bir sonraki zamanlanan aralıkta yinelenmeye devam eder. Yerleşik Zamanlama tetikleyicileri ve eylemleri hakkında daha fazla bilgi için Zamanlama'ya bakın [ve Azure Mantık Uygulamaları ile yinelenen otomatik, görevleri ve iş akışlarını çalıştırın.](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)

Bu tetikleyicinin daha gelişmiş yinelemeler ve karmaşık zamanlamalarla birlikte desteklediği bazı desenler aşağıda veda eder:

* Hemen çalıştırın ve her *n* saniye, dakika, saat, gün, hafta veya ay sayısını tekrarlayın.

* Belirli bir tarih ve saatte başlayın, ardından her *n* saniye, dakika, saat, gün, hafta veya ay sayısını çalıştırın ve yineleyin.

* Örneğin, 08:00 ve 17:00'de her gün bir veya daha fazla kez çalıştırın ve tekrarlayın.

* Her hafta çalıştırın ve tekrarlayın, ancak yalnızca Cumartesi ve Pazar gibi belirli günler için.

* Her hafta çalıştırın ve tekrarlayın, ancak pazartesiden cumaya saat 08:00 ve 17:00 gibi belirli gün ve saatlerde çalıştırın ve tekrarlayın.

Bu tetikleyici ile Kayar Pencere tetikleyicisi arasındaki farklar için veya yinelenen iş akışlarını zamanlama hakkında daha fazla bilgi için Zamanlama'ya bakın [ve Azure Mantık Uygulamaları ile yinelenen otomatik görevleri, işlemleri ve iş akışlarını çalıştırın.](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)

> [!TIP]
> Mantık uygulamanızı tetiklemek ve gelecekte yalnızca bir kez çalıştırmak istiyorsanız, [işleri yalnızca bir kez çalıştır'a](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantık uygulamaları](../logic-apps/logic-apps-overview.md)hakkında temel bilgi . Mantık uygulamalarında yeniyseniz, [ilk mantık uygulamanızı nasıl oluşturabileceğinizi](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

## <a name="add-recurrence-trigger"></a>Yineleme tetikleyicisi ekleme

1. [Azure portalında](https://portal.azure.com)oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantık Uygulama Tasarımcısı göründükten sonra, arama `recurrence` kutusuna filtreniz olarak girin. Tetikleyiciler listesinden, mantık uygulaması iş akışınızın ilk adımı olarak bu tetikleyiciyi seçin: **Yineleme**

   !["Yineleme" tetikleyicisi'ni seçin](./media/connectors-native-recurrence/add-recurrence-trigger.png)

1. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte, bu özellikleri iş akışınızı her hafta çalışacak şekilde ayarlayın.

   ![Aralığı ve frekansı ayarlama](./media/connectors-native-recurrence/recurrence-trigger-details.png)

   | Özellik | JSON adı | Gerekli | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | **Interval** | `interval` | Evet | Tamsayı | İş akışının sıklığına bağlı olarak ne sıklıkta çalıştığını açıklayan pozitif bir tamsayı. Minimum ve maksimum aralıklar şunlardır: <p>- Ay: 1-16 ay </br>- Gün: 1-500 gün </br>- Saat: 1-12.000 saat </br>- Dakika: 1-72.000 dakika </br>- İkinci: 1-9,999,999 saniye<p>Örneğin, aralık 6 ise ve sıklık "Ay" ise, yineleme her 6 ayda bir olur. |
   | **Frequency** | `frequency` | Evet | Dize | Yineleme için zaman birimi: **İkinci**, **Dakika**, **Saat**, **Gün**, **Hafta**, veya **Ay** |
   ||||||

   > [!IMPORTANT]
   > Yinelemeler gelişmiş zamanlama seçenekleri belirtmediğinde, gelecekteki yinelemeler son çalışma süresini temel alar.
   > Bu yinelemelerin başlangıç süreleri, depolama aramaları sırasında gecikme süresi gibi etkenler nedeniyle sürüklenebilir. Mantık uygulamanızın bir yinelemeyi kaçırmadığından emin olmak için, özellikle sıklık birkaç gün veya daha uzun olduğunda aşağıdaki seçeneklerden birini kullanın:
   > 
   > * Yineleme için bir başlangıç zamanı sağlayın.
   > 
   > * **Bu saatlerde** at ve **bu dakika** özelliklerini kullanarak yinelemenin ne zaman çalıştırılması için saat ve dakika belirtin.
   > 
   > * Yinelenen [tetikleme yerine Sürgülü Pencere tetikleyicisini](../connectors/connectors-native-sliding-window.md)kullanın.

1. Gelişmiş zamanlama seçeneklerini ayarlamak için **yeni parametre** ekle listesini açın. Seçtiğiniz seçenekler seçimden sonra tetikleyicide görünür.

   ![Gelişmiş zamanlama seçenekleri](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   | Özellik | JSON adı | Gerekli | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | **Saat dilimi** | `timeZone` | Hayır | Dize | Bu tetikleyici [UTC ofset](https://en.wikipedia.org/wiki/UTC_offset)kabul etmediğinden, yalnızca bir başlangıç saati belirttiğiniz zaman geçerlidir. Uygulamak istediğiniz saat dilimini seçin. |
   | **Başlangıç saati** | `startTime` | Hayır | Dize | Bu biçimde bir başlangıç tarihi ve saati sağlayın: <p>Saat dilimi seçerseniz YYYY-MM-DDThh:mm:ss <p>-veya- <p>Saat dilimi seçmezseniz YYYY-MM-DDThh:mm:ssZ <p>Örneğin, 18 Eylül 2017 saat 14:00'te isterseniz, "2017-09-18T14:00:00" seçeneğini belirleyin ve Pasifik Standart Saati gibi bir saat dilimi seçin. Veya saat dilimi olmadan "2017-09-18T14:00:00Z" olarak belirtiniz. <p>**Not:** Bu başlangıç süresi gelecekte en fazla 49 yıl vardır ve [UTC tarih saati biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) [ISO 8601 tarih saati belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) takip etmelidir, ancak [UTC mahsup](https://en.wikipedia.org/wiki/UTC_offset)olmadan. Bir saat dilimi seçmezseniz, herhangi bir boşluk olmadan sonuna "Z" harfini eklemeniz gerekir. Bu "Z" eşdeğer [denizcilik zamanı](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. <p>Basit zamanlamalar için başlangıç zamanı ilk olaydır, karmaşık zamanlamalar için tetikleyici başlangıç saatinden daha erken ateş etmez. [*Başlangıç tarihini ve saatini kullanma yolları nelerdir?*](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   | **Şu günlerde** | `weekDays` | Hayır | Dize veya dize dizisi | "Hafta"yı seçerseniz, iş akışını çalıştırmak istediğinizde bir veya daha fazla gün seçebilirsiniz: **Pazartesi,** **Salı,** **Çarşamba,** **Perşembe,** **Cuma,** **Cumartesi**ve **Pazar** |
   | **Şu saatlerde** | `hours` | Hayır | İnteger veya sonda dizisi | "Gün" veya "Hafta" seçeneğini belirlerseniz, iş akışını çalıştırmak istediğiniz günün saatleri olarak 0'dan 23'e kadar bir veya daha fazla arayıcı seçebilirsiniz. <p><p>Örneğin, 10, 12 ve 14 belirtirseniz, günün saatleri için 10, 12 PM ve 14 alırsınız, ancak günün dakikaları yinelemenin ne zaman başladığına göre hesaplanır. Günün dakikalarını ayarlamak için, **at bu dakika** özelliğinin değerini belirtin. |
   | **Şu dakikalarda** | `minutes` | Hayır | İnteger veya sonda dizisi | "Gün" veya "Hafta" seçeneğini belirlerseniz, iş akışını çalıştırmak istediğiniz saatin dakikası olarak 0'dan 59'a kadar bir veya daha fazla tümseci seçebilirsiniz. <p>Örneğin, dakika işareti olarak "30" belirtebilirsiniz ve günün saatleri için önceki örneği kullanarak, 10:30, 12:30 ve 14:30 olsun. |
   |||||

   Örneğin, bugünün 4 Eylül 2017 Pazartesi olduğunu varsayalım. Aşağıdaki Yineleme tetikleyicisi, 18 Eylül 2017 Pazartesi günü SAAT 08:00 PST'de başlayacak tarih ve saatten *önce* ateşlenmez. Ancak, yineleme zamanlaması yalnızca Pazartesi günleri 10:30, 12:30 ve 14:30'da ayarlanır. Yani ilk kez tetik leme ve bir mantık uygulaması iş akışı örneği oluşturur 10:30. Başlangıç saatinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için aşağıdaki [başlangıç saati örneklerine](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time)bakın.

   Gelecek koşular aynı gün 12:30 ve 14:30'da gerçekleşir. Her yineleme kendi iş akışı örneğini oluşturur. Bundan sonra, tüm program önümüzdeki Pazartesi tekrar tekrarlar. [*Diğer bazı örnek olaylar nelerdir?*](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#example-recurrences)

   ![Gelişmiş zamanlama örneği](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > Tetikleyici, yalnızca sıklık olarak "Gün" veya "Hafta" seçeneğini seçtiğinizde belirttiğiniz yineleme için bir önizleme gösterir.

1. Şimdi kalan iş akışınızı diğer eylemlerle oluşturun. Ekleyebileceğiniz diğer işlemler [için Azure Mantık Uygulamaları Için Bağlayıcılar'a](../connectors/apis-list.md)bakın.

## <a name="workflow-definition---recurrence"></a>İş akışı tanımı - Yineleme

Mantık uygulamanızın JSON kullanan temel iş akışı tanımında, seçtiğiniz seçeneklerle [Yineleme tetikleyici tanımını](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger) görüntüleyebilirsiniz. Bu tanımı görüntülemek için, tasarımcı araç çubuğunda **Kod görünümü'ni**seçin. Tasarımcıdönmek için, tasarımcı araç çubuğu, **Designer**seçin.

Bu örnek, yinelenen tetikleyici tanımının temel iş akışı tanımında nasıl görünebileceğini gösterir:

``` json
"triggers": {
   "Recurrence": {
      "type": "Recurrence",
      "recurrence": {
         "frequency": "Week",
         "interval": 1,
         "schedule": {
            "hours": [
               10,
               12,
               14
            ],
            "minutes": [
               30
            ],
            "weekDays": [
               "Monday"
            ]
         },
         "startTime": "2017-09-07T14:00:00Z",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Gecikme eylemleriyle iş akışlarını duraklatma](../connectors/connectors-native-delay.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)
