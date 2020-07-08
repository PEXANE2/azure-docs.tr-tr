---
title: Azure Logic Apps RSS akışlarına bağlanma
description: Azure Logic Apps kullanarak RSS akışlarını izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 1e95b7bfce3d57dfe579a5622c3c13fac2b6af68
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83834824"
---
# <a name="manage-rss-feeds-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak RSS akışlarını yönetme

Azure Logic Apps ve RSS Bağlayıcısı ile herhangi bir RSS akışı için otomatik görevler ve iş akışları oluşturabilirsiniz, örneğin:

* RSS akış öğeleri yayımlandığında izleyin.
* Tüm RSS akışı öğelerini listeleyin.

Aslında basit dağıtım olarak da bilinen RSS (zengin site özeti), Web dağıtımı için popüler bir biçimdir ve blog gönderileri ve haber başlıkları gibi sık güncellenen içerikleri yayımlamak için kullanılır. Birçok içerik yayımcısı, kullanıcıların bu içeriğe abone olabilmesi için bir RSS akışı sağlar. 

RSS akışından yanıt alan bir RSS tetikleyicisi kullanabilir ve çıktıyı diğer eylemler için kullanılabilir hale getirebilirsiniz. RSS akışı ile bir görev gerçekleştirmek için mantıksal uygulamalarınızda RSS eylemini kullanabilirsiniz. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* RSS akışı URL 'SI

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Bir RSS akışına erişmek istediğiniz mantıksal uygulama. Bir RSS tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir RSS eylemi kullanmak için, mantıksal uygulamanızı başka bir tetikleyici ile başlatın, örneğin **yineleme** tetikleyicisi.

## <a name="connect-to-an-rss-feed"></a>RSS akışına bağlanma

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Bir yol seçin: 

   * Boş Logic Apps için, arama kutusuna filtreniz olarak "RSS" yazın. Tetikleyiciler listesinde istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Mevcut Logic Apps için, eylem eklemek istediğiniz adım altında **yeni adım**' ı seçin. Arama kutusuna "rss" girin. Eylemler listesi altında istediğiniz eylemi seçin.

1. Seçtiğiniz tetikleyici veya eyleminiz için gerekli ayrıntıları sağlayın ve mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/rss/)gözden geçirin.

## <a name="get-support"></a>Destek alma

* Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](https://docs.microsoft.com/answers/topics/azure-logic-apps.html)ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin