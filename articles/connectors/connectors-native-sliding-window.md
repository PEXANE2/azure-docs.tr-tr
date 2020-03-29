---
title: Bitişik verileri işlemek için görevleri zamanlama
description: Azure Mantık Uygulamaları'nda kayan pencereleri kullanarak bitişik verileri işleyen yinelenen görevler oluşturun ve çalıştırın
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: ab4bf802772c95d8c48a8cdba48def05e8a2761b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74786920"
---
# <a name="schedule-and-run-tasks-for-contiguous-data-by-using-the-sliding-window-trigger-in-azure-logic-apps"></a>Azure Mantık Uygulamaları'nda Kayar Pencere tetikleyicisini kullanarak bitişik veriler için görevleri zamanlama ve çalıştırma

Verileri bitişik parçalar halinde işlemesi gereken görevleri, işlemleri veya işleri düzenli olarak çalıştırmak **için, Kaydırma Penceresi** tetikleyicisi ile mantık uygulaması iş akışınızı başlatabilirsiniz. İş akışını başlatmak için bir tarih ve saatin yanı sıra bir saat dilimi ve bu iş akışını yinelemek için bir yineleme ayarlayabilirsiniz. Nüksler her ne sebeple olursa olsun kaçırılırsa, bu tetikleyici bu cevapsız yinelemeleri işler. Örneğin, veritabanınız ve yedekleme depolama alanınız arasında veri eşitlerken, verilerin boşluklara maruz kalmadan eşitlenabilmesi için Kayan Pencere tetikleyicisini kullanın. Yerleşik Zamanlama tetikleyicileri ve eylemleri hakkında daha fazla bilgi için Zamanlama'ya bakın [ve Azure Mantık Uygulamaları ile yinelenen otomatik, görevleri ve iş akışlarını çalıştırın.](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)

Bu tetikleyicinin desteklediği bazı desenler şunlardır:

* Hemen çalıştırın ve her *n* saniye, dakika veya saat sayısını tekrarlayın.

* Belirli bir tarih ve saatte başlayın, ardından her *n* saniye, dakika veya saat sayısını çalıştırın ve yineleyin. Bu tetikleyici yle, geçmişte tüm geçmiş yinelemeleri çalıştıran bir başlangıç saati belirtebilirsiniz.

* Çalıştırmadan önce her yinelemeyi belirli bir süre geciktirin.

Bu tetikleyici ile Yinelenen tetikleyici arasındaki farklar için veya yinelenen iş akışlarını zamanlama hakkında daha fazla bilgi için [Zamanlama'ya bakın ve Azure Mantık Uygulamaları ile yinelenen otomatik görevleri, işlemleri ve iş akışlarını çalıştırın.](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)

> [!TIP]
> Mantık uygulamanızı tetiklemek ve gelecekte yalnızca bir kez çalıştırmak istiyorsanız, [işleri yalnızca bir kez çalıştır'a](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz [yoksa, ücretsiz bir Azure hesabına](https://azure.microsoft.com/free/)kaydolabilirsiniz.

* [Mantık uygulamaları](../logic-apps/logic-apps-overview.md)hakkında temel bilgi . Mantık uygulamalarında yeniyseniz, [ilk mantık uygulamanızı nasıl oluşturabileceğinizi](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

## <a name="add-sliding-window-trigger"></a>Kayan Pencere tetikleyicisi ekle

1. [Azure portalında](https://portal.azure.com)oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantık Uygulama Tasarımcısı göründükten sonra, arama kutusuna filtreniz olarak "sürgülü pencere" girin. Tetikleyiciler listesinden, mantık uygulaması iş akışınızın ilk adımı olarak bu tetikleyiciyi seçin: **Kayan Pencere**

   !["Kayan Pencere" tetikleyicisi](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte, bu özellikleri iş akışınızı her hafta çalışacak şekilde ayarlayın.

   ![Aralığı ve frekansı ayarlama](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Özellik | Gerekli | JSON adı | Tür | Açıklama |
   |----------|----------|-----------|------|-------------|
   | **Interval** | Evet | interval | Tamsayı | İş akışının sıklığına bağlı olarak ne sıklıkta çalıştığını açıklayan pozitif bir tamsayı. Minimum ve maksimum aralıklar şunlardır: <p>- Saat: 1-12.000 saat </br>- Dakika: 1-72.000 dakika </br>- İkinci: 1-9,999,999 saniye<p>Örneğin, aralık 6 ise ve sıklık "Saat" ise, yineleme her 6 saatte bir olur. |
   | **Frequency** | Evet | frequency | Dize | Yineleme için zaman birimi: **İkinci**, **Dakika**, veya **Saat** |
   ||||||

   ![Gelişmiş yineleme seçenekleri](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Daha fazla yineleme seçeneği için **yeni parametre** ekle listesini açın. 
   Seçtiğiniz seçenekler seçimden sonra tetikleyicide görünür.

   | Özellik | Gerekli | JSON adı | Tür | Açıklama |
   |----------|----------|-----------|------|-------------|
   | **Gecikme** | Hayır | Gecikme | Dize | [ISO 8601 tarih saati belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Durations) kullanarak her yinelemeyi geciktirme süresi |
   | **Saat dilimi** | Hayır | timeZone | Dize | Bu tetikleyici [UTC ofset](https://en.wikipedia.org/wiki/UTC_offset)kabul etmediğinden, yalnızca bir başlangıç saati belirttiğiniz zaman geçerlidir. Uygulamak istediğiniz saat dilimini seçin. |
   | **Başlangıç saati** | Hayır | startTime | Dize | Bu biçimde bir başlangıç tarihi ve saati sağlayın: <p>Saat dilimi seçerseniz YYYY-MM-DDThh:mm:ss <p>-veya- <p>Saat dilimi seçmezseniz YYYY-MM-DDThh:mm:ssZ <p>Örneğin, 18 Eylül 2017 saat 14:00'te isterseniz, "2017-09-18T14:00:00" seçeneğini belirleyin ve Pasifik Standart Saati gibi bir saat dilimi seçin. Veya saat dilimi olmadan "2017-09-18T14:00:00Z" olarak belirtiniz. <p>**Not:** Bu başlangıç [saati, UTC tarih saati biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) [iso 8601 tarih saati belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) takip etmelidir, ancak [UTC mahsup](https://en.wikipedia.org/wiki/UTC_offset)olmadan. Bir saat dilimi seçmezseniz, herhangi bir boşluk olmadan sonuna "Z" harfini eklemeniz gerekir. Bu "Z" eşdeğer [denizcilik zamanı](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. <p>Basit zamanlamalar için başlangıç zamanı ilk olaydır, gelişmiş yinelemeler için tetikleyici başlangıç saatinden daha erken ateş etmez. [*Başlangıç tarihini ve saatini kullanma yolları nelerdir?*](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Şimdi kalan iş akışınızı diğer eylemlerle oluşturun. Ekleyebileceğiniz diğer işlemler [için Azure Mantık Uygulamaları Için Bağlayıcılar'a](../connectors/apis-list.md)bakın.

## <a name="workflow-definition---sliding-window"></a>İş akışı tanımı - Kayan Pencere

Mantık uygulamanızın JSON kullanan temel iş akışı tanımında, Seçtiğiniz seçeneklerle Sürgülü Pencere tetikleyici tanımını görüntüleyebilirsiniz. Bu tanımı görüntülemek için, tasarımcı araç çubuğunda **Kod görünümü'ni**seçin. Tasarımcıdönmek için, tasarımcı araç çubuğu, **Designer**seçin.

Bu örnek, sürgülü pencere tetikleyici tanımının, her yineleme için gecikmenin saatlik yineleme için beş saniye olduğu temel bir iş akışı tanımında nasıl görünebileceğini gösterir:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [İş akışlarında sonraki eylemi geciktirme](../connectors/connectors-native-delay.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)