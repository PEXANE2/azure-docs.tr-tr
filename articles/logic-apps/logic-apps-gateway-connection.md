---
title: Azure Logic Apps Şirket içindeki veri kaynaklarına erişin | Microsoft Docs
description: Şirket içi veri ağ geçidi oluşturarak Logic Apps 'ten şirket içi veri kaynaklarına bağlanma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 029dc8daaf456c155d46eefa699772882bdabee5
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982880"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Azure Logic Apps şirket içi veri kaynaklarına bağlanma

Mantıksal uygulamalarınızdan şirket içi veri kaynaklarına erişmek için Azure portal bir şirket içi veri ağ geçidi kaynağı oluşturun. Mantıksal uygulamalarınız daha sonra [Şirket içi bağlayıcıları](../logic-apps/logic-apps-gateway-install.md#supported-connections)kullanabilir. Bu makalede [, yerel bilgisayarınıza ağ geçidini indirip yükledikten](../logic-apps/logic-apps-gateway-install.md) *sonra* Azure ağ geçidi kaynağınızın nasıl oluşturulacağı gösterilmektedir. 

> [!TIP]
> Azure sanal ağlarına bağlanmak için bunun yerine bir [*tümleştirme hizmeti ortamı*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturmayı düşünün. 

Ağ geçidini diğer hizmetlerle kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Şirket içi veri ağ geçidini Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Şirket içi veri ağ geçidini Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Şirket içi veri ağ geçidini Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

## <a name="prerequisites"></a>Önkoşullar

* [Veri ağ geçidini zaten bir yerel bilgisayara indirdiniz ve yüklediniz](../logic-apps/logic-apps-gateway-install.md).

* Ağ Geçidi yüklemeniz, Azure 'daki bir ağ geçidi kaynağıyla zaten ilişkili değil. Ağ Geçidi yüklemenizi yalnızca bir ağ geçidi kaynağına bağlayabilir, bu durum ağ geçidi kaynağını oluşturduğunuzda ve ağ geçidi yüklemenizi seçersiniz. Bu bağlama, ağ geçidi yüklemesini diğer kaynaklar için kullanılamaz hale getirir.

* Azure portal oturum açtığınızda ve ağ geçidi kaynağını oluşturduğunuzda, Şirket [içi veri ağ geçidini](../logic-apps/logic-apps-gateway-install.md#requirements) yüklemek için kullanılan aynı [Azure aboneliğiyle](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access) birlikte, daha önce kullanılan oturum açma hesabını kullandığınızdan emin olun. geçidinde. Henüz bir Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Azure portal ağ geçidi kaynağını oluşturmak ve korumak için, [Windows hizmet hesabınızın](../logic-apps/logic-apps-gateway-install.md#windows-service-account) en az **katkıda bulunan** izinleri olması gerekir. Şirket içi veri ağ geçidi bir Windows hizmeti olarak çalışır ve Windows hizmeti oturum açma kimlik bilgileri `NT SERVICE\PBIEgwService` için kullanılmak üzere ayarlanır. 

  > [!NOTE]
  > Windows hizmet hesabı, şirket içi veri kaynaklarına bağlanmak için kullanılan hesaptan ve bulut hizmetlerinde oturum açmak için kullanılan Azure iş veya okul hesabından farklıdır.

## <a name="download-and-install-gateway"></a>Ağ geçidini indirme ve yükleme

Bu makaledeki adımlara devam edebilmeniz için önce ağ geçidinizin yerel bir bilgisayarda zaten yüklü olduğundan emin olun.
Henüz yapmadıysanız, Şirket [içi veri ağ geçidini indirme ve yükleme](../logic-apps/logic-apps-gateway-install.md)adımlarını izleyin. 

<a name="create-gateway-resource"></a>

## <a name="create-azure-resource-for-gateway"></a>Ağ Geçidi için Azure kaynağı oluşturma

Ağ geçidini yerel bir bilgisayara yükledikten sonra, ağ geçidiniz için bir Azure kaynağı oluşturabilirsiniz. Bu adım Ayrıca ağ geçidi kaynağınızı Azure aboneliğinizle ilişkilendirir.

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> oturum açın. Ağ geçidini yüklemek için kullanılan Azure iş veya okul e-posta adresini kullandığınızdan emin olun.

2. Ana Azure menüsünde, **kaynak** > 
**tümleştirmesi** > Şirket**içi veri ağ geçidi**oluştur ' u seçin.

   !["Şirket içi veri ağ geçidi" ni bulun](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. **Bağlantı ağ geçidi oluştur** sayfasında, ağ geçidi kaynağınız için şu bilgileri sağlayın:

   | Özellik | Açıklama | 
   |----------|-------------|
   | **Kaynak Adı** | Yalnızca harf, sayı`-`, kısa çizgi (), alt çizgi (`_`), parantez (`(`, `)`) ve nokta (`.`) içerebilen ağ geçidi kaynak adınız. | 
   | **Abonelik** | Azure aboneliğinizin adı, mantıksal uygulamanızla aynı abonelik olmalıdır. Varsayılan abonelik, oturum açmak için kullandığınız Azure hesabını temel alır. | 
   | **Kaynak grubu** | İlgili kaynakları düzenlemek için [Azure Kaynak grubunun](../azure-resource-manager/resource-group-overview.md) adı | 
   | **Location** | Azure, [ağ geçidi yüklemesi](../logic-apps/logic-apps-gateway-install.md)sırasında bu konumu ağ geçidi bulutu hizmeti için seçilen bölge ile kısıtlar. <p>**Not**: Bu ağ geçidi kaynak konumunun ağ geçidi bulut hizmeti konumuyla eşleştiğinden emin olun. Aksi halde, bir sonraki adımda seçim yapmanız için ağ geçidi yüklemeniz yüklü ağ geçitleri listesinde görünmeyebilir. Ağ Geçidi kaynağınız ve mantıksal uygulamanız için farklı bölgeler kullanabilirsiniz. | 
   | **Yükleme adı** | Ağ Geçidi yüklemeniz zaten seçili değilse, daha önce yüklediğiniz ağ geçidini seçin. | 
   | | | 

   Örnek aşağıda verilmiştir:

   ![Şirket içi veri ağ geçidinizi oluşturma ayrıntılarını sağlayın](./media/logic-apps-gateway-connection/createblade.png)

4. Ağ Geçidi kaynağını Azure panonuza eklemek için **panoya sabitle**' yi seçin. İşiniz bittiğinde **Oluştur**’u seçin.

   İstediğiniz zaman ağ geçidinizi bulmak veya görüntülemek için, ana Azure menüsünden **tüm hizmetler**' i seçin. 
   Arama kutusuna "Şirket içi veri ağ geçitleri" yazın ve şirket **Içi veri ağ geçitleri**' ni seçin.

   !["Şirket içi veri ağ geçitleri" bulun](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Şirket içi verilere bağlanma

Ağ Geçidi kaynağınızı oluşturduktan ve Azure aboneliğinizi bu kaynakla ilişkilendirdikten sonra, ağ geçidini kullanarak mantıksal uygulamanız ve şirket içi veri kaynağınız arasında bir bağlantı oluşturabilirsiniz.

1. Azure portal mantıksal uygulama tasarımcısında mantıksal uygulamanızı oluşturun veya açın.

2. Şirket içi bağlantıları destekleyen bir bağlayıcı ekleyin, örneğin **SQL Server**.

3. Şimdi bağlantınızı ayarlayın:

   1. Şirket **içi veri ağ geçidi üzerinden Bağlan '** ı seçin. 

   2. **Ağ geçitleri**için, daha önce oluşturduğunuz ağ geçidi kaynağını seçin. 

      Ağ Geçidi bağlantı konumunuz mantıksal uygulamanızla aynı bölgede bulunmalı, ancak farklı bir bölgede bir ağ geçidi seçebilirsiniz.

   3. Benzersiz bir bağlantı adı ve diğer gerekli bilgileri sağlayın. 

      Benzersiz bağlantı adı, özellikle birden fazla bağlantı oluşturduğunuzda bu bağlantıyı daha sonra kolayca tanımlamanızı sağlar. Uygulanabiliyorsa, Kullanıcı adınız için tam etki alanını da dahil edin.
   
      Örnek aşağıda verilmiştir:

      ![Mantıksal uygulama ve veri ağ geçidi arasında bağlantı oluşturma](./media/logic-apps-gateway-connection/blankconnection.png)

   4. İşiniz bittiğinde **Oluştur**’u seçin. 

Ağ Geçidi bağlantınız artık mantıksal uygulamanızın kullanması için hazırdır.

## <a name="edit-connection"></a>Bağlantıyı Düzenle

Mantıksal uygulamanız için bir ağ geçidi bağlantısı oluşturduktan sonra, belirli bir bağlantının ayarlarını daha sonra güncelleştirmek isteyebilirsiniz.

1. Ağ Geçidi bağlantınızı bulun:

   * Yalnızca mantıksal uygulamanızın tüm API bağlantılarını bulmak için, mantıksal uygulamanızın menüsünde, **geliştirme araçları**' nın altında **API bağlantıları**' nı seçin. 
   
     ![Mantıksal uygulamanıza gidin, "API bağlantıları" nı seçin](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Azure aboneliğinizle ilişkili tüm API bağlantılarını bulmak için: 

     * Ana Azure menüsünden **tüm hizmetler** > **Web** > **API bağlantıları**' na gidin. 
     * Ya da, ana Azure menüsünden **tüm kaynaklar**' a gidin.

2. İstediğiniz ağ geçidi bağlantısını seçin ve ardından **API bağlantısını Düzenle**' yi seçin.

   > [!TIP]
   > Güncelleştirmelerinizin etkisi yoksa, [ağ geçidi Windows hizmetini durdurup yeniden başlatmayı](./logic-apps-gateway-install.md#restart-gateway)deneyin.

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Ağ Geçidi kaynağını Sil

Farklı bir ağ geçidi kaynağı oluşturmak, ağ geçidinizi farklı bir kaynakla ilişkilendirmek veya ağ geçidi kaynağını kaldırmak için ağ geçidi yüklemesini etkilemeden ağ geçidi kaynağını silebilirsiniz. 

1. Ana Azure menüsünden **tüm kaynaklar**' a gidin. 

2. Ağ Geçidi kaynağınızı bulun ve seçin.

3. Henüz seçili değilse, ağ geçidi kaynak menüsünde Şirket **Içi veri ağ geçidi**' ni seçin. 

4. Kaynak araç çubuğunda **Sil**' i seçin.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamanızı güvenli hale getirme](./logic-apps-securing-a-logic-app.md)
* [Logic Apps için ortak örnekler ve senaryolar](./logic-apps-examples-and-scenarios.md)
