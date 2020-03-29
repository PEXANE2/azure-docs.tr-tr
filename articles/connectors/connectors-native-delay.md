---
title: İş akışlarında sonraki eylemi geciktirme
description: Azure Mantık Uygulamaları'ndaki eylemlere Kadar Gecikme veya Gecikme'yi kullanarak mantık uygulaması iş akışlarında bir sonraki eylemi çalıştırmayı bekleyin
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
tags: connectors
ms.openlocfilehash: 5348ade1ba6eec6cbd360849411b4520cb3c2b19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74787345"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Azure Logic Apps'ta bir sonraki eylemi çalıştırmayı geciktirme

Mantık uygulamanızın bir sonraki eylemi çalıştırmadan önce bir süre beklemesini sağlamak için, yerleşik **Gecikme - Zamanlama** eylemini mantık uygulamanızın iş akışındaki bir eylemden önce ekleyebilirsiniz. Veya, bir sonraki eylemi çalıştırmadan önce belirli bir tarih ve saate kadar beklemek üzere yerleşik **Gecikme kadar - Zamanlama** eylemi ekleyebilirsiniz. Yerleşik Zamanlama eylemleri ve tetikleyicileri hakkında daha fazla bilgi için Zamanlama'ya bakın [ve Azure Mantık Uygulamaları ile yinelenen otomatik, görevleri ve iş akışlarını çalıştırın.](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md)

* **Gecikme**: Bir sonraki eylem çalışmadan önce saniye, dakika, saat, gün, hafta veya ay gibi belirtilen zaman birimi sayısını bekleyin.

* **Gecikme :** Bir sonraki eylem çalışmadan önce belirtilen tarih ve saati bekleyin.

Aşağıda, bu eylemleri kullanmanın bazı örnek yolları verilmiştir:

* E-posta üzerinden durum güncelleştirmesi göndermek için hafta içi bir güne kadar bekleyin.

* Verileri devam ettirmeden ve almadan önce bir HTTP çağrısı bitene kadar iş akışınızı geciktirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz [yoksa, ücretsiz bir Azure hesabına](https://azure.microsoft.com/free/)kaydolabilirsiniz.

* [Mantık uygulamaları](../logic-apps/logic-apps-overview.md)hakkında temel bilgi . Bir eylemi kullanamadan önce mantık uygulamanızın önce bir tetikleyiciyle başlaması gerekir. Bir erteleme eylemi eklemeden önce istediğiniz tetikleyiciyi kullanabilir ve başka eylemler ekleyebilirsiniz. Bu konu bir Office 365 Outlook tetikleyicisi kullanır. Mantık uygulamalarında yeniyseniz, [ilk mantık uygulamanızı nasıl oluşturabileceğinizi](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Gecikme eylemini ekleme

1. Mantık Uygulama Tasarımcısı'nda, gecikme eylemini eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Adımlar arasında gecikme eylemi eklemek için işaretçiyi adımları bağlayan okun üzerine taşıyın. Görünen artı işaretini (+) seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "gecikme" girin. Eylemler listesinden şu eylemi seçin: **Gecikme**

   !["Gecikme" eylemi ekleme](./media/connectors-native-delay/add-delay-action.png)

1. Bir sonraki eylem çalışmadan önce beklemek için gereken süreyi belirtin.

   ![Gecikme için zaman ayarlama](./media/connectors-native-delay/delay-time-intervals.png)

   | Özellik | JSON adı | Gerekli | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | Sayı | count | Evet | Tamsayı | Geciktirecek zaman birimlerinin sayısı |
   | Birim | unit | Evet | Dize | Zaman `Second`birimi, örneğin: , `Minute` `Hour`, `Day` `Week`, , veya`Month` |
   ||||||

1. İş akışınızda çalıştırmak istediğiniz diğer eylemleri ekleyin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>Gecikme ye kadar eylemi ekleme

1. Mantık Uygulama Tasarımcısı'nda, gecikme eylemini eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Adımlar arasında gecikme eylemi eklemek için işaretçiyi adımları bağlayan okun üzerine taşıyın. Görünen artı işaretini (+) seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "gecikme" girin. Eylemler listesinden şu eylemi **seçin:**

   !["Gecikmeye kadar" eylemi ekleme](./media/connectors-native-delay/add-delay-until-action.png)

1. İş akışını ne zaman devam ettirmek istediğinizi bitiş tarihi ve saati sağlayın.

   ![Gecikmenin ne zaman sona erdirileceye inecek zaman damgası belirtin](./media/connectors-native-delay/delay-until-timestamp.png)

   | Özellik | JSON adı | Gerekli | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | Zaman damgası | timestamp | Evet | Dize | Bu biçimi kullanarak iş akışını devam ettireme bitiş tarihi ve saati: <p>YYYY-MM-DDThh:mm:ssZ <p>Örneğin, 18 Eylül 2017 saat 14:00'te "2017-09-18T14:00:00Z" deyebilirsiniz. <p>**Not:** Bu zaman biçimi [UTC tarih saati biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) [ISO 8601 tarih saati belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) takip etmelidir, ancak [UTC ofset](https://en.wikipedia.org/wiki/UTC_offset)olmadan. Bir saat dilimi olmadan, herhangi bir boşluk olmadan sonuna "Z" harfi eklemeniz gerekir. Bu "Z" eşdeğer [denizcilik zamanı](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. |
   ||||||

1. İş akışınızda çalıştırmak istediğiniz diğer eylemleri ekleyin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

* [Yinelenen görevler ve iş akışları yineleme tetikleyicisi ile oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-recurrence.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)