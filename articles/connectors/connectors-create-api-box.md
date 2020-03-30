---
title: Kutuya Bağlan
description: Azure Mantık Uygulamaları'nı kullanarak Kutu'da dosya oluşturan ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: c7f97ff33742eb545cbfbd7521ba135584851e5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666780"
---
# <a name="create-and-manage-files-in-box-by-using-azure-logic-apps"></a>Azure Mantık Uygulamaları'nı kullanarak Kutu'da dosya oluşturma ve yönetme

Bu makalede, Kutu bağlayıcısı ile bir mantık uygulaması içinden Kutu'da dosyalarınızı nasıl oluşturabileceğinizve yönetebileceğiniz gösterilmektedir. Bu şekilde, dosyalarınızı ve diğer eylemlerinizi yönetmek için görevleri ve iş akışlarını otomatikleştiren mantıksal uygulamalar oluşturabilirsiniz, örneğin:

* Box'tan aldığınız verilere göre iş akışınızı oluşturun.

* Bir dosya oluşturulduğunda veya güncelleştirildiğinde otomatik görevleri ve iş akışını tetikle.

* Bir dosyayı kopyalayan veya bir dosyayı silen bir eylem çalıştırın.

  Bu eylemler bir yanıt aldığında, çıktıyı diğer eylemler için kullanılabilir hale getirin. 
  Örneğin, Kutu'da bir dosya değiştirildiğinde, bu dosyayı Office 365'i kullanarak e-posta yla gönderebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Bir Kutu hesabı](https://www.box.com/home)

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Kutu hesabınıza erişmek istediğiniz mantık uygulaması. Mantık uygulamanızı Kutu tetikleyicisi ile başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)ihtiyacınız vardır.

* [Mantık uygulamaları oluşturmak için nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md)temel bilgi .
Mantıksal uygulamalarda yeniyseniz, [Azure Mantıksal Uygulamaları nedir'yi](../logic-apps/logic-apps-overview.md)inceleyin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) dosyasında açıklandığı gibi tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/box/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin