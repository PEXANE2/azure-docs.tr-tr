---
title: Azure Logic Apps 'da tümleştirme hizmeti ortamları (sesleri) için yapıt ekleme
description: Azure sanal ağlarına (VNet) erişmek için tümleştirme hizmeti ortamınıza (ıSE) Logic Apps, özel bağlayıcılar ve tümleştirme hesapları ekleme, özel ve genel veya "genel" Azure 'tan yalıtılmış bir şekilde
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: df43b52514eebc3216dbec01cff0d8a3b14e7940
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517663"
---
# <a name="add-artifacts-to-your-integration-service-environment-ise-in-azure-logic-apps"></a>Azure Logic Apps tümleştirme hizmeti ortamınıza (ıSE) yapıt ekleme

Bir [tümleştirme hizmeti ortamı (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturduktan sonra, Azure sanal ağınızdaki kaynaklara erişebilmeleri için Logic Apps, tümleştirme hesapları ve özel bağlayıcılar gibi yapıtları ekleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantıksal uygulamalarınızı çalıştırmak için oluşturduğunuz ıSE. Bir ıSE yoksa, [önce BIR Ise oluşturun](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-apps-in-an-ise"></a>ISE 'de Logic Apps oluşturma

Tümleştirme hizmeti ortamınızda (ıSE) çalışan Logic Apps oluşturmak için aşağıdaki adımları izleyin:

1. Henüz açık değilse, ıSE 'nizi bulun ve açın. Ise menüsünde, **Ayarlar**' ın altında **Logic Apps** > **Ekle**' yi seçin.

   ![ISE 'ye yeni mantıksal uygulama ekleme](./media/add-artifacts-integration-service-environment-ise/add-logic-app-to-ise.png)

   -veya-

   Ana Azure menüsünden **kaynak** > **tümleştirme** > **mantıksal uygulaması**oluştur ' u seçin.

1. Mantıksal uygulamanız için kullanılacak ad, Azure aboneliği ve Azure kaynak grubunu (yeni veya var olan) sağlayın.

1. **Konum** listesinden, **tümleştirme hizmeti ortamları** bölümünde, Ise 'yi seçin, örneğin:

   ![Tümleştirme hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-logic-app-with-integration-service-environment.png)

   > [!IMPORTANT]
   > Mantıksal uygulamalarınızı bir tümleştirme hesabıyla kullanmak istiyorsanız, bu mantıksal uygulamalar ve tümleştirme hesabı aynı ıSE 'yi kullanmalıdır.

1. [Mantıksal uygulamanızı her zamanki şekilde oluşturmaya](../logic-apps/quickstart-create-first-logic-app-workflow.md)devam edin.

   Tetikleyiciler ve eylemlerin nasıl çalıştığı ve küresel Logic Apps hizmetiyle karşılaştırıldığında bir ıSE kullandığınızda nasıl etiketlendiği hakkında farklılıklar için bkz. [Ise 'ye genel bakış](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference).

1. Ortamınızdaki Logic Apps ve API bağlantılarını yönetmek için bkz. [tümleştirme hizmeti ortamınızı yönetme](../logic-apps/ise-manage-integration-service-environment.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-accounts-in-an-ise"></a>Bir ıSE 'de tümleştirme hesapları oluşturma

Oluşturma sırasında seçilen [ıSE SKU 'su](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) temel alınarak, Ise 'niz hiçbir ek ücret ödemeden belirli tümleştirme hesabı kullanımını içerir. Bir tümleştirme hizmeti ortamında (ıSE) bulunan Logic Apps, yalnızca aynı ıSE 'de bulunan tümleştirme hesaplarına başvurabilir. Bu nedenle, bir tümleştirme hesabının bir ıSE 'de Logic Apps ile çalışması için, hem tümleştirme hesabı hem de Logic Apps, konumuyla *aynı ortamı* kullanmalıdır. Tümleştirme hesapları ve sesleri hakkında daha fazla bilgi için bkz [. Ise](connect-virtual-network-vnet-isolated-environment-overview.md#create-integration-account-environment
)ile tümleştirme hesapları.

ISE kullanan bir tümleştirme hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Henüz açık değilse, ıSE 'nizi bulun ve açın. Ise menüsünde, **Ayarlar**' ın altında **tümleştirme hesapları** > **Ekle**' yi seçin.

   ![ISE 'ye yeni tümleştirme hesabı ekleme](./media/add-artifacts-integration-service-environment-ise/add-integration-account-to-ise.png)

   -veya-

   Ana Azure menüsünden **kaynak** > **tümleştirme** > **tümleştirme hesabı**oluştur ' u seçin.

1. Tümleştirme hesabınız için kullanılacak ad, Azure aboneliği, Azure Kaynak grubu (yeni veya var olan) ve fiyatlandırma katmanını sağlayın.

1. **Konum** listesinden, **tümleştirme hizmeti ortamları** bölümünde, mantıksal uygulamalarınızın kullandığı Ise 'yi seçin, örneğin:

   ![Tümleştirme hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-integration-account-with-integration-service-environment.png)

1. [Mantıksal uygulamanızı, her zamanki şekilde tümleştirme hesabınıza bağlayın](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account).

1. Tümleştirme hesabınıza, [ticari iş ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [anlaşmalar](../logic-apps/logic-apps-enterprise-integration-agreements.md)gibi yapıtlar ekleyerek devam edin.

1. Şirket içindeki tümleştirme hesaplarını yönetmek için bkz. [tümleştirme hizmeti ortamınızı yönetme](../logic-apps/ise-manage-integration-service-environment.md).

<a name="create-custom-connectors-environment"></a>

## <a name="create-custom-connectors-in-an-ise"></a>ISE 'de özel bağlayıcılar oluşturma

ISE 'de özel bağlayıcılar kullanmak için, bu özel bağlayıcıları doğrudan ıSE içinden oluşturun.

1. Henüz açık değilse, ıSE 'nizi bulun ve açın. Ise menüsünde, **Ayarlar**' ın altında **özel bağlayıcılar** > **Ekle**' yi seçin.

   ![Özel bağlayıcı oluşturma](./media/add-artifacts-integration-service-environment-ise/add-custom-connector-to-ise.png)

1. Özel Bağlayıcınız için kullanılacak ad, Azure aboneliği ve Azure kaynak grubunu (yeni veya var olan) sağlayın.

1. **Konum** listesinden, **tümleştirme hizmeti ortamları** bölümünde, mantıksal uygulamalarınızın kullandığı Ise 'yi seçin ve **Oluştur**' u seçin, örneğin:

   ![Tümleştirme hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-custom-connector-with-integration-service-environment.png)

1. Yeni özel bağlayıcınızı seçin ve ardından **Düzenle**' yi seçin, örneğin:

   ![Özel bağlayıcıyı seçme ve düzenleme](./media/add-artifacts-integration-service-environment-ise/edit-custom-connectors.png)

1. Bir [Openapı tanımından](https://docs.microsoft.com/connectors/custom-connectors/define-openapi-definition#import-the-openapi-definition) veya [SOAP](https://docs.microsoft.com/connectors/custom-connectors/create-register-logic-apps-soap-connector#2-define-your-connector)'tan her zamanki şekilde bağlayıcı oluşturarak devam edin.

1. Çalışma ortamınızdaki özel bağlayıcıları yönetmek için bkz. [tümleştirme hizmeti ortamınızı yönetme](../logic-apps/ise-manage-integration-service-environment.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md)
