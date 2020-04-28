---
title: Kutuya Bağlan
description: Azure Logic Apps kullanarak Box 'ta dosya oluşturup yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: c7f97ff33742eb545cbfbd7521ba135584851e5e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75666780"
---
# <a name="create-and-manage-files-in-box-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Box 'ta dosya oluşturma ve yönetme

Bu makalede, Box Bağlayıcısı ile bir mantıksal uygulamanın içinden dosyaları nasıl oluşturabileceğiniz ve yönetebileceğiniz gösterilmektedir. Bu şekilde, dosyalarınızı ve diğer eylemlerinizi yönetmek için görevleri ve iş akışlarını otomatikleştirebilen mantıksal uygulamalar oluşturabilirsiniz, örneğin:

* İş akışınızı, aldığınız verilere göre oluşturun.

* Bir dosya oluşturulduğunda veya güncelleştirilirken otomatikleştirilmiş görevleri ve iş akışını tetikler.

* Bir dosyayı kopyalayan veya dosyayı silen bir eylem çalıştırın.

  Bu eylemler bir yanıt edindiklerinde, çıktıyı diğer eylemler için kullanılabilir hale getirir. 
  Örneğin, Box 'ta bir dosya değiştirildiğinde, Office 365 kullanarak bu dosyayı e-posta ile gönderebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Kutu hesabı](https://www.box.com/home)

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Box hesabınıza erişmek istediğiniz mantıksal uygulama. Mantıksal uygulamanızı bir kutu tetikleyicisi ile başlatmak için [boş bir mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)gerekir.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi.
Logic Apps 'e yeni başladıysanız [Azure Logic Apps ne olduğunu](../logic-apps/logic-apps-overview.md)gözden geçirin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) dosyasında açıklandığı gibi Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/box/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin