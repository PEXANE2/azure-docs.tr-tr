---
title: İş akışlarında sonraki eylemi geciktir
description: Azure Logic Apps eylemleri tamamlanana kadar geciktir veya Delay kullanarak Logic App iş akışlarında sonraki eylemi çalıştırmayı bekleyin
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
tags: connectors
ms.openlocfilehash: 5348ade1ba6eec6cbd360849411b4520cb3c2b19
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74787345"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Azure Logic Apps sonraki eylemi çalıştırmayı geciktir

Mantıksal uygulamanızın bir sonraki eylemi çalıştırmadan önce bir süre beklemesi için, mantıksal uygulamanızın iş akışındaki bir eylemden önce yerleşik **gecikme zamanlama** eylemini ekleyebilirsiniz. Ya da bir sonraki eylemi çalıştırmadan önce belirli bir tarih ve saate kadar beklemek üzere yerleşik gecikme süresi ( **zamanlama** ) eylemini ekleyebilirsiniz. Yerleşik zamanlama eylemleri ve Tetikleyicileri hakkında daha fazla bilgi için bkz. [Azure Logic Apps ile yinelenen otomatik, görev ve iş akışlarını zamanlama ve çalıştırma](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

* **Gecikme**: sonraki eylem çalıştırılmadan önce, saniye, dakika, saat, gün, hafta veya ay gibi belirtilen zaman birimi sayısını bekleyin.

* Şu **kadar geciktir**: sonraki eylem çalıştırılmadan önce belirtilen tarih ve saate kadar bekleyin.

Bu eylemleri kullanmanın bazı örnek yolları aşağıda verilmiştir:

* Bir iş gününü e-posta üzerinden bir durum güncelleştirmesi göndermek için bekleyin.

* Verileri devam ettirmeden ve almadan önce bir HTTP çağrısı bitene kadar iş akışınızı erteleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolabilirsiniz](https://azure.microsoft.com/free/).

* [Logic Apps](../logic-apps/logic-apps-overview.md)hakkında temel bilgi. Bir eylemi kullanabilmeniz için öncelikle mantıksal uygulamanızın bir tetikleyiciyle başlaması gerekir. Bir gecikme eylemi eklemeden önce istediğiniz tetikleyiciyi kullanabilir ve başka eylemler ekleyebilirsiniz. Bu konu, bir Office 365 Outlook tetikleyicisi kullanır. Logic Apps 'e yeni başladıysanız, [ilk mantıksal uygulamanızı oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Gecikme eylemini ekleyin

1. Mantıksal uygulama Tasarımcısı ' nda, gecikme eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Adımlar arasındaki gecikme eylemini eklemek için, adımları bağlayan okun üzerine işaretçiyi taşıyın. Görüntülenen artı işaretini (+) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak "Delay" yazın. Eylemler listesinden şu eylemi seçin: **gecikme**

   !["Gecikme" eylemi Ekle](./media/connectors-native-delay/add-delay-action.png)

1. Sonraki eylem çalıştırılmadan önce beklenecek süreyi belirtin.

   ![Gecikme için zaman miktarı ayarla](./media/connectors-native-delay/delay-time-intervals.png)

   | Özellik | JSON adı | Gereklidir | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | Sayı | count | Yes | Tamsayı | Geciktirime zaman birimi sayısı |
   | Birim | birim | Yes | Dize | Zaman birimi; örneğin: `Second`, `Minute`, `Hour`, `Day`, `Week`veya `Month` |
   ||||||

1. İş akışınızda çalıştırmak istediğiniz herhangi bir eylemi ekleyin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>Gecikme-Until eylemini ekleyin

1. Mantıksal uygulama Tasarımcısı ' nda, gecikme eylemini eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Adımlar arasındaki gecikme eylemini eklemek için, adımları bağlayan okun üzerine işaretçiyi taşıyın. Görüntülenen artı işaretini (+) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak "Delay" yazın. Eylemler listesinden şu eylemi seçin: **gecikme tarihine kadar geciktir**

   !["Gecikme süresi" eylemini Ekle](./media/connectors-native-delay/add-delay-until-action.png)

1. İş akışını ne zaman sürdürüleceği için bitiş tarihini ve saatini belirtin.

   ![Gecikmeye bitiş için zaman damgasını belirtin](./media/connectors-native-delay/delay-until-timestamp.png)

   | Özellik | JSON adı | Gereklidir | Tür | Açıklama |
   |----------|-----------|----------|------|-------------|
   | Zaman damgası | timestamp | Yes | Dize | Şu biçimi kullanarak iş akışının sürdürülürken bitiş tarihi ve saati: <p>YYYY-MM-DDThh: mm: ssZ <p>Örneğin, 18 Eylül 2017, 2:00 PM 'de istiyorsanız, "2017-09-18T14:00:00Z" belirtin. <p>**Note:** Bu zaman biçimi UTC [8601 tarih saat belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) [UTC Tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)izlemelidir, ancak [UTC boşluğu](https://en.wikipedia.org/wiki/UTC_offset)olmadan gelmelidir. Saat dilimi olmadan sonunda boşluk olmadan "Z" harfini eklemeniz gerekir. Bu "Z", eşdeğer [nadeniz saati](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. |
   ||||||

1. İş akışınızda çalıştırmak istediğiniz herhangi bir eylemi ekleyin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

* [Yineleme tetikleyicisiyle yinelenen görevleri ve iş akışlarını oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-recurrence.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)