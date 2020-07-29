---
title: Dağıtım & Azure Container Instances yönetme
description: Azure Logic Apps kullanarak Azure Container Instances kapsayıcı dağıtımlarını oluşturan ve yöneten görevleri ve iş akışlarını otomatikleştirin
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76046298"
---
# <a name="deploy-and-manage-azure-container-instances-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Azure Container Instances dağıtın ve yönetin

Azure Logic Apps ve Azure Container Instance Bağlayıcısı sayesinde, [kapsayıcı grupları](../container-instances/container-instances-container-groups.md)dağıtan ve yöneten otomatik görevleri ve iş akışlarını ayarlayabilirsiniz. Kapsayıcı örneği Bağlayıcısı aşağıdaki eylemleri destekler:

* Kapsayıcı grubu oluşturma veya silme
* Bir kapsayıcı grubunun özelliklerini al
* Kapsayıcı gruplarının bir listesini alın
* Bir kapsayıcı örneğinin günlüklerini al

Logic Apps tetikleyicisine yanıt olarak kapsayıcı iş yükü çalıştırma gibi görevler için mantıksal uygulamalarınızda bu eylemleri kullanın. Ayrıca, kapsayıcı örneği eylemlerinden alınan çıktıyı başka eylemlere de kullanabilirsiniz. 

Bu bağlayıcı yalnızca eylemler sağlar, bu nedenle mantıksal uygulamanızı başlatmak için bir kapsayıcı iş yükünü düzenli bir zamanlamaya göre çalıştırmak için **yineleme** tetikleyicisi gibi ayrı bir tetikleyici kullanın. Ya da bir Outlook e-postası gibi bir olaydan sonra kapsayıcı grubu dağıtımı tetiklemeniz gerekebilir. 

Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ve [kapsayıcı örnekleri oluşturma ve yönetme](../container-instances/container-instances-quickstart.md) hakkında temel bilgi

* Kapsayıcı örneklerinize erişmek istediğiniz mantıksal uygulama. Bir eylem kullanmak için, mantıksal uygulamanızı başka bir tetikleyici ile başlatın, örneğin **yineleme** tetikleyicisi.

## <a name="add-a-container-instance-action"></a>Kapsayıcı örneği eylemi ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Bir yol seçin: 

   * Eylem eklemek istediğiniz son adım altında **yeni adım**' ı seçin. 

     -veya-

   * Eylem eklemek istediğiniz adımlar arasında, işaretçinizi adımlar arasındaki oka taşıyın. 
   Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Arama kutusuna filtreniz olarak "kapsayıcı örneği" yazın. Eylemler listesi altında istediğiniz Azure Container Instance bağlayıcı eylemini seçin.

1. Bağlantınıza bir ad verin. 

1. Seçtiğiniz eyleminiz için gerekli ayrıntıları sağlayın ve mantıksal uygulamanızın iş akışını oluşturmaya devam edin.

  Örneğin, **kapsayıcı grubu oluştur** ' u seçin ve aşağıdaki görüntüde gösterildiği gibi bir kapsayıcı grubu ve gruptaki bir veya daha fazla kapsayıcı örneği için özellikleri girin (kısmi ayrıntı):

  ![Kapsayıcı grubu oluştur](./media/connectors-create-api-container-instances/logic-apps-aci-connector.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/aci/) veya kapsayıcı grubu [YAML başvurusunu](../container-instances/container-instances-reference-yaml.md)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* E-posta veya Twitter metninin yaklaşımını çözümlemek için Azure Container Instances bir kapsayıcı çalıştıran [örnek bir mantıksal uygulama](https://github.com/Azure-Samples/aci-logicapps-integration) görüntüleyin

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin