---
title: Bir İmKB ile Azure sanal ağlarına bağlanma
description: Azure Mantık Uygulamaları'ndan Azure sanal ağlarına (VNET) erişebilen bir entegrasyon hizmeti ortamı (ISE) oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 6683c1b78b0e7ecba162026708c83843e2c08180
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478879"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Bir tümleştirme hizmeti ortamı (ISE) kullanarak Azure Logic Apps'tan Azure sanal ağlarına bağlanma

Mantık uygulamalarınızın ve tümleştirme hesaplarınızın bir [Azure sanal ağına](../virtual-network/virtual-networks-overview.md)erişmeye ihtiyaç duyduğu senaryolarda bir [ *tümleştirme hizmeti ortamı* (Ise)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturun. Ise, "genel" çok kiracılı Logic Apps hizmetinden ayrı tutulan özel depolama ve diğer kaynakları kullanan yalıtılmış bir ortamdır. Bu ayrım, diğer Azure kiracılarının uygulamalarınızın performansı üzerindeki etkisini de azaltır. Bir ISE aynı zamanda kendi statik IP adreslerinizi de sağlar. Bu IP adresleri, genel, çok kiracılı hizmetteki mantık uygulamaları tarafından paylaşılan statik IP adreslerinden ayrıdır.

Bir İmKB oluşturduğunuzda, Azure Azure sanal ağınıza bu SKS *enjekte* eder ve ardından Logic Apps hizmetini sanal ağınıza dağıtır. Bir mantık uygulaması veya entegrasyon hesabı oluşturduğunuzda, Konum olarak İmKB'nizi seçin. Mantık uygulamanız veya tümleştirme hesabınız sanal ağınızdaki sanal makineler (VM'ler), sunucular, sistemler ve hizmetler gibi kaynaklara doğrudan erişebilir.

![Entegrasyon hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> Mantıksal uygulamaların ve tümleştirme hesaplarının Bir İmKB'de birlikte çalışabilmesi için her ikisinin de konumlarıyla *aynı İmKB'yi* kullanması gerekir.

Bir Ise, çalışma süresi, depolama bekletme, iş ortası, HTTP isteği ve yanıt zaman zaman ları, ileti boyutları ve özel bağlayıcı istekleriyle ilgili sınırları artırmıştır. Daha fazla bilgi için [Azure Mantık Uygulamaları için Sınırlar ve yapılandırma ya](../logic-apps/logic-apps-limits-and-config.md)da yapılandırma ya da ISS'ler hakkında daha fazla bilgi edinmek için Azure [Logic Apps'tan Azure Sanal Ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)'e bakın.

Bu makalede, Azure portalını kullanarak bu görevleri nasıl tamamlayabileceğinizgösterilmektedir:

* İmKB'niz için erişimi etkinleştirin.
* İmKB'inizi oluşturun.
* İmKB'nize ekstra kapasite ekleyin.

Ayrıca, örnek Azure Kaynak [Yöneticisi hızlı başlatma şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment) kullanarak veya müşteri tarafından yönetilen anahtarları ayarlama da dahil olmak üzere Logic Apps REST API'sini kullanarak bir İmKB oluşturabilirsiniz:

* [Logic Apps REST API'yi kullanarak bir entegrasyon hizmet ortamı (Ise) oluşturun](../logic-apps/create-integration-service-environment-rest-api.md)
* [ISEs için istirahatte verileri şifrelemek için müşteri tarafından yönetilen anahtarları ayarlama](../logic-apps/customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

  > [!IMPORTANT]
  > Logic uygulamaları, yerleşik tetikleyiciler, yerleşik eylemler ve İmKB'nizde çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. ISE'ler için fiyatlandırma ve faturalandırmanın nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](../logic-apps/logic-apps-pricing.md)bakın.

* Azure [sanal ağı.](../virtual-network/virtual-networks-overview.md) Sanal ağınız yoksa, [Azure sanal ağının](../virtual-network/quick-create-portal.md)nasıl oluşturulabildiğini öğrenin.

  * Sanal ağınızın İmKB'nizde kaynak oluşturmak ve dağıtmak için dört *boş* alt ağına sahip olması gerekir. Her alt ağ, İmKB'nizde kullanılan farklı bir Logic Apps bileşenini destekler. Bu alt ağları önceden oluşturabilir veya aynı anda alt ağlar oluşturabileceğiniz İmKB'nizi oluşturana kadar bekleyebilirsiniz. [Alt ağ gereksinimleri](#create-subnet)hakkında daha fazla bilgi edinin.

  * Alt ağ adları ya alfabetik bir karakter ya da bir alt puan `<` `>`ile `%` `&`başlamak `\\`gerekir ve bu karakterleri kullanamazsınız: , , , , `?`. `/` 
  
  * İmKB'yi Azure Kaynak Yöneticisi şablonu aracılığıyla dağıtmak istiyorsanız, öncelikle boş bir alt ağı Microsoft.Logic/integrationServiceEnvironment'a devrettiğinizden emin olun. Azure portalı üzerinden dağıtırken bu temsilciliği yapmanız gerekmez.

  * İmKB'nizin doğru çalışabilmesi ve erişilebilir kalabilmesi için sanal ağınızın [İmKB'inize erişim sağladığından](#enable-access) emin olun.

  * Bağlantı sağlayıcısı tarafından kolaylaştırılan Microsoft bulut hizmetlerine özel bir bağlantı sağlayan [ExpressRoute](../expressroute/expressroute-introduction.md)kullanıyorsanız, aşağıdaki rotaya sahip [bir rota tablosu oluşturmanız](../virtual-network/manage-route-table.md) ve bu tabloyu İmKB tarafından kullanılan her alt ağa bağlamanız gerekir:

    **Adı**: <*rota adı*><br>
    **Adres öneki**: 0.0.0.0/0<br>
    **Sonraki hop**: Internet

* Azure sanal ağınız için özel DNS sunucuları kullanmak istiyorsanız, İmKB'nizi sanal ağınıza dağıtmadan önce [aşağıdaki adımları izleyerek bu sunucuları ayarlayın.](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) DNS sunucu ayarlarını yönetme hakkında daha fazla bilgi için [bkz.](../virtual-network/manage-virtual-network.md#change-dns-servers)

  > [!NOTE]
  > DNS sunucu veya DNS sunucu ayarlarınızı değiştirirseniz, İmKB'nin bu değişiklikleri alabilmeleri için İmKB'nizi yeniden başlatmanız gerekir. Daha fazla bilgi için [BK.n'ınızı yeniden başlatın.](../logic-apps/ise-manage-integration-service-environment.md#restart-ISE)

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>ISE erişimini etkinleştirin

Azure sanal ağı olan bir İmKB kullandığınızda, sık karşılaşılan bir kurulum sorunu bir veya daha fazla engellenmiş bağlantı noktası olmasıdır. İmKB ve hedef sistemleriniz arasında bağlantı oluşturmak için kullandığınız bağlayıcıların kendi bağlantı noktası gereksinimleri de olabilir. Örneğin, FTP konektörünü kullanarak bir FTP sistemiyle iletişim kuruyorsanız, FTP sisteminizde kullandığınız bağlantı noktasının komut göndermek için 21 no'lu bağlantı noktasının kullanılabilir olması gerekir.

İmKB'nizin erişilebilir olduğundan ve bu İmKB'deki mantık uygulamalarının sanal ağınızdaki her alt ağ üzerinden iletişim kurabildiğinden emin olmak [için, bu tabloda açıklanan her alt ağ için açıklanan bağlantı noktalarını açın.](#network-ports-for-ise) Gerekli bağlantı noktaları kullanılamıyorsa, İmKB'niz düzgün çalışmaz.

* IP kısıtlamaları olan diğer uç noktalara erişmeniz gereken birden fazla İmKB örneğiniz varsa, sanal ağınıza bir [Azure Güvenlik Duvarı](../firewall/overview.md) veya ağ sanal [cihazı](../virtual-network/virtual-networks-overview.md#filter-network-traffic) dağıtın ve giden trafiği bu güvenlik duvarı veya ağ sanal cihazı üzerinden yönlendirin. Daha sonra, sanal ağınızdaki tüm İmKB örneklerinin hedef sistemlerle iletişim kurmak için kullanabileceği [tek, giden, genel, statik ve öngörülebilir bir IP adresi](connect-virtual-network-vnet-set-up-single-ip-address.md) ayarlayabilirsiniz. Bu şekilde, her ise için bu hedef sistemlerde ek güvenlik duvarı açıklıkları ayarlamanız gerekmez.

   > [!NOTE]
   > Senaryonuz erişim gerektiren IP adreslerinin sayısını sınırlamayı gerektirdiğinde bu yaklaşımı tek bir İmKB için kullanabilirsiniz. Güvenlik duvarı veya sanal ağ cihazı için ek maliyetlerin senaryonuz için anlamlı olup olmadığını göz önünde bulundurun. [Azure Güvenlik Duvarı fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-firewall/)hakkında daha fazla bilgi edinin.

* Herhangi bir kısıtlama olmaksızın yeni bir Azure sanal ağı ve alt ağ oluşturduysanız, alt ağlardaki trafiği denetlemek için sanal ağınızda [ağ güvenlik grupları (NSG' ler)](../virtual-network/security-overview.md#network-security-groups) oluşturmanız gerekmez.

* Varolan bir sanal ağda, [ağ trafiğini alt ağlar](../virtual-network/tutorial-filter-network-traffic.md)arasında filtreleyerek *isteğe bağlı olarak* NSG'leri ayarlayabilirsiniz. Bu rotaya gitmek istiyorsanız veya zaten NSG kullanıyorsanız, NSG'lerin bulunduğu sanal ağdaki [bu tablodaki bağlantı noktalarını açtığından](#network-ports-for-ise) veya NSG'leri kurmak istediğinizden emin olun.

  > [!NOTE]
  > [NSG güvenlik kurallarını](../virtual-network/security-overview.md#security-rules)kullanıyorsanız, hem TCP *hem de* UDP protokollerini kullanmanız gerekir. NSG güvenlik kuralları, bu bağlantı noktalarına erişmesi gereken IP adresleri için açmanız gereken bağlantı noktalarını açıklar. Bu uç noktalar arasında bulunan güvenlik duvarlarının, yönlendiricilerin veya diğer öğelerin de bu bağlantı noktalarının bu IP adresleri için erişilebilir olmasını dığından emin olun.

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>İmKB'niz tarafından kullanılan ağ bağlantı noktaları

Bu tablo, İmKB'nizin kullandığı Azure sanal ağınızdaki bağlantı noktalarını ve bu bağlantı noktalarının nerede kullanıldığını açıklar. Güvenlik kuralları oluştururken karmaşıklığı azaltmaya yardımcı olmak için, tablodaki [hizmet etiketleri](../virtual-network/service-tags-overview.md) belirli bir Azure hizmeti için IP adresi önekleri gruplarını temsil eder.

> [!IMPORTANT]
> Kaynak bağlantı noktaları geçicidir, bu nedenle tüm `*` kurallar için ayarladığınızdan emin olun. Belirtildiği yerde, iç İmKB ve dış [İmKB, İmKB oluşturmada seçilen bitiş noktasına](connect-virtual-network-vnet-isolated-environment.md#create-environment)bakın. Daha fazla bilgi için [Endpoint erişimine](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access)bakın. 

| Amaç | Yön | Hedef bağlantı noktaları | Kaynak servis etiketi | Hedef hizmet etiketi | Notlar |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| Sanal ağınızda intersubnet iletişimi | Gelen & Giden | * | İmKB alt ağlarına sahip sanal ağın adres alanı | İmKB alt ağlarına sahip sanal ağın adres alanı | Trafiğin sanal ağınızdaki alt ağlar *arasında* akması için gereklidir. <p><p>**Önemli**: Trafiğin her alt ağdaki *bileşenler* arasında akması için, her alt ağdaki tüm bağlantı noktalarını açtığından emin olun. |
| Mantık uygulamanızla iletişim | Gelen | 443 | İç İmKB: <br>VirtualNetwork <p><p>Dış İmKB: <br>Internet <br>(bkz. **Notlar** sütunu) | VirtualNetwork | **Internet** hizmeti etiketini kullanmak yerine, mantık uygulamanızda herhangi bir isteği tetikleyen veya webhooks çağıran bilgisayarın veya hizmetin kaynak IP adresini belirtebilirsiniz. <p><p>**Önemli**: Bu bağlantı noktasının kapatılması veya engellenmesi, istek tetikleyicileri olan mantıksal uygulamalara HTTP çağrılarının engellenmesini sağlar. |
| Mantık uygulaması çalışma geçmişi | Gelen | 443 | İç İmKB: <br>VirtualNetwork <p><p>Dış İmKB: <br>Internet <br>(bkz. **Notlar** sütunu) | VirtualNetwork | **Internet** hizmeti etiketini kullanmak yerine, mantık uygulamanızın çalışma geçmişini görüntülemek istediğiniz yerden bilgisayarın veya hizmetin kaynak IP adresini belirtebilirsiniz. <p><p>**Önemli**: Bu bağlantı noktasını kapatmak veya engellemek çalışma geçmişini görüntülemenizi engellemese de, bu çalışma geçmişindeki her adımiçin giriş ve çıktıları görüntüleyemezsiniz. |
| Logic Apps Designer - dinamik özellikler | Gelen | 454 | LogicAppsManagement | VirtualNetwork | İstekler, Logic Apps'ın o bölge için [gelen](../logic-apps/logic-apps-limits-and-config.md#inbound) IP adreslerine erişim bitiş noktası ndan gelir. |
| Bağlayıcı dağıtımı | Gelen | 454 | AzureBağlayıcıları | VirtualNetwork | Bağlayıcıları dağıtmak ve güncelleştirmek için gereklidir. Bu bağlantı noktasının kapatılması veya engellenmesi Ise dağıtımlarının başarısız olmasını sağlar ve bağlayıcı güncelleştirmelerini veya düzeltmelerini engeller. |
| Ağ sistem durumu denetimi | Gelen | 454 | LogicApps | VirtualNetwork | İstekler, o bölge için hem [gelen](../logic-apps/logic-apps-limits-and-config.md#inbound) hem de [giden](../logic-apps/logic-apps-limits-and-config.md#outbound) IP adresleri için Logic Apps erişim bitiş noktasından gelir. |
| Uygulama Hizmet Yönetimi bağımlılığı | Gelen | 454, 455 | AppServiceManagement | VirtualNetwork | |
| Azure Trafik Yöneticisi'nden İletişim | Gelen | Dahili İmKB: 454 <p><p>Dış İmKB: 443 | AzureTrafficManager | VirtualNetwork | |
| API Yönetimi - yönetim bitiş noktası | Gelen | 3443 | APIManagement | VirtualNetwork | |
| Bağlayıcı ilke dağıtımı | Gelen | 3443 | APIManagement | VirtualNetwork | Bağlayıcıları dağıtmak ve güncelleştirmek için gereklidir. Bu bağlantı noktasının kapatılması veya engellenmesi Ise dağıtımlarının başarısız olmasını sağlar ve bağlayıcı güncelleştirmelerini veya düzeltmelerini engeller. |
| Mantık uygulamanızdan iletişim | Giden | 80, 443 | VirtualNetwork | Hedefe göre değişir | Mantık uygulamanızın iletişim kurması gereken harici hizmetin bitiş noktaları. |
| Azure Active Directory | Giden | 80, 443 | VirtualNetwork | AzureActiveDirectory | |
| Bağlantı yönetimi | Giden | 443 | VirtualNetwork  | AppService | |
| Tanılama Günlükleri & Ölçümleri Yayımla | Giden | 443 | VirtualNetwork  | AzureMonitör | |
| Azure Depolama bağımlılığı | Giden | 80, 443, 445 | VirtualNetwork | Depolama | |
| Azure SQL bağımlılığı | Giden | 1433 | VirtualNetwork | SQL | |
| Azure Kaynak Durumu | Giden | 1886 | VirtualNetwork | AzureMonitör | Kaynak Durumu'na sağlık durumunu yayımlamak için gerekli |
| Günlükten Olay Hub'ına bağımlılık ilkesi ve izleme aracısı | Giden | 5672 | VirtualNetwork | EventHub | |
| Rol Örnekleri Arasında Redis Örnekleri için Azure Önbelleğine Erişin | Gelen <br>Giden | 6379 - 6383 | VirtualNetwork | VirtualNetwork | Ayrıca, Ise'nin Redis için Azure Önbelleği ile çalışması [için, Redis SSS'si için Azure Önbelleğinde açıklanan](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements)bu giden ve gelen bağlantı noktalarını açmanız gerekir. |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>ISE'nizi oluşturun

1. Azure [portalında,](https://portal.azure.com)ana Azure arama kutusuna filtreniz olarak girin `integration service environments` ve **Tümleştirme Hizmeti Ortamlarını**seçin.

   !["Entegrasyon Hizmeti Ortamlarını" bulun ve seçin](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. **Tümleştirme Hizmet Ortamları** bölmesine **Ekle'yi**seçin.

   !["Entegrasyon Hizmeti Ortamlarını" bulun ve seçin](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. Ortamınız için bu ayrıntıları sağlayın ve ardından **Gözden Geçir + oluştur**'u seçin, örneğin:

   ![Ortam ayrıntılarını sağlama](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Abonelik** | Evet | <*Azure abonelik adı*> | Ortamınız için kullanılacak Azure aboneliği |
   | **Kaynak grubu** | Evet | <*Azure-kaynak grubu adı*> | Ortamınızı oluşturmak istediğiniz yeni veya varolan Bir Azure kaynak grubu |
   | **Entegrasyon hizmeti ortamı adı** | Evet | <*çevre adı*> | Sadece harfler, sayılar, tireler ( ),`-`alt ( ),`_`ve dönemleri içerebilen İmKB adınız (`.`). |
   | **Konum** | Evet | <*Azure veri merkezi-bölge*> | Ortamınızı dağıtacağınız Azure veri merkezi bölgesi |
   | **Sku** | Evet | **Premium** veya **Geliştirici (No SLA)** | İmKB SKU oluşturmak ve kullanmak. Bu SK'lar arasındaki farklar için [Ise SK'larına](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)bakın. <p><p>**Önemli**: Bu seçenek yalnızca İmKB oluşturmada kullanılabilir ve daha sonra değiştirilemez. |
   | **Ek kapasite** | Premium: <br>Evet <p><p>Geliştirici: <br>Uygulanamaz | Premium: <br>0 ile 10 arasında <p><p>Geliştirici: <br>Uygulanamaz | Bu İmKB kaynağı için kullanılacak ek işleme birimlerinin sayısı. Oluşturmadan sonra kapasite eklemek için [bkz.](../logic-apps/ise-manage-integration-service-environment.md#add-capacity) |
   | **Erişim bitiş noktası** | Evet | **Dahili** veya **Harici** | İmKB için kullanılacak erişim uç noktalarının türü. Bu uç noktalar, İmKB'nizdeki mantık uygulamalarındaki istek veya webhook tetikleyicilerinin sanal ağınızın dışından çağrı alıp alamayamayacağını belirler. <p><p>Seçiminiz, mantık uygulamanızdaki giriş ve çıktıları görüntüleme ve erişim tarihi ne kadar etkieder. Daha fazla bilgi için [Ise uç nokta erişimine](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access)bakın. <p><p>**Önemli**: Bu seçenek yalnızca İmKB oluşturmada kullanılabilir ve daha sonra değiştirilemez. |
   | **Sanal ağ** | Evet | <*Azure-sanal ağ adı*> | Ortamınızı enjekte etmek istediğiniz Azure sanal ağı, bu ortamdaki mantıksal uygulamaların sanal ağınıza erişebilmesini sağlar. Ağınız yoksa, [önce bir Azure sanal ağı oluşturun.](../virtual-network/quick-create-portal.md) <p><p>**Önemli**: Bu enjeksiyonu *ancak* İmKB'nizi oluşturduğunuzda gerçekleştirebilirsiniz. |
   | **Alt ağlar** | Evet | <*subnet kaynak listesi*> | Bir ISE, ortamınızda kaynak oluşturmak ve dağıtmak için dört *boş* alt ağ gerektirir. Her alt ağ oluşturmak için [bu tablonun altındaki adımları izleyin.](#create-subnet) |
   |||||

   <a name="create-subnet"></a>

   **Alt ağ oluşturma**

   Ortamınızda kaynak oluşturmak ve dağıtmak için, İmKB'nizin herhangi bir hizmete devredilmeyen dört *boş* alt ağ gerekir. Her alt ağ, İmKB'nizde kullanılan farklı bir Logic Apps bileşenini destekler. Ortamınızı oluşturduktan sonra bu alt ağ adreslerini *değiştiremezsiniz.* Her alt ağ bu gereksinimleri karşılamak gerekir:

   * Alfabetik bir karakter veya alt (sayı yok) ile başlayan ve bu karakterleri `<` `>`kullanmayan bir `?` `/`adı vardır: , `%`, `&` `\\`, , .

   * [Sınıfsız Etki Alanları Arası Yönlendirme (CIDR) biçimini](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) ve B sınıfı adres alanını kullanır.

   * Her alt `/27` ağ *en az*32 adres gerektirdiğinden adres alanında en az bir tane kullanır. Örnek:

     * `10.0.0.0/28`2<sup>(32-28)</sup> 2<sup>4</sup> veya 16 olduğu için sadece 16 adresleri vardır ve çok küçüktür.

     * `10.0.0.0/27`2<sup>(32-27)</sup> 2<sup>5</sup> veya 32 olduğu için 32 adresleri vardır.

     * `10.0.0.0/24`2<sup>(32-24)</sup> 2<sup>8</sup> veya 256 olduğundan 256 adresleri vardır. Ancak, daha fazla adres ek avantajlar sağlamaz.

     Adresleri hesaplama hakkında daha fazla bilgi edinmek için [Bkz. IPv4 CIDR blokları.](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks)

   * [ExpressRoute](../expressroute/expressroute-introduction.md)kullanıyorsanız, aşağıdaki rotaya sahip [bir rota tablosu oluşturmanız](../virtual-network/manage-route-table.md) ve bu tabloyu İmKB tarafından kullanılan her alt netle bağlamanız gerekir:

     **Adı**: <*rota adı*><br>
     **Adres öneki**: 0.0.0.0/0<br>
     **Sonraki hop**: Internet

   1. Alt **ağlar** listesinin altında, **alt net yapılandırmasını yönet'i**seçin.

      ![Alt ağ yapılandırmayı yönetme](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. Alt **ağlar** bölmesine, **Subnet'i**seçin.

      ![Dört boş alt ağ ekleme](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. Ekle **alt ağ** bölmesinde bu bilgileri sağlayın.

      * **Adı**: Alt ağınızın adı
      * **Adres aralığı (CIDR bloğu)**: Alt ağınızın sanal ağınızdaki ve CIDR formatında aralığı

      ![Alt ağ ayrıntıları ekleme](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. İşiniz bittiğinde **Tamam**’ı seçin.

   1. Üç alt ağ için bu adımları yineleyin.

      > [!NOTE]
      > Oluşturmaya çalıştığınız alt ağlar geçerli değilse, Azure portalı bir ileti gösterir, ancak ilerlemenizi engellemez.

   Alt ağlar oluşturma hakkında daha fazla bilgi [için](../virtual-network/virtual-network-manage-subnet.md)bkz.

1. Azure İmKB bilgilerinizi başarıyla doğruladıktan sonra, örneğin **Oluştur'u**seçin:

   ![Başarılı doğrulamadan sonra "Oluştur" seçeneğini belirleyin](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure ortamınızı dağıtmaya başlar ve bu işlemin tamamlanması genellikle iki saat sürer. Bazen, dağıtım dört saat kadar sürebilir. Dağıtım durumunu denetlemek için Azure araç çubuğunuzda bildirimler bölmesini açan bildirimler simgesini seçin.

   ![Dağıtım durumunu denetleme](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Dağıtım başarıyla biterse, Azure şu bildirimi gösterir:

   ![Dağıtım başarılı oldu](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   Aksi takdirde, sorun giderme dağıtımı için Azure portalı yönergelerini izleyin.

   > [!NOTE]
   > Dağıtım başarısız olursa veya İmKB'nizi silerseniz, Azure'un alt ağlarınızı serbest bırakması bir saat kadar sürebilir. Bu gecikme, bu alt ağları başka bir ISE'de yeniden kullanmadan önce beklemeniz anlamına gelir.
   >
   > Sanal ağınızı silerseniz, Azure'un alt ağlarınızı serbest bırakması genellikle iki saat kadar sürer, ancak bu işlem daha uzun sürebilir. 
   > Sanal ağları silerken, hiçbir kaynağın hala bağlı olmadığından emin olun. 
   > Bkz. [Sanal ağı sil.](../virtual-network/manage-virtual-network.md#delete-a-virtual-network)

1. Ortamınızı görüntülemek için, Azure dağıtım bittikten sonra ortamınıza otomatik olarak gitmiyorsa **kaynağa git'i** seçin.

1. İmKB'niz için ağ durumunu denetlemek için [bkz.](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)

1. İmKB'nizde mantık uygulamaları ve diğer yapılar oluşturmaya başlamak [için](../logic-apps/add-artifacts-integration-service-environment-ise.md)bkz.

   > [!IMPORTANT]
   > İmKB'nizi oluşturduktan sonra kullanıma sunulan yönetilen ISE konektörleri, Logic App Designer'Daki bağlayıcı seçicide otomatik olarak görünmez. Bu İmKB konektörlerini kullanabilmeniz için, Bu konektörleri Logic App Designer'da görünmesi için [Bİm'inize](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment) el ile eklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Entegrasyon hizmeti ortamlarına kaynak ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* [Azure Sanal Ağı](../virtual-network/virtual-networks-overview.md) hakkında daha fazla bilgi edinin
* Azure [hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md) hakkında bilgi edinin
