---
title: Entegrasyon hizmeti ortamlarına kaynak ekleme
description: Entegrasyon hizmet ortamınıza (Ise) mantık uygulamaları, entegrasyon hesapları, özel bağlayıcılar ve yönetilen bağlayıcılar ekleyin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/28/2020
ms.openlocfilehash: 58d2efd0c61045739930ce36ba317b1aa6a40ce8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79164884"
---
# <a name="add-resources-to-your-integration-service-environment-ise-in-azure-logic-apps"></a>Azure Logic Apps'ta entegrasyon hizmeti ortamınıza (ISE) kaynak ekleme

Bir [tümleştirme hizmeti ortamı (Ise)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturduktan sonra, Azure sanal ağınızdaki kaynaklara erişebilmeleri için mantık uygulamaları, tümleştirme hesapları ve bağlayıcılar gibi kaynaklar ekleyin. Örneğin, İmKB'nizi oluşturduktan sonra kullanıma sunulan yönetilen Ise konektörleri, Mantık Uygulama Tasarımcısı'nda otomatik olarak görünmez. Bu İmKB konektörlerini kullanabilmeniz için, bu konektörleri Logic App Designer'da görünmesi için El ile [İmKB'nize eklemeniz ve dağıtmanız](#add-ise-connectors-environment) gerekir.

> [!IMPORTANT]
> Mantıksal uygulamaların ve tümleştirme hesaplarının Bir İmKB'de birlikte çalışabilmesi için her ikisinin de konumlarıyla *aynı İmKB'yi* kullanması gerekir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantık uygulamalarınızı çalıştırmak için oluşturduğunuz İmKB. Bir İmKB'niz yoksa, [önce bir ISE oluşturun.](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

* Bir İmKB'de dağıtılan kaynakları oluşturmak, eklemek veya güncelleştirmek için, bu İmKB'de Sahip veya Katılımcı rolüne atanmamanız veya İmKB ile ilişkili Azure aboneliği veya Azure kaynak grubu aracılığıyla devralınan izinlere sahip olmanız gerekir. Sahibi, katılımcısı veya devralınan izinleri olmayan kişiler için, Bunlara Tümleştirme Hizmeti Ortamı Katılımcısı rolü veya Tümleştirme Hizmeti Ortamı Geliştiricisi rolü atanabilir. Rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi için azure [kaynakları için rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-apps"></a>Mantık uygulamaları oluşturma

Entegrasyon hizmeti ortamınızda (İmKB) çalışan mantık uygulamaları oluşturmak için aşağıdaki adımları izleyin:

1. Zaten açık değilse, İmKB'nizi bulun ve açın. İmKB menüsünden **Ayarlar**altında, **Mantık uygulamalarını** > seçin**Ekle**.

   ![İmKB'ye yeni mantık uygulaması ekleme](./media/add-artifacts-integration-service-environment-ise/add-logic-app-to-ise.png)

1. Oluşturmak istediğiniz mantık uygulaması hakkında bilgi sağlayın, örneğin:

   ![Entegrasyon hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-logic-app-integration-service-environment.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Adı** | Evet | Oluşturulacak mantık uygulamasının adı |
   | **Abonelik** | Evet | Kullanılacak Azure aboneliğinin adı |
   | **Kaynak grubu** | Evet | Kullanılacak Azure kaynak grubunun adı (yeni veya varolan) |
   | **Konum** | Evet | **Tümleştirme hizmet ortamları**altında, zaten seçilmemişse kullanılacak İmKB'yi seçin. <p><p> **Önemli**: Mantık uygulamalarınızı bir entegrasyon hesabıyla kullanmak için her ikisi de aynı İmKB'yi kullanmalıdır. |
   ||||

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Mantık uygulamanızı her zamanki gibi oluşturmaya devam [edin.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

   Çok kiracılı Logic Apps hizmetiyle karşılaştırıldığında tetikleyicilerin ve eylemlerin nasıl çalıştığı ve bir İmKB kullandığınızda nasıl etiketlendikleri ile ilgili farklılıklar için, [İmKB genel bakışında Yalıtılmış ve çok kiracılı](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference)olarak bkz.

1. İmKB'inizde mantık uygulamalarını ve API bağlantılarını yönetmek için [bkz.](../logic-apps/ise-manage-integration-service-environment.md)

<a name="create-integration-account-environment"></a>

## <a name="create-integration-accounts"></a>Tümleştirme hesapları oluşturma

Oluşturma sırasında seçilen [ISE SKU'ya](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) bağlı olarak, İmKB'niz ek ücret ödemeden özel entegrasyon hesabı kullanımını içerir. Bir tümleştirme hizmeti ortamında (İmKB) bulunan mantıksal uygulamalar, yalnızca aynı İmKB'de bulunan tümleştirme hesaplarına başvurur. Bu nedenle, bir entegrasyon hesabının Bir İmKB'deki mantık uygulamalarıyla çalışabilmesi için hem entegrasyon hesabının hem de mantık uygulamalarının konumlarıyla *aynı ortamı* kullanması gerekir. Tümleştirme hesapları ve ISE'ler hakkında daha fazla bilgi için [Ise ile Tümleştirme hesaplarına](connect-virtual-network-vnet-isolated-environment-overview.md#create-integration-account-environment)bakın.

Bir İmKB kullanan bir tümleştirme hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Zaten açık değilse, İmKB'nizi bulun ve açın. İmKB menüsünden **Ayarlar**altında, **Tümleştirme hesaplarını** > **seçin Ekle**.

   ![İmKB'ye yeni entegrasyon hesabı ekleme](./media/add-artifacts-integration-service-environment-ise/add-integration-account-to-ise.png)

1. Oluşturmak istediğiniz mantık uygulaması hakkında bilgi sağlayın, örneğin:

   ![Entegrasyon hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-integration-account-integration-service-environment.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Adı** | Evet | Oluşturmak istediğiniz tümleştirme hesabının adı |
   | **Abonelik** | Evet | Kullanmak istediğiniz Azure aboneliğinin adı |
   | **Kaynak grubu** | Evet | Kullanılacak Azure kaynak grubunun adı (yeni veya varolan) |
   | **Fiyatlandırma katmanı** | Evet | Tümleştirme hesabı için kullanılacak fiyatlandırma katmanı |
   | **Konum** | Evet | **Tümleştirme hizmet ortamları**altında, zaten seçilmemişse, mantık uygulamalarınızın kullandığı Aynı İmKB'yi seçin. <p><p> **Önemli**: Entegrasyon hesabınızı mantık uygulamalarıyla kullanmak için her ikisi nin de aynı İmKB'yi kullanması gerekir. |
   ||||

1. İşiniz bittiğinde **Oluştur**’u seçin.

1. [Mantık uygulamanızı her zamanki gibi entegrasyon hesabınıza bağlayın.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)

1. [İşlem ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [anlaşmalar](../logic-apps/logic-apps-enterprise-integration-agreements.md)gibi entegrasyon hesabınıza kaynak ekleyerek devam edin.

1. İmKB'nizdeki tümleştirme hesaplarını yönetmek için [bkz.](../logic-apps/ise-manage-integration-service-environment.md)

<a name="add-ise-connectors-environment"></a>

## <a name="add-ise-connectors"></a>İmKB konektörleri ekle

İmKB'nizi oluşturduktan sonra kullanıma sunulan Microsoft tarafından yönetilen bağlayıcılar, Logic App Designer'Daki bağlayıcı seçicide otomatik olarak görünmez. Bu İmKB konektörlerini kullanabilmeniz için önce, Bu konektörleri Logic App Designer'da görünmesi için El ile İmKB'nize eklemeniz ve dağıtmanız gerekir.

1. İmKB menünüzde **Ayarlar**altında **Yönetilen bağlayıcıları**seçin. Araç çubuğunda **Ekle'yi**seçin.

   ![Yönetilen konektörleri görüntüleme](./media/add-artifacts-integration-service-environment-ise/ise-view-managed-connectors.png)

1. Yeni **yönetilen bağlayıcı** bölmesini ekle'de Bul **bağlayıcı** listesini açın. Kullanmak istediğiniz ancak Henüz İmKB'de dağıtılmadı. **Oluştur'u**seçin.

   ![İmKB'nizde dağıtmak istediğiniz İmKB konektörünü seçin](./media/add-artifacts-integration-service-environment-ise/add-managed-connector.png)

   Yalnızca Uygun olan ancak henüz İmKB'nize dağıtılmayan İmKB konektörleri seçebilecek şekilde görünür. İmKB'nizde zaten dağıtılan bağlayıcılar seçim için kullanılamaz görünür.

<a name="create-custom-connectors-environment"></a>

## <a name="create-custom-connectors"></a>Özel bağlayıcılar oluşturma

İmKB'nizde özel konektörler kullanmak için, bu özel konektörleri doğrudan İmKB'nizin içinden oluşturun.

1. Zaten açık değilse, İmKB'nizi bulun ve açın. İmKB menüsünden **Ayarlar'ın**altında **Özel bağlayıcılar** > **Ekle'yi**seçin.

   ![Özel bağlayıcı oluşturma](./media/add-artifacts-integration-service-environment-ise/add-custom-connector-to-ise.png)

1. Özel bağlayıcınız için kullanılacak ad, Azure aboneliği ve Azure kaynak grubunu (yeni veya mevcut) sağlayın.

1. **Tümleştirme hizmet ortamları** bölümünün altındaki **Konum** listesinden, mantık uygulamalarınızın kullandığı Aynı İmKB'yi seçin ve örneğin **Oluştur'u**seçin:

   ![Entegrasyon hizmeti ortamını seçin](./media/add-artifacts-integration-service-environment-ise/create-custom-connector-integration-service-environment.png)

1. Yeni özel bağlayıcınızı seçin ve ardından **Edit'i**seçin, örneğin:

   ![Özel bağlayıcıyı seçin ve edin](./media/add-artifacts-integration-service-environment-ise/edit-custom-connectors.png)

1. [OpenAPI tanımı](https://docs.microsoft.com/connectors/custom-connectors/define-openapi-definition#import-the-openapi-definition) ndan veya SOAP'dan konektörünü her zamanki gibi [oluşturarak](https://docs.microsoft.com/connectors/custom-connectors/create-register-logic-apps-soap-connector#2-define-your-connector)devam edin.

1. İmKB'nizde özel konektörleri yönetmek için [bkz.](../logic-apps/ise-manage-integration-service-environment.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md)
