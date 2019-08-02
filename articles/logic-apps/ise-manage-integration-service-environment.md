---
title: Azure Logic Apps tümleştirme hizmeti ortamlarını yönetme
description: Azure Logic Apps için tümleştirme hizmeti ortamınızda (ıSE) ağ durumunu kontrol edin ve Logic Apps, bağlantılar, özel bağlayıcılar ve tümleştirme hesaplarını yönetin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: 8f10e3d3fd7c67d1e803e8f85c9918c91bb81d59
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517468"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Azure Logic Apps tümleştirme hizmeti ortamınızı (ıSE) yönetme

[Tümleştirme hizmeti ortamınız (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) için ağ durumunu denetlemek ve ortamınızda bulunan Logic Apps, bağlantılar, tümleştirme hesapları ve özel bağlayıcılar yönetmek için bu konudaki adımları izleyin.

## <a name="view-your-ise"></a>ISE 'nizi görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Portalın arama kutusuna "tümleştirme hizmeti ortamları" yazın ve ardından **tümleştirme hizmeti ortamları**' nı seçin.

   ![Tümleştirme hizmeti ortamlarını bulun](./media/ise-manage-integration-service-environment/find-integration-service-environment.png)

1. Sonuçlar listesinden tümleştirme hizmeti ortamınızı seçin.

   ![Tümleştirme hizmeti ortamını seçin](./media/ise-manage-integration-service-environment/select-integration-service-environment.png)

1. Şirket içinde Logic Apps, bağlantılar, bağlayıcılar veya tümleştirme hesapları bulmak için sonraki bölümlere devam edin.

<a name="check-network-health"></a>

## <a name="check-network-health"></a>Ağ durumunu denetle

ISE menüsünde **Ayarlar**' ın altında **ağ durumu**' nu seçin. Bu bölmede, alt ağlarınızın sistem durumu ve diğer hizmetlere giden bağımlılıklarınız gösterilir.

![Ağ durumunu denetle](./media/ise-manage-integration-service-environment/ise-check-network-health.png)

<a name="find-logic-apps"></a>

## <a name="manage-your-logic-apps"></a>Mantıksal uygulamalarınızı yönetin

1. ISE menüsünde, **Ayarlar**' ın altında **Logic Apps**' i seçin.

   ![Logic Apps bulma](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Artık gerekmiyorsa mantıksal uygulamaları ıSE 'den kaldırmak için bu mantıksal uygulamaları seçin ve **Sil**' i seçin.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>API bağlantılarını yönetme

1. ISE 'de çalışan Logic Apps tarafından oluşturulan API bağlantılarını görüntülemek için, çalışma menüsünde, **Ayarlar**' ın altında, **API bağlantıları**' nı seçin.

   ![API bağlantılarını bul](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Artık gerekli olmayan bağlantıları bağlantı noktasından kaldırmak için bu bağlantıları seçin ve **Sil**' i seçin.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Özel bağlayıcıları yönetme

1. ISE ortamınızda oluşturulan özel bağlayıcıları görüntülemek için, ıSE menüsünde, **Ayarlar**' ın altında **özel bağlayıcılar**' ı seçin.

   ![Özel bağlayıcıları bulma](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Artık gerekli olmayan özel bağlayıcıları ıSE bilgisayarınızdan kaldırmak için bu Bağlayıcılar ' ı seçin ve **Sil**' i seçin.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Tümleştirme hesaplarını yönetme

1. ISE menüsünde **Ayarlar**' ın altında **tümleştirme hesapları**' nı seçin.

   ![Tümleştirme hesaplarını bul](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Artık gerekmeyen tümleştirme hesaplarını ıSE bilgisayarınızdan kaldırmak için bu tümleştirme hesaplarını seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Yalıtılmış Logic Apps 'Ten Azure sanal ağlarına](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) nasıl bağlanacağınızı öğrenin
