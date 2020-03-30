---
title: Azure Kapsayıcı Örneklerini dağıtma & yönetme
description: Azure Mantıksal Uygulamaları'nı kullanarak Azure Kapsayıcı Örnekleri'nde kapsayıcı dağıtımları oluşturan ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps, container-instances
ms.service: logic-apps
ms.suite: integration
author: dlepow
ms.author: danlep
ms.manager: gwallace
ms.reviewer: estfan, macolso
ms.topic: article
tags: connectors
ms.date: 01/14/2020
ms.openlocfilehash: ecb1049d64197f2a60438df7eedfb244907f7327
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76046298"
---
# <a name="deploy-and-manage-azure-container-instances-by-using-azure-logic-apps"></a>Azure Mantıksal Uygulamaları kullanarak Azure Kapsayıcı Örneklerini dağıtma ve yönetme

Azure Mantık Uygulamaları ve Azure Kapsayıcı Örneği bağlayıcısı [ile, kapsayıcı gruplarını](../container-instances/container-instances-container-groups.md)dağıtan ve yöneten otomatik görevler ve iş akışları ayarlayabilirsiniz. Kapsayıcı Örneği bağlayıcısı aşağıdaki eylemleri destekler:

* Kapsayıcı grubu oluşturma veya silme
* Kapsayıcı grubunun özelliklerini alma
* Konteyner gruplarının listesini alma
* Kapsayıcı örneğinin günlüklerini alma

Mantık Uygulamaları tetikleyicisine yanıt olarak kapsayıcı iş yükünü çalıştırmak gibi görevler için mantık uygulamalarınızda bu eylemleri kullanın. Diğer eylemlerin Kapsayıcı Örneği eylemlerinden çıktı kullanmasını da sağlayabilirsiniz. 

Bu bağlayıcı yalnızca eylemler sağlar, bu nedenle mantık uygulamanızı başlatmak için, kapsayıcı iş yükünü düzenli bir zamanlamada çalıştırmak için **Yineleme tetikleyicisi** gibi ayrı bir tetikleyici kullanın. Veya, Outlook e-postasının gelmesi gibi bir olaydan sonra bir kapsayıcı grubu dağıtımını tetiklemeniz gerekebilir. 

Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Mantık uygulamalarının nasıl oluşturulup](../logic-apps/quickstart-create-first-logic-app-workflow.md) oluşturulması ve [konteyner örneklerinin nasıl oluşturulup yönetileceği](../container-instances/container-instances-quickstart.md) hakkında temel bilgiler

* Kapsayıcı örneklerinize erişmek istediğiniz mantık uygulaması. Bir eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="add-a-container-instance-action"></a>Kapsayıcı Örneği eylemi ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Bir yol seçin: 

   * Eylem eklemek istediğiniz son adımaltında Yeni **adımı**seçin. 

     -veya-

   * Eylem eklemek istediğiniz adımlar arasında, işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "kapsayıcı örneği"ni girin. Eylemler listesinin altında, istediğiniz Azure Kapsayıcı Örneği bağlayıcı eylemini seçin.

1. Bağlantınıza bir ad verin. 

1. Seçtiğiniz eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

  Örneğin, **kapsayıcı grubu oluştur'u** seçin ve aşağıdaki resimde (kısmi ayrıntı) gösterildiği gibi, bir kapsayıcı grubu ve gruptaki bir veya daha fazla kapsayıcı örneğinin özelliklerini girin:

  ![Kapsayıcı grubu oluştur](./media/connectors-create-api-container-instances/logic-apps-aci-connector.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/aci/) veya kapsayıcı grubu [YAML referansını](../container-instances/container-instances-reference-yaml.md)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* E-posta veya Twitter metninin duyarlılığını analiz etmek için Azure Kapsayıcı Örnekleri'nde kapsayıcı çalıştıran örnek bir [mantık uygulamasına](https://github.com/Azure-Samples/aci-logicapps-integration) bakın

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin