---
title: Tümleştirme hizmeti ortamına yapıt ekleme
description: Azure sanal ağlarına (VNet) erişmek için tümleştirme hizmeti ortamınıza (ıSE) Logic Apps, tümleştirme hesapları ve özel bağlayıcılar ekleme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: c597bc4430e4390f0e29e4fe8ae4014521e1ae74
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732281"
---
# <a name="add-artifacts-to-your-integration-service-environment-ise-in-azure-logic-apps"></a>Azure Logic Apps tümleştirme hizmeti ortamınıza (ıSE) yapıt ekleme

Bir [tümleştirme hizmeti ortamı (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturduktan sonra, Azure sanal ağınızdaki kaynaklara erişebilmeleri için Logic Apps, tümleştirme hesapları ve bağlayıcılar gibi yapıtları ekleyin. Örneğin, ıSE 'nizi oluşturduktan sonra kullanılabilir hale gelen yönetilen ıSE bağlayıcıları mantıksal uygulama tasarımcısında otomatik olarak görünmez. Bu ıSE bağlayıcılarını kullanabilmeniz için, mantıksal uygulama Tasarımcısı 'nda gözükmeleri için [Bu bağlayıcıları el ile ekleyip dağıtmanız](#add-ise-connectors-environment) gerekir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantıksal uygulamalarınızı çalıştırmak için oluşturduğunuz ıSE. Bir ıSE yoksa, [önce BIR Ise oluşturun](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-apps"></a>Mantıksal uygulamalar oluşturma

Tümleştirme hizmeti ortamınızda (ıSE) çalışan Logic Apps oluşturmak için aşağıdaki adımları izleyin:

1. Henüz açık değilse, ıSE 'nizi bulun ve açın. ISE menüsünde **Ayarlar**' ın altında, **Ekle** > **Logic Apps** ' i seçin.

   ![ISE 'ye yeni mantıksal uygulama ekleme](./media/add-artifacts-integration-service-environment-ise/add-logic-app-to-ise.png)

   -veya-

   Ana Azure menüsünden **kaynak oluştur** > **tümleştirme** > **mantıksal uygulama**' yı seçin.

1. Mantıksal uygulamanız için kullanılacak ad, Azure aboneliği ve Azure kaynak grubunu (yeni veya var olan) sağlayın.

1. **Konum** listesinden, **tümleştirme hizmeti ortamları** bölümünde, Ise 'yi seçin, örneğin:

   ![Tümleştirme hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-logic-app-with-integration-service-environment.png)

   > [!IMPORTANT]
   > Mantıksal uygulamalarınızı bir tümleştirme hesabıyla kullanmak istiyorsanız, bu mantıksal uygulamalar ve tümleştirme hesabı aynı ıSE 'yi kullanmalıdır.

1. [Mantıksal uygulamanızı her zamanki şekilde oluşturmaya](../logic-apps/quickstart-create-first-logic-app-workflow.md)devam edin.

   Tetikleyiciler ve eylemlerin nasıl çalıştığı ve küresel Logic Apps hizmetiyle karşılaştırıldığında bir ıSE kullandığınızda nasıl etiketlendiği hakkında farklılıklar için bkz. [Ise 'ye genel bakış](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference).

1. Ortamınızdaki Logic Apps ve API bağlantılarını yönetmek için bkz. [tümleştirme hizmeti ortamınızı yönetme](../logic-apps/ise-manage-integration-service-environment.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-accounts"></a>Tümleştirme hesapları oluşturma

Oluşturma sırasında seçilen [ıSE SKU 'su](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) temel alınarak, Ise 'niz hiçbir ek ücret ödemeden belirli tümleştirme hesabı kullanımını içerir. Bir tümleştirme hizmeti ortamında (ıSE) bulunan Logic Apps, yalnızca aynı ıSE 'de bulunan tümleştirme hesaplarına başvurabilir. Bu nedenle, bir tümleştirme hesabının bir ıSE 'de Logic Apps ile çalışması için, hem tümleştirme hesabı hem de Logic Apps, konumuyla *aynı ortamı* kullanmalıdır. Tümleştirme hesapları ve sesleri hakkında daha fazla bilgi için bkz. [Ise Ile tümleştirme hesapları](connect-virtual-network-vnet-isolated-environment-overview.md#create-integration-account-environment).

ISE kullanan bir tümleştirme hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Henüz açık değilse, ıSE 'nizi bulun ve açın. ISE menüsünde, **Ayarlar**' ın altında, **Ekle** > **tümleştirme hesapları** ' nı seçin.

   ![ISE 'ye yeni tümleştirme hesabı ekleme](./media/add-artifacts-integration-service-environment-ise/add-integration-account-to-ise.png)

   -veya-

   Ana Azure menüsünden **kaynak oluştur** > **tümleştirme** > **tümleştirme hesabı**' nı seçin.

1. Tümleştirme hesabınız için kullanılacak ad, Azure aboneliği, Azure Kaynak grubu (yeni veya var olan) ve fiyatlandırma katmanını sağlayın.

1. **Konum** listesinden, **tümleştirme hizmeti ortamları** bölümünde, mantıksal uygulamalarınızın kullandığı Ise 'yi seçin, örneğin:

   ![Tümleştirme hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-integration-account-with-integration-service-environment.png)

1. [Mantıksal uygulamanızı, her zamanki şekilde tümleştirme hesabınıza bağlayın](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account).

1. Tümleştirme hesabınıza, [ticari iş ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [anlaşmalar](../logic-apps/logic-apps-enterprise-integration-agreements.md)gibi yapıtlar ekleyerek devam edin.

1. Şirket içindeki tümleştirme hesaplarını yönetmek için bkz. [tümleştirme hizmeti ortamınızı yönetme](../logic-apps/ise-manage-integration-service-environment.md).

<a name="add-ise-connectors-environment"></a>

## <a name="add-ise-connectors"></a>ISE bağlayıcıları ekleme

ISE 'nizi oluşturduktan sonra sunulan Microsoft tarafından yönetilen bağlayıcılar, mantıksal uygulama Tasarımcısı 'ndaki bağlayıcı seçicisinde otomatik olarak görünmez. Bu ıSE bağlayıcılarını kullanabilmeniz için, mantıksal uygulama Tasarımcısı 'nda görünmesi için bu bağlayıcıları el ile eklemeniz ve ıSE 'ye dağıtmanız gerekir.

1. ISE menüsünde, **Ayarlar**' ın altında, **yönetilen bağlayıcılar**' ı seçin. Araç çubuğunda **Ekle**' yi seçin.

   ![Yönetilen bağlayıcıları görüntüleme](./media/add-artifacts-integration-service-environment-ise/ise-view-managed-connectors.png)

1. **Yeni bir yönetilen bağlayıcı Ekle** bölmesinde, **bağlayıcı bul** listesini açın. Kullanmak istediğiniz, ancak henüz ıSE dağıtımı yapılmamış olan ıSE bağlayıcısını seçin. **Oluştur**’u seçin.

   ![ISE 'de dağıtmak istediğiniz ıSE bağlayıcısını seçin](./media/add-artifacts-integration-service-environment-ise/add-managed-connector.png)

   Yalnızca sizin için uygun olan ancak yalnız ıSE 'e dağıtılan ıSE bağlayıcıları, seçmeniz için kullanılabilir. ISE 'de zaten dağıtılan bağlayıcılar seçim için kullanılamaz olarak görünür.

<a name="create-custom-connectors-environment"></a>

## <a name="create-custom-connectors"></a>Özel Bağlayıcılar oluşturma

ISE 'de özel bağlayıcılar kullanmak için, bu özel bağlayıcıları doğrudan ıSE içinden oluşturun.

1. Henüz açık değilse, ıSE 'nizi bulun ve açın. ISE menüsünde, **Ayarlar**' ın altında, **Ekle** > **özel bağlayıcılar** ' ı seçin.

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
