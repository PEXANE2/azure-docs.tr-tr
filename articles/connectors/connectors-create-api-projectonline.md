---
title: Azure Logic Apps'tan Project Online'a bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak Project Online projelerini, görevlerini ve kaynaklarını izleyen, oluşturan ve yöneten iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 116922e018410f260b33f4dfd76e5983fbbcc0a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789333"
---
# <a name="manage-project-online-projects-tasks-and-resources-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak Project Online projelerini, görevlerini ve kaynaklarını yönetme

Azure Logic Apps ve Project Online bağlayıcısı ile Project Online'da Projeleriniz, görevleriniz ve kaynaklarınız için Office 365 aracılığıyla otomatik görevler ve iş akışları oluşturabilirsiniz. İş akışlarınız bu eylemleri ve diğer eylemleri gerçekleştirebilir, örneğin:

* Yeni projeler, görevler veya kaynaklar oluşturulduğunda izleyin. Veya yeni projeler yayımlandığında izleyin.
* Yeni projeler, görevler veya kaynaklar oluşturun.
* Varolan projeleri veya görevleri listele.
* Projeleri kontrol edin, iade edin veya yayımlayın.

Project Online, güçlü proje yönetimi yetenekleri sağlayarak hemen hemen her cihazda proje ve proje portföy yatırımlarını planlamanıza, önceliklendirmenize ve yönetmenize yardımcı olur. Project Online'dan yanıt alan ve çıktıyı diğer eylemleriçin kullanılabilir hale getiren Project Online tetikleyicilerini kullanabilirsiniz. Project Online'da çeşitli görevleri gerçekleştirmek için mantık uygulamalarınızdaki eylemleri kullanabilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Project Online, Office [365 hesabı](https://www.office.com/)üzerinden kullanılabilir , 

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Project Online verilerinize erişmek istediğiniz mantık uygulaması. Project Online tetikleyicisiyle başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Project Online eylemlerini kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-project-online"></a>Project Online'a bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "Project Online"ı girin. 
   Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Varolan mantık uygulamaları için, eylem eklemek istediğiniz adımAltında **Yeni adımı**seçin. Arama kutusuna filtreniz olarak "Project Online"ı girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

1. Project Online'da oturum açmanız istenirse, hemen oturum açın.

   Kimlik bilgileriniz, Project Online'a bağlantı oluşturmak ve verilerinize erişmek için mantık uygulamanıza yetki vetir.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/projectonline/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin