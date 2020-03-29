---
title: Azure Logic Apps'tan RSS akışlarına bağlanma
description: Azure Logic Apps'ı kullanarak RSS akışlarını izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 3f1e092c2ff325cdcbc32c617af316d6fbe6dd74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789350"
---
# <a name="manage-rss-feeds-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak RSS akışlarını yönetme

Azure Logic Apps ve RSS bağlayıcısı ile, örneğin aşağıdakiler gibi rss akışları için otomatik görevler ve iş akışları oluşturabilirsiniz:

* RSS besleme öğelerinin ne zaman yayınlanıncayı izleyin.
* Tüm RSS besleme öğelerini listeleyin.

RSS (Zengin Site Özeti), ayrıca Really Simple Syndication olarak da adlandırılır, web sendikasyon için popüler bir biçimdir ve blog gönderileri ve haber başlıkları gibi sık güncellenen içerikleri yayınlamak için kullanılır. Birçok içerik yayımcısı, kullanıcıların bu içeriğe abone olabilmesi için bir RSS akışı sağlar. 

RsS akışından yanıt alan ve çıktıyı diğer eylemler için kullanılabilir kılan bir RSS tetikleyicisi kullanabilirsiniz. RSS akışıyla bir görevi gerçekleştirmek için mantık uygulamalarınızda bir RSS eylemi kullanabilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* RSS akışı url'si

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* RSS akışına erişmek istediğiniz mantık uygulaması. RSS tetikleyicisiyle başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bir RSS eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-an-rss-feed"></a>RSS akışına bağlanma

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "rss" girin. Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Varolan mantık uygulamaları için, eylem eklemek istediğiniz adımAltında **Yeni adımı**seçin. Arama kutusuna "rss" girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/rss/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin