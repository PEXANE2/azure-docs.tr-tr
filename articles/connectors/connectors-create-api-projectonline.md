---
title: Azure Logic Apps 'tan Project Online 'a bağlanma
description: Azure Logic Apps kullanarak proje çevrimiçi projelerini, görevleri ve kaynakları izleyen, oluşturan ve yöneten iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: 116922e018410f260b33f4dfd76e5983fbbcc0a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74789333"
---
# <a name="manage-project-online-projects-tasks-and-resources-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Project Online projelerini, görevlerini ve kaynaklarını yönetme

Azure Logic Apps ve Project Online Bağlayıcısı sayesinde, Office 365 aracılığıyla Project Online 'daki projeleriniz, görevleriniz ve kaynaklarınız için otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. İş akışlarınız bu eylemleri ve diğerlerini gerçekleştirebilir, örneğin:

* Yeni proje, görev veya kaynak oluşturulduğunda izleyin. Ya da yeni projeler yayımlandığında izleyin.
* Yeni projeler, görevler veya kaynaklar oluşturun.
* Mevcut projeleri veya görevleri listeleyin.
* Projeleri kullanıma alma, iade etme veya yayımlama.

Project Online, güçlü proje yönetimi özellikleri sağlayarak neredeyse tüm cihazlarda projeler ve proje portföyü yatırımlarını planlamanızı, önceliklendirmenize ve yönetmenize yardımcı olur. Projeden çevrimiçi olarak yanıt alan Project Online tetikleyicilerini kullanabilir ve çıktıyı diğer eylemler için kullanılabilir hale getirebilirsiniz. Project Online 'da çeşitli görevleri gerçekleştirmek için Logic Apps 'teki işlemleri kullanabilirsiniz. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Office 365 hesabıyla](https://www.office.com/)sunulan Project Online, 

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Project Online verilerinize erişmek istediğiniz mantıksal uygulama. Bir Project Online tetikleyicisiyle başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Project Online eylemlerini kullanmak için, mantıksal uygulamanızı başka bir tetikleyici ile başlatın, örneğin **yineleme** tetikleyicisi.

## <a name="connect-to-project-online"></a>Project Online 'a bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Bir yol seçin: 

   * Boş Logic Apps için, arama kutusuna filtreniz olarak "Project Online" yazın. 
   Tetikleyiciler listesinde istediğiniz tetikleyiciyi seçin. 

     -veya-

   * Mevcut Logic Apps için, eylem eklemek istediğiniz adım altında **yeni adım**' ı seçin. Arama kutusuna filtreniz olarak "Project Online" yazın. Eylemler listesi altında istediğiniz eylemi seçin.

1. Project Online 'da oturum açmanız istenirse, şimdi oturum açın.

   Kimlik bilgileriniz, mantıksal uygulamanızı Project Online 'a bir bağlantı oluşturmak ve verilerinize erişmek için yetkilendirin.

1. Seçtiğiniz tetikleyici veya eyleminiz için gerekli ayrıntıları sağlayın ve mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/projectonline/)gözden geçirin.

## <a name="get-support"></a>Destek alma

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin