---
title: Azure Logic Apps'tan Salesforce'a bağlanın
description: Azure Mantık Uygulamalarını kullanarak Salesforce kayıtlarını ve işlerini izleyen, oluşturan ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 000f4381ef2a7c0a2099a021b991087725ff2070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789299"
---
# <a name="monitor-create-and-manage-salesforce-resources-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak Salesforce kaynaklarını izleyin, oluşturun ve yönetin

Azure Logic Apps ve Salesforce bağlayıcısı ile Salesforce kaynaklarınız için örneğin kayıtlar, işler ve nesneler gibi otomatik görevler ve iş akışları oluşturabilirsiniz:

* Kayıtların oluşturulduğunu veya değiştirilmelerini izleyin. 
* Ekleme, güncelleştirme ve silme eylemleri de dahil olmak üzere iş ve kayıtları oluşturun, alın ve yönetin.

Salesforce'tan yanıt almak ve çıktıyı diğer eylemler için kullanılabilir hale getirmek için Salesforce tetikleyicilerini kullanabilirsiniz. Salesforce kaynaklarıyla görevleri gerçekleştirmek için mantık uygulamalarınızdaki eylemleri kullanabilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Salesforce [hesabı](https://salesforce.com/)

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Salesforce hesabınıza erişmek istediğiniz mantık uygulaması. Salesforce tetikleyicisiyle başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Salesforce eylemini kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-salesforce"></a>Salesforce'a bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "salesforce"u girin. 
   Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Varolan mantık uygulamaları için, eylem eklemek istediğiniz adımAltında **Yeni adımı**seçin. Arama kutusuna filtreniz olarak "salesforce" girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Salesforce'ta oturum açmanız istenirse, şimdi oturum açın ve erişime izin verin.

   Kimlik bilgileriniz, Salesforce'a bağlantı oluşturmak ve verilerinize erişmek için mantık uygulamanıza yetki vetir.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/salesforce/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin