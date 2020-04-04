---
title: Şirket içinde veri kaynaklarına erişin
description: Azure şirket içi veri ağ geçidi kaynağı oluşturarak Azure Logic Apps'tan şirket içi veri kaynaklarına bağlanma
services: logic-apps
ms.suite: integration
ms.reviewer: arthii, logicappspm
ms.topic: article
ms.date: 02/14/2020
ms.openlocfilehash: 096943ff796f9c12c7f8715cadce5c3085965d4d
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657133"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Azure Logic Apps'ten şirket içi veri kaynaklarına bağlanma

Mantıksal uygulamalarınızdan şirket içi veri kaynaklarına erişebilmeniz için, [şirket içi veri ağ *geçidini* yerel bir bilgisayara yükledikten](../logic-apps/logic-apps-gateway-install.md)sonra bir Azure kaynağı oluşturmanız gerekir. Daha sonra mantıksal uygulamalarınız bu Azure ağ geçidi kaynağını, Azure Mantık Uygulamaları için kullanılabilen [şirket içi bağlayıcılar](../connectors/apis-list.md#on-premises-connectors) tarafından sağlanan tetikleyicilerde ve eylemlerde kullanır.

Bu makalede, yerel bilgisayarınızda daha önce yüklenmiş bir ağ geçidi için Azure ağ geçidi [kaynağıoluşturmak](../logic-apps/logic-apps-gateway-install.md)için nasıl gösterir. Ağ geçidi hakkında daha fazla bilgi için [ağ geçidinin nasıl çalıştığını](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service)görün.

> [!TIP]
> Azure sanal ağlarına bağlanmak için bunun yerine bir [*tümleştirme hizmeti ortamı*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturmayı düşünün. 

Ağ geçidinin diğer hizmetlerle nasıl kullanılacağı hakkında bilgi için aşağıdaki makalelere bakın:

* [Microsoft Power Şirket içi veri ağ geçidini otomatikleştir](/power-automate/gateway-reference)
* [Microsoft Power BI şirket içi veri ağ geçidi](/power-bi/service-gateway-onprem)
* [Microsoft Power Apps şirket içi veri ağ geçidi](/powerapps/maker/canvas-apps/gateway-reference)
* [Azure Analiz Hizmetleri şirket içi veri ağ geçidi](../analysis-services/analysis-services-gateway.md)

<a name="supported-connections"></a>

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları

Azure Logic Apps'ta şirket içi veri ağ geçidi, bu veri kaynakları için [şirket içi bağlayıcıları](../connectors/apis-list.md#on-premises-connectors) destekler:

* BizTalk Server 2016
* Dosya Sistemi
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Oracle Veritabanı
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

Azure Logic Apps, veri ağ geçidi üzerinden okuma ve yazma işlemlerini destekler. Ancak, bu [işlemlerin yük boyutu nda sınırlamaları](https://docs.microsoft.com/data-integration/gateway/service-gateway-onprem#considerations)vardır. Ağ geçidinin kendisi ek maliyetlere tabi olmasa da, [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md) bu bağlayıcılar ve Azure Logic Apps'taki diğer işlemler için geçerlidir.

## <a name="prerequisites"></a>Ön koşullar

* Şirket [içi veri ağ geçidini zaten yerel bir bilgisayara yüklediniz.](../logic-apps/logic-apps-gateway-install.md)

* Bu veri ağ geçidini yüklerken kullanılan [aynı Azure hesabını ve aboneliğini](../logic-apps/logic-apps-gateway-install.md#requirements) kullanıyorsunuz. Bu Azure hesabı tek bir [Azure Etkin Dizin (Azure AD) kiracısına veya dizinine](../active-directory/fundamentals/active-directory-whatis.md#terminology)ait olmalıdır.

* Ağ geçidi yüklemeniz başka bir Azure ağ geçidi kaynağı tarafından zaten kaydedilmemiş ve talep edilmemiş.

  Azure portalında bir ağ geçidi kaynağı oluşturduğunuzda, ağ geçidi kaynağınıza ve yalnızca bu ağ geçidi kaynağına bağlantı veren bir ağ geçidi yüklemesi seçersiniz. Azure Logic Apps'ta şirket içi tetikleyiciler ve eylemler, şirket içi veri kaynaklarına bağlanmak için ağ geçidi kaynağını kullanır. Bu tetikleyiciler ve eylemlerde, Azure aboneliğinizi ve kullanmak istediğiniz ilişkili ağ geçidi kaynağını seçersiniz. Her ağ geçidi kaynağı yalnızca bir Azure hesabına bağlanan tek bir ağ geçidi yüklemesine bağlanır.

  > [!NOTE]
  > Azure portalında ağ geçidi kaynağını yalnızca ağ geçidi yöneticisi oluşturabilir. Şu anda, hizmet ilkeleri desteklenmiyor. 

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>Azure ağ geçidi kaynağı oluşturma

Ağ geçidini yerel bir bilgisayara yükledikten sonra, ağ geçidiniz için Azure kaynağını oluşturun.

1. Ağ geçidini yüklemek için kullanılan aynı Azure hesabıyla [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalı arama kutusuna "şirket içi veri ağ geçidi" girin ve **şirket içi Veri Ağ Geçitleri'ni**seçin.

   !["Şirket içi veri ağ geçidini" bulma](./media/logic-apps-gateway-connection/search-for-on-premises-data-gateway.png)

1. **Şirket Içi Veri Ağ Geçitleri**altında **Ekle'yi**seçin.

   ![Veri ağ geçidi için yeni Azure kaynağı ekleme](./media/logic-apps-gateway-connection/add-azure-data-gateway-resource.png)

1. **Bağlantı ağ geçidi oluştur**altında, ağ geçidi kaynağınız için bu bilgileri sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

   | Özellik | Açıklama |
   |----------|-------------|
   | **Kaynak Adı** | Ağ geçidi kaynağınız için yalnızca harfler, sayılar, tireler (`-``_`), alt tümleçler ( ), parantez (`(`, `)`), ()`.`veya dönemleri içeren bir ad sağlayın. |
   | **Abonelik** | Ağ geçidi yüklemesi için kullanılan Azure hesabı için Azure aboneliğini seçin. Varsayılan abonelik, oturum açmak için kullandığınız Azure hesabını temel alır. |
   | **Kaynak grubu** | Kullanmak istediğiniz [Azure kaynak grubu](../azure-resource-manager/management/overview.md) |
   | **Konum** | [Ağ geçidi yüklemesi](../logic-apps/logic-apps-gateway-install.md)sırasında ağ geçidi bulut hizmeti için seçilen aynı bölge veya konum. Aksi takdirde, ağ geçidi yüklemeniz **Yükleme Adı** listesinde görünmez. Mantık uygulama konumunuz ağ geçidi kaynak konumunuzdan farklı olabilir. |
   | **Kurulum Adı** | Yalnızca bu koşullar karşılandığında listede görünen bir ağ geçidi yüklemesi seçin: <p><p>- Ağ geçidi yüklemesi, oluşturmak istediğiniz ağ geçidi kaynağıyla aynı bölgeyi kullanır. <br>- Ağ geçidi yüklemesi başka bir Azure ağ geçidi kaynağına bağlı değildir. <br>- Ağ geçidi yüklemesi, ağ geçidi kaynağını oluşturmak için kullandığınız aynı Azure hesabına bağlıdır. <br>- Azure hesabınız tek bir [Azure Etkin Dizin (Azure AD) kiracısına veya dizinine](../active-directory/fundamentals/active-directory-whatis.md#terminology) aittir ve ağ geçidi yüklemesi için kullanılan hesapla aynıdır. <p><p>Daha fazla bilgi için sık [sorulan sorular](#faq) bölümüne bakın. |
   |||

   Ağ geçidi kaynağınızla aynı bölgede bulunan ve aynı Azure hesabına bağlı bir ağ geçidi yüklemesini gösteren bir örnek aşağıda verilmiştir:

   ![Veri ağ geçidi kaynağı oluşturmak için ayrıntılar sağlama](./media/logic-apps-gateway-connection/on-premises-data-gateway-create-connection.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Şirket içi verilere bağlanma

Ağ geçidi kaynağınızı oluşturduktan ve Azure aboneliğinizi bu kaynakla ilişkilendirdikten sonra, artık ağ geçidini kullanarak mantık uygulamanız ile şirket içi veri kaynağınız arasında bir bağlantı oluşturabilirsiniz.

1. Azure portalında Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı oluşturun veya açın.

1. Sql **Server**gibi şirket içi bağlantıları destekleyen bir bağlayıcı ekleyin.

1. **Şirket içi veri ağ geçidi üzerinden Bağlan'ı**seçin.

1. **Ağ Geçitleri**altında , **Abonelikler** listesinden, istediğiniz ağ geçidi kaynağına sahip Azure aboneliğinizi seçin.

1. Seçili aboneliğinizdeki kullanılabilir ağ geçidi kaynaklarını gösteren **Bağlantı Ağ Geçidi** listesinden, istediğiniz ağ geçidi kaynağını seçin. Her ağ geçidi kaynağı tek bir ağ geçidi yüklemesine bağlıdır.

   > [!NOTE]
   > Ağ geçidi listesi, mantık uygulamanızın konumu ağ geçidi kaynağınızın konumundan farklı olabileceğinden, diğer bölgelerdeki ağ geçidi kaynaklarını içerir. 

1. Oluşturmak istediğiniz bağlantıya bağlı olarak benzersiz bir bağlantı adı ve diğer gerekli bilgiler sağlayın.

   Benzersiz bir bağlantı adı, özellikle birden çok bağlantı oluşturuyorsanız, bu bağlantıyı daha sonra kolayca bulmanıza yardımcı olur. Mümkünse, kullanıcı adınız için nitelikli etki alanını da ekleyin.

   Örnek aşağıda verilmiştir:

   ![Mantık uygulaması ve veri ağ geçidi arasında bağlantı oluşturma](./media/logic-apps-gateway-connection/logic-app-gateway-connection.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

Ağ geçidi bağlantınız artık mantık uygulamanızın kullanımına hazır.

## <a name="edit-connection"></a>Bağlantıyı ede

Ağ geçidi bağlantısı ayarlarını güncelleştirmek için bağlantınızı değiştirebilirsiniz.

1. Sadece mantık uygulamanız için tüm API bağlantılarını, mantık uygulamanızın menüsünde, **Geliştirme Araçları**altında, **API bağlantılarını**seçin.

   ![Mantık uygulaması menüsünde "API Bağlantıları" seçeneğini belirleyin](./media/logic-apps-gateway-connection/logic-app-api-connections.png)

1. İstediğiniz ağ geçidi bağlantısını seçin ve ardından **API bağlantısını edit'i**seçin.

   > [!TIP]
   > Güncelleştirmeleriniz etkin değilse, ağ geçidi yüklemeniz için [windows hizmet hesabını durdurmayı ve yeniden başlatmayı](../logic-apps/logic-apps-gateway-install.md#restart-gateway) deneyin.

Azure aboneliğinizle ilişkili tüm API bağlantılarını bulmak için:

* Azure portalı menüsünden **Tüm hizmetler** > **Web** > **API Bağlantıları'nı**seçin.
* Veya Azure portalı menüsünden **Tüm kaynakları**seçin. **Tür** filtresini **API Bağlantısıolarak**ayarlayın.

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Ağ geçidi kaynağını silme

Farklı bir ağ geçidi kaynağı oluşturmak, ağ geçidi yüklemenizi farklı bir ağ geçidi kaynağına bağlamak veya ağ geçidi kaynağını kaldırmak için ağ geçidi kaynağını etkilemeden ağ geçidi kaynağını silebilirsiniz.

1. Azure portalı menüsünden **Tüm kaynakları**seçin veya herhangi bir sayfadaki **Tüm kaynakları** arayın ve seçin. Ağ geçidi kaynağınızı bulun ve seçin.

1. Ağ geçidi kaynak menüsünde zaten seçilmemişse, **şirket içi Veri Ağ Geçidi'ni**seçin. Ağ geçidi kaynak araç çubuğunda **Sil'i**seçin.

   Örnek:

   ![Azure'da ağ geçidi kaynağını silme](./media/logic-apps-gateway-connection/delete-on-premises-data-gateway.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S**: Azure'da ağ geçidi kaynağımı oluştururken ağ geçidi yüklemem neden görünmüyor? <br/>
**C**: Bu sorun şu nedenlerle olabilir:

* Azure hesabınız, yerel bilgisayardaki ağ geçidi yüklemesine bağlı olan hesapla aynı olmalıdır. Azure portalında ağ geçidi yüklemesiyle bağlantılı aynı kimliğe sahip oturum açıp olmadığınızı kontrol edin. Ayrıca, Azure hesabınızın tek bir [Azure AD kiracısına veya dizinine](../active-directory/fundamentals/active-directory-whatis.md#terminology) ait olduğundan ve ağ geçidi yüklemesi sırasında kullanılan aynı Azure AD kiracısına veya dizinine ayarlandığını unutmayın.

* Ağ geçidi kaynağınız ve ağ geçidi yüklemeniz aynı bölgeyi kullanmak zorunda. Ancak, mantık uygulama konumunuz ağ geçidi kaynak konumunuzdan farklı olabilir.

* Ağ geçidi yüklemeniz zaten kaydedilmiş ve başka bir ağ geçidi kaynağı tarafından talep edilebistir. Bu yüklemeler **Yükleme Adı** listesinde görünmez. Azure portalındaki ağ geçidi kayıtlarınızı incelemek için, *tüm* Azure aboneliklerinizde **şirket içi Veri Ağ Geçitleri** türüne sahip tüm Azure kaynaklarınızı bulun. Ağ geçidi yüklemesini diğer ağ geçidi kaynağından çıkarmak için [bkz.](#change-delete-gateway-resource)

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulamanızı güvenli hale getirme](./logic-apps-securing-a-logic-app.md)
* [Mantık uygulamaları için yaygın örnekler ve senaryolar](./logic-apps-examples-and-scenarios.md)
