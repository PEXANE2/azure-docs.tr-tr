---
title: Azure Logic Apps Şirket içindeki veri kaynaklarına erişin
description: Şirket içi veri ağ geçidi oluşturarak Logic Apps 'ten şirket içi veri kaynaklarına bağlanma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 07/01/2019
ms.openlocfilehash: 65c1d427939dc39aebece24b923bc4ebfbf136bb
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861057"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Azure Logic Apps şirket içi veri kaynaklarına bağlanma

Mantıksal uygulamalarınızdan şirket içi veri kaynaklarına erişmek için Azure portal bir şirket içi veri ağ geçidi kaynağı oluşturun. Mantıksal uygulamalarınız daha sonra [Şirket içi bağlayıcıları](../logic-apps/logic-apps-gateway-install.md#supported-connections)kullanabilir. Bu makalede [, yerel bilgisayarınıza ağ geçidini indirip yükledikten](../logic-apps/logic-apps-gateway-install.md) *sonra* Azure ağ geçidi kaynağınızın nasıl oluşturulacağı gösterilmektedir. Ağ geçidinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [ağ geçidinin nasıl çalıştığı](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service).

> [!TIP]
> Azure sanal ağlarına bağlanmak için bunun yerine bir [*tümleştirme hizmeti ortamı*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturmayı düşünün. 

Ağ geçidini diğer hizmetlerle kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Şirket içi veri ağ geçidini Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Şirket içi veri ağ geçidini Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Şirket içi veri ağ geçidini Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

## <a name="prerequisites"></a>Önkoşullar

* Şirket [içi veri ağ geçidini zaten yerel bir bilgisayara yüklediniz](../logic-apps/logic-apps-gateway-install.md).

* Şirket içi veri ağ geçidini yüklerken kullandığınız [Azure hesabına ve Azure aboneliğine](../logic-apps/logic-apps-gateway-install.md#requirements) sahipsiniz.

* Daha önce ağ geçidi yüklemenizi Azure 'daki başka bir ağ geçidi kaynağına bağlamadınız.

  Bir ağ geçidi kaynağı oluşturduğunuzda, ağ geçidi kaynağınız ile ilişkilendirilecek bir ağ geçidi yüklemesi seçersiniz. Zaten bağlı ağ geçidi yüklemesi, ağ geçidi kaynaklarını oluştururken seçebileceğiniz şekilde kullanılamaz.

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>Azure ağ geçidi kaynağı oluşturma

Ağ geçidini yerel bir bilgisayara yükledikten sonra, ağ geçidiniz için Azure kaynağı oluşturun. 

1. Ağ geçidini yüklemek için kullanılan aynı Azure hesabıyla [Azure Portal](https://portal.azure.com) oturum açın.

1. Azure portal arama kutusuna "Şirket içi veri ağ geçidi" girin ve şirket **Içi veri ağ geçitleri**' ni seçin.

   !["Şirket içi veri ağ geçidi" ni bulun](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

1. **Şirket Içi veri ağ geçitleri**altında **Ekle**' yi seçin.

   ![Veri ağ geçidi ekle](./media/logic-apps-gateway-connection/add-gateway.png)

1. **Bağlantı ağ geçidi oluştur**altında, ağ geçidi kaynağınız için bu bilgileri sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

   | Özellik | Açıklama |
   |----------|-------------|
   | **Kaynak Adı** | Yalnızca harf, sayı`-`, kısa çizgi (), alt çizgi (`_`), parantez (`(`, `)`) ve nokta (`.`) içerebilen ağ geçidi kaynak adınız. |
   | **Abonelik** | Azure aboneliğiniz, ağ geçidi yükleme ve mantıksal uygulama ile aynı olmalıdır. Varsayılan abonelik, oturum açmak için kullandığınız Azure hesabını temel alır. |
   | **Kaynak grubu** | Kullanmak istediğiniz [Azure Kaynak grubu](../azure-resource-manager/resource-group-overview.md) |
   | **Location** | Ağ [geçidi yüklemesi](../logic-apps/logic-apps-gateway-install.md)sırasında ağ geçidi bulutu hizmeti için seçilen konumla aynı bölge. Aksi halde, seçtiğiniz ağ geçidi yüklemeniz **yükleme adı** listesinde görünmez. Mantıksal uygulama konumunuz, ağ geçidi kaynak konumunuzda farklılık gösterebilir. |
   | **Yükleme adı** | Ağ Geçidi yüklemeniz zaten seçili değilse, daha önce yüklediğiniz ağ geçidini seçin. Daha önce bağlı ağ geçidi yüklemeleri, seçmeniz için bu listede görünmez. |
   |||

   Örnek aşağıda verilmiştir:

   ![Şirket içi veri ağ geçidinizi oluşturma ayrıntılarını sağlayın](./media/logic-apps-gateway-connection/gateway-details.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Şirket içi verilere bağlanma

Ağ Geçidi kaynağınızı oluşturduktan ve Azure aboneliğinizi bu kaynakla ilişkilendirdikten sonra, ağ geçidini kullanarak mantıksal uygulamanız ve şirket içi veri kaynağınız arasında bir bağlantı oluşturabilirsiniz.

1. Azure portal mantıksal uygulama tasarımcısında mantıksal uygulamanızı oluşturun veya açın.

1. Şirket içi bağlantıları destekleyen bir bağlayıcı ekleyin, örneğin **SQL Server**.

1. Şirket **içi veri ağ geçidi üzerinden Bağlan '** ı seçin. 

1. **Ağ geçitleri**için, oluşturduğunuz ağ geçidi kaynağını seçin.

   > [!NOTE]
   > Mantıksal uygulamanızın konumu ağ geçidi kaynağınızın konumundan farklı olabileceğinden, ağ geçitleri listesi diğer bölgelerdeki ağ geçidi kaynaklarını içerir.

1. Benzersiz bir bağlantı adı ve oluşturmak istediğiniz bağlantıya bağlı diğer gerekli bilgileri sağlayın.

   Benzersiz bir bağlantı adı, özellikle birden çok bağlantı oluşturuyorsanız bu bağlantıyı daha sonra kolayca bulmanıza yardımcı olur. Uygulanabiliyorsa, Kullanıcı adınız için tam etki alanını da dahil edin.
   
   Örnek aşağıda verilmiştir:

   ![Mantıksal uygulama ve veri ağ geçidi arasında bağlantı oluşturma](./media/logic-apps-gateway-connection/logic-app-gateway-connection.png)

1. İşiniz bittiğinde **Oluştur**’u seçin. 

Ağ Geçidi bağlantınız artık mantıksal uygulamanızın kullanması için hazırdır.

## <a name="edit-connection"></a>Bağlantıyı Düzenle

Bir ağ geçidi bağlantısının ayarlarını güncelleştirmek için bağlantınızı düzenleyebilirsiniz.

1. Yalnızca mantıksal uygulamanızın tüm API bağlantılarını bulmak için, mantıksal uygulamanızın menüsünde, **geliştirme araçları**' nın altında **API bağlantıları**' nı seçin.
   
   ![Mantıksal uygulama menünüzde "API bağlantıları" nı seçin](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

1. İstediğiniz ağ geçidi bağlantısını seçin ve ardından **API bağlantısını Düzenle**' yi seçin.

   > [!TIP]
   > Güncelleştirmelerinizin etkisi yoksa ağ geçidi yüklemeniz için [ağ geçidi Windows hizmeti hesabını durdurup yeniden başlatmayı](../logic-apps/logic-apps-gateway-install.md#restart-gateway) deneyin.

Azure aboneliğinizle ilişkili tüm API bağlantılarını bulmak için: 

* Ana Azure menüsünden **tüm hizmetler** > **Web** > **API bağlantıları**' na gidin.
* Ya da, ana Azure menüsünden **tüm kaynaklar**' a gidin. **Tür** filtresini **API bağlantısı**olarak ayarlayın.

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Ağ Geçidi kaynağını Sil

Farklı bir ağ geçidi kaynağı oluşturmak, ağ geçidi yüklemenizi farklı bir ağ geçidi kaynağına bağlamak veya ağ geçidi kaynağını kaldırmak için ağ geçidi yüklemesini etkilemeden ağ geçidi kaynağını silebilirsiniz. 

1. Ana Azure menüsünden **tüm kaynaklar**' ı seçin. Ağ Geçidi kaynağınızı bulun ve seçin.

1. Henüz seçili değilse, ağ geçidi kaynak menüsünde Şirket **Içi veri ağ geçidi**' ni seçin. Ağ Geçidi kaynağı araç çubuğunda **Sil**' i seçin.

   Örneğin:

   ![Ağ geçidini Sil](./media/logic-apps-gateway-connection/gateway-delete.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamanızı güvenli hale getirme](./logic-apps-securing-a-logic-app.md)
* [Logic Apps için ortak örnekler ve senaryolar](./logic-apps-examples-and-scenarios.md)
