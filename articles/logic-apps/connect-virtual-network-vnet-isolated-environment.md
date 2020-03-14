---
title: ISE ile Azure sanal ağlarına bağlanma
description: Azure Logic Apps Azure sanal ağlarına (VNet) erişebilen bir tümleştirme hizmeti ortamı (ıSE) oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fedc1f6ce8fbaeaf0d2cae3a1b04169192868e61
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79270699"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Tümleştirme hizmeti ortamı (ıSE) kullanarak Azure Logic Apps Azure sanal ağlarına bağlanma

Mantıksal uygulamalarınızın ve tümleştirme hesaplarınızın bir [Azure sanal ağına](../virtual-network/virtual-networks-overview.md)erişmesi gereken senaryolarda, bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturun. ISE, "Global" çok kiracılı Logic Apps hizmetinden ayrı tutulan adanmış depolama ve diğer kaynakları kullanan yalıtılmış bir ortamdır. Bu ayrım Ayrıca diğer Azure kiracılarının uygulamalarınızın performansı üzerinde sahip olabileceği etkileri azaltır. ISE, size kendi statik IP adreslerinizi de sağlar. Bu IP adresleri, genel, çok kiracılı hizmette Logic Apps tarafından paylaşılan statik IP adreslerinden ayrıdır.

Bir ıSE oluşturduğunuzda, Azure sanal ağınıza ait olan ve Logic Apps hizmetini sanal ağınıza dağıtan Azure sanal ağınız üzerinde yer alan Azure 'u sanal ağınıza *çıkartır* . Bir mantıksal uygulama veya tümleştirme hesabı oluşturduğunuzda, çalışma alanınızı konum olarak seçin. Mantıksal uygulamanız veya tümleştirme hesabınız, sanal ağınızdaki sanal makineler (VM 'Ler), sunucular, sistemler ve hizmetler gibi kaynaklara doğrudan erişim sağlayabilir.

![Tümleştirme hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> Mantıksal uygulamalar ve tümleştirme hesaplarının bir ıSE 'de birlikte çalışması için, her ikisi de konumuyla *aynı Ise* 'yı kullanmalıdır.

Bir ıSE çalışma süresi, depolama tutma, aktarım hızı, HTTP isteği ve yanıt zaman aşımları, ileti boyutları ve özel bağlayıcı istekleri için sınırları artmıştır. Daha fazla bilgi için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). Sesleri hakkında daha fazla bilgi edinmek için bkz. [Azure Logic Apps Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

Bu makalede, Azure portal kullanarak bu görevlerin nasıl tamamlanacağı gösterilmektedir:

* ISE için erişimi etkinleştirin.
* ISE 'nizi oluşturun.
* ISE 'nize ekstra kapasite ekleyin.

Ayrıca, müşteri tarafından yönetilen anahtarları ayarlama da dahil olmak üzere Logic Apps REST API kullanarak bir ıSE oluşturabilirsiniz:

* [Logic Apps kullanarak bir tümleştirme hizmeti ortamı (ıSE) oluşturun REST API](../logic-apps/create-integration-service-environment-rest-api.md)
* [Rest için bekleyen verileri şifrelemek üzere müşteri tarafından yönetilen anahtarlar ayarlama](../logic-apps/customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

  > [!IMPORTANT]
  > Logic Apps, yerleşik Tetikleyiciler, yerleşik Eylemler ve ıSE 'de çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. Fiyatlandırma ve faturalandırma işinin nasıl sesleri olduğunu öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma fiyatları için bkz. [Logic Apps fiyatlandırması](../logic-apps/logic-apps-pricing.md).

* Bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bir sanal ağınız yoksa, [Azure sanal ağı oluşturmayı](../virtual-network/quick-create-portal.md)öğrenin.

  * Sanal ağınızın, ıSE 'de kaynak oluşturup dağıtmak için dört *boş* alt ağa sahip olması gerekir. Her alt ağ, ıSE 'de kullanılan farklı bir Logic Apps bileşenini destekler. Bu alt ağları önceden oluşturabilirsiniz veya aynı anda alt ağlar oluşturabileceğiniz için ıSE 'yi oluşturmaya kadar bekleyebilirsiniz. [Alt ağ gereksinimleri](#create-subnet)hakkında daha fazla bilgi edinin.

  * Alt ağ adlarının alfabetik bir karakter veya alt çizgi ile başlaması gerekir ve şu karakterleri kullanamaz: `<`, `>`, `%`, `&`, `\\`, `?`, `/`. 
  
  * ISE 'yi bir Azure Resource Manager şablonuyla dağıtmak istiyorsanız, önce bir boş alt ağı Microsoft. Logic/ıntegrationserviceenvironment 'a temsilcdiğinizden emin olun. Azure portal aracılığıyla dağıtırken bu temsilciyi yapmanız gerekmez.

  * Sanal ağınızın, ıSE 'nin doğru çalışabilmesi ve erişilebilir kalabilmesi [IÇIN Ise için erişim sağladığından](#enable-access) emin olun.

  * Bağlantı sağlayıcısı tarafından sağlanan Microsoft bulut hizmetlerine özel bir bağlantı sağlayan [ExpressRoute](../expressroute/expressroute-introduction.md)kullanırsanız, aşağıdaki rotayı içeren [bir yol tablosu oluşturmanız](../virtual-network/manage-route-table.md) ve bu tabloyu Ise tarafından kullanılan her alt ağa bağlamanız gerekir:

    **Ad**: <*yol adı*><br>
    **Adres ön eki**: 0.0.0.0/0<br>
    **Sonraki durak**: Internet

* Azure sanal ağınız için özel DNS sunucuları kullanmak istiyorsanız, ıSE 'nizi sanal ağınıza dağıtmadan önce [Bu adımları izleyerek bu sunucuları ayarlayın](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) . DNS sunucusu ayarlarını yönetme hakkında daha fazla bilgi için bkz. [sanal ağ oluşturma, değiştirme veya silme](../virtual-network/manage-virtual-network.md#change-dns-servers).

  > [!NOTE]
  > DNS sunucunuzu veya DNS sunucusu ayarlarını değiştirirseniz, ıSE 'nin bu değişiklikleri görebilmesi için ıSE 'nizi yeniden başlatmanız gerekir. Daha fazla bilgi için bkz. [Ise 'Nizi yeniden başlatma](../logic-apps/ise-manage-integration-service-environment.md#restart-ISE).

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>ISE için erişimi etkinleştir

Bir Azure sanal ağı ile bir ıSE kullandığınızda, yaygın bir kurulum sorunu bir veya daha fazla engellenen bağlantı noktasına sahiptir. ISE ve hedef sistemleriniz arasında bağlantı oluşturmak için kullandığınız bağlayıcılar kendi bağlantı noktası gereksinimlerine da sahip olabilir. Örneğin, FTP bağlayıcısını kullanarak bir FTP sistemiyle iletişim kurdıysanız, FTP sisteminizde kullandığınız bağlantı noktasının kullanılabilir olması gerekir, örneğin, komut göndermek için bağlantı noktası 21.

ISE 'nizin erişilebilir olduğundan ve bu ıSE 'deki mantıksal uygulamaların sanal ağınızdaki her alt ağ üzerinde iletişim kurabildiğinden emin olmak için, [her alt ağ için bu tabloda açıklanan bağlantı noktalarını açın](#network-ports-for-ise). Gerekli bağlantı noktaları kullanılamıyorsa, ıSE 'niz doğru çalışmaz.

* IP kısıtlamalarına sahip diğer uç noktalara erişmesi gereken birden çok ıSE örneğiniz varsa, sanal ağınıza bir [Azure Güvenlik Duvarı](../firewall/overview.md) veya [ağ sanal](../virtual-network/virtual-networks-overview.md#filter-network-traffic) gereci dağıtın ve giden trafiği bu güvenlik duvarı veya ağ sanal gereci üzerinden yönlendirin. Daha sonra, sanal ağınızdaki tüm ıSE örneklerinin hedef sistemlerle iletişim kurmak için kullanabileceği [tek, giden, genel, statik ve öngörülebilir BIR IP adresi ayarlayabilirsiniz](connect-virtual-network-vnet-set-up-single-ip-address.md) . Bu şekilde, her bir ıSE için bu hedef sistemlerde ek güvenlik duvarı açılışlarını ayarlamanız gerekmez.

   > [!NOTE]
   > Bu yaklaşımı, senaryolarınız erişmesi gereken IP adresi sayısını sınırlandırmaya gerektirdiğinde tek bir ıSE için kullanabilirsiniz. Güvenlik duvarının veya sanal ağ gerecinin ek maliyetlerinin senaryonuz için anlamlı olup olmadığını göz önünde bulundurun. [Azure Güvenlik Duvarı fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-firewall/)hakkında daha fazla bilgi edinin.

* Herhangi bir kısıtlama olmadan yeni bir Azure sanal ağı ve alt ağı oluşturduysanız, alt ağlardaki trafiği denetlemek için sanal ağınızda [ağ güvenlik grupları (NSG 'ler)](../virtual-network/security-overview.md#network-security-groups) ayarlamanıza gerek yoktur.

* Var olan bir sanal ağda, [alt ağlar arasında ağ trafiğini filtreleyerek](../virtual-network/tutorial-filter-network-traffic.md)NSG 'leri *isteğe bağlı olarak* ayarlayabilirsiniz. Bu rotaya gitmek istiyorsanız veya zaten NSG 'leri kullanıyorsanız, [Bu tablodaki bağlantı noktalarını](#network-ports-for-ise) NSG 'leri olan sanal ağ üzerinde açın veya NSG 'leri ayarlamak istediğinizi doğrulayın.

  > [!NOTE]
  > [NSG güvenlik kuralları](../virtual-network/security-overview.md#security-rules)kullanıyorsanız, hem TCP hem *de* UDP protokollerini kullanmanız gerekir. NSG güvenlik kuralları, bu bağlantı noktalarına erişmesi gereken IP adresleri için açmanız gereken bağlantı noktalarını anlatmaktadır. Bu uç noktalar arasındaki tüm güvenlik duvarlarının, yönlendiricilerin veya diğer öğelerin bu IP adresleriyle erişilebilir olan bağlantı noktalarını da tutduğundan emin olun.

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>ISE 'niz tarafından kullanılan ağ bağlantı noktaları

Bu tabloda, Azure sanal ağınızda ıSE 'nin kullandığı ve bu bağlantı noktalarının kullanıldığı bağlantı noktaları açıklanmaktadır. Güvenlik kuralları oluştururken karmaşıklığı azaltmaya yardımcı olmak için, tablodaki [hizmet etiketleri](../virtual-network/service-tags-overview.md) belirli bir Azure HIZMETI için IP adresi ön eklerinin gruplarını temsil eder.

> [!IMPORTANT]
> Kaynak bağlantı noktaları kısa ömürlü olduğundan, tüm kurallar için `*` olarak ayarladığınızdan emin olun. Belirtildiği yerde, iç ıSE ve dış ıSE, [Ise oluşturma sırasında seçilen uç noktaya](connect-virtual-network-vnet-isolated-environment.md#create-environment)başvurur. Daha fazla bilgi için bkz. [uç nokta erişimi](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). 

| Amaç | Yön | Hedef bağlantı noktaları | Kaynak hizmet etiketi | Hedef hizmet etiketi | Notlar |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| Sanal ağınız içinde ıntersubnet iletişimi | Gelen & giden | * | ISE alt ağlarınızın bulunduğu sanal ağın adres alanı | ISE alt ağlarınızın bulunduğu sanal ağın adres alanı | Sanal ağınızdaki alt ağlar *arasında* akış için gereken trafik. <p><p>**Önemli**: her bir alt ağdaki *Bileşenler* arasında akış yapılacak trafik için, her alt ağ içinde tüm bağlantı noktalarını açtığınızdan emin olun. |
| Mantıksal uygulamanıza yönelik iletişim | Gelen | 443 | İç ıSE: <br>VirtualNetwork <p><p>Dış ıSE: <br>Internet <br>(bkz. **Notlar** sütunu) | VirtualNetwork | **Internet** Service etiketini kullanmak yerine, mantıksal uygulamanızda herhangi bir istek tetikleyicisi veya Web kancaları çağıran bilgisayar veya hizmet IÇIN kaynak IP adresini belirtebilirsiniz. <p><p>**Önemli**: Bu bağlantı noktasını kapatmak veya engellemek, istek Tetikleyicileri olan Logic Apps 'e http çağrıları önler. |
| Mantıksal uygulama çalıştırma geçmişi | Gelen | 443 | İç ıSE: <br>VirtualNetwork <p><p>Dış ıSE: <br>Internet <br>(bkz. **Notlar** sütunu) | VirtualNetwork | **Internet** Service etiketini kullanmak yerine, mantıksal uygulamanızın çalıştırma geçmişini görüntülemek istediğiniz bilgisayar veya hizmet IÇIN kaynak IP adresini belirtebilirsiniz. <p><p>**Önemli**: Bu bağlantı noktasını kapatmak veya engellemek, çalıştırma geçmişini görüntülemenize engel olmakla birlikte, bu çalıştırma geçmişindeki her adımın giriş ve çıkışlarını görüntüleyemezsiniz. |
| Logic Apps Designer-Dynamic özellikleri | Gelen | 454 | LogicAppsManagement | VirtualNetwork | İstekler, bu bölge için Logic Apps erişim uç noktası [gelen](../logic-apps/logic-apps-limits-and-config.md#inbound) IP adreslerinden gelir. |
| Bağlayıcı dağıtımı | Gelen | 454 | AzureConnectors | VirtualNetwork | Bağlayıcıları dağıtmak ve güncelleştirmek için gereklidir. Bu bağlantı noktasını kapatmak veya engellemek ıSE dağıtımlarının başarısız olmasına neden olur ve bağlayıcı güncelleştirmelerinin veya düzeltmelerinin yapılmasını önler. |
| Ağ durumu denetimi | Gelen | 454 | LogicApps | VirtualNetwork | İstekler, bu bölge için hem [gelen](../logic-apps/logic-apps-limits-and-config.md#inbound) hem de [giden](../logic-apps/logic-apps-limits-and-config.md#outbound) IP adresleri için Logic Apps erişim uç noktasından gelir. |
| App Service yönetimi bağımlılığı | Gelen | 454, 455 | AppServiceManagement | VirtualNetwork | |
| Azure Traffic Manager iletişimi | Gelen | İç ıSE: 454 <p><p>Dış ıSE: 443 | AzureTrafficManager | VirtualNetwork | |
| API Management yönetim uç noktası | Gelen | 3443 | APIManagement | VirtualNetwork | |
| Bağlayıcı İlkesi dağıtımı | Gelen | 3443 | APIManagement | VirtualNetwork | Bağlayıcıları dağıtmak ve güncelleştirmek için gereklidir. Bu bağlantı noktasını kapatmak veya engellemek ıSE dağıtımlarının başarısız olmasına neden olur ve bağlayıcı güncelleştirmelerinin veya düzeltmelerinin yapılmasını önler. |
| Mantıksal uygulamanızdan iletişim | Giden | 80, 443 | VirtualNetwork | Hedefe göre farklılık gösterir | Mantıksal uygulamanızın iletişim kurması gereken dış hizmetin uç noktaları. |
| Azure Active Directory | Giden | 80, 443 | VirtualNetwork | AzureActiveDirectory | |
| Bağlantı yönetimi | Giden | 443 | VirtualNetwork  | AppService | |
| Ölçümleri & tanılama günlüklerini yayımlayın | Giden | 443 | VirtualNetwork  | AzureMonitor | |
| Azure depolama bağımlılığı | Giden | 80, 443, 445 | VirtualNetwork | Depolama | |
| Azure SQL bağımlılığı | Giden | 1433 | VirtualNetwork | SQL | |
| Azure Kaynak Durumu | Giden | 1886 | VirtualNetwork | AzureMonitor | Kaynak Durumu sistem durumu yayımlama için gereklidir |
| Günlüğe Olay Hub 'ı ilke ve izleme aracısına bağımlılık | Giden | 5672 | VirtualNetwork | EventHub | |
| Rol örnekleri arasında Redsıs örnekleri için Azure önbelleğine erişin | Gelen <br>Giden | 6379-6383 | VirtualNetwork | VirtualNetwork | Ayrıca, ıSE 'nin redde Azure Cache ile çalışması için, [REDSıS SSS Için Azure önbelleğinde açıklanan bu giden ve gelen bağlantı noktalarını](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements)açmanız gerekir. |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>ISE 'nizi oluşturun

1. [Azure Portal](https://portal.azure.com), ana Azure Arama kutusuna filtreniz olarak `integration service environments` girin ve **tümleştirme hizmeti ortamları**' nı seçin.

   !["Tümleştirme hizmeti ortamları" nı bulun ve seçin](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. **Tümleştirme hizmeti ortamları** bölmesinde **Ekle**' yi seçin.

   !["Tümleştirme hizmeti ortamları" nı bulun ve seçin](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. Ortamınız için bu ayrıntıları girin ve ardından **gözden geçir + oluştur**' u seçin. Örneğin:

   ![Ortam ayrıntılarını sağlama](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Abonelik** | Yes | <*Azure-subscription-name*> | Ortamınız için kullanılacak Azure aboneliği |
   | **Kaynak grubu** | Yes | <*Azure-Resource-Group-name*> | Ortamınızı oluşturmak istediğiniz yeni veya mevcut bir Azure Kaynak grubu |
   | **Tümleştirme hizmeti ortam adı** | Yes | <*ortam adı*> | Yalnızca harf, sayı, kısa çizgi (`-`), alt çizgi (`_`) ve nokta (`.`) içerebilen ıSE adınız. |
   | **Konum** | Yes | <*Azure-Datacenter-region*> | Ortamınızı dağıtacağınız Azure veri merkezi bölgesi |
   | **SKU** | Yes | **Premium** veya **Geliştirici (SLA yok)** | Oluşturulacak ve kullanılacak ıSE SKU 'SU. Bu SKU 'Lar arasındaki farklar için bkz. [Ise SKU 'ları](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). <p><p>**Önemli**: Bu seçenek yalnızca Ise oluşturma sırasında kullanılabilir ve daha sonra değiştirilemez. |
   | **Ek kapasite** | Premium <br>Yes <p><p>Tasarımcı <br>Uygulanamaz | Premium <br>0 ila 10 <p><p>Tasarımcı <br>Uygulanamaz | Bu ıSE kaynağı için kullanılacak ek işleme birimi sayısı. Oluşturulduktan sonra kapasite eklemek için, bkz. [Ise kapasitesi ekleme](../logic-apps/ise-manage-integration-service-environment.md#add-capacity). |
   | **Erişim uç noktası** | Yes | **İç** veya **dış** | ISE için kullanılacak erişim uç noktalarının türü. Bu uç noktalar, işinizdeki Logic Apps 'teki istek veya Web kancasının, sanal ağınızın dışından çağrı alıp almamadığını belirtir. <p><p>Seçiminiz, mantıksal uygulama çalışma geçmişinizdeki girdileri ve çıkışları görüntüleme ve erişme şeklini de etkiler. Daha fazla bilgi için bkz. [Ise uç noktası erişimi](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). <p><p>**Önemli**: Bu seçenek yalnızca Ise oluşturma sırasında kullanılabilir ve daha sonra değiştirilemez. |
   | **Sanal ağ** | Yes | <*Azure-sanal-ağ-adı*> | Ortamınızı eklemek istediğiniz Azure sanal ağı, bu ortamdaki Logic Apps 'in sanal ağınıza erişebilmesi için kullanabilirsiniz. Ağınız yoksa, [önce bir Azure sanal ağı oluşturun](../virtual-network/quick-create-portal.md). <p><p>**Önemli**: Bu ekleme işlemini *yalnızca* Ise 'nizi oluştururken gerçekleştirebilirsiniz. |
   | **Alt ağlar** | Yes | <*alt ağ-kaynak listesi*> | ISE, ortamınızda kaynak oluşturmak ve dağıtmak için dört *boş* alt ağ gerektirir. Her bir alt ağı oluşturmak için [Bu tablodaki adımları izleyin](#create-subnet). |
   |||||

   <a name="create-subnet"></a>

   **Alt ağ oluştur**

   Ortamınızda kaynak oluşturup dağıtmak için, ıSE 'niz, hiçbir hizmete temsilci olmayan dört *boş* alt ağa ihtiyaç duyuyor. Her alt ağ, ıSE 'de kullanılan farklı bir Logic Apps bileşenini destekler. Ortamınızı oluşturduktan sonra bu alt ağ *adreslerini değiştiremezsiniz.* Her alt ağın bu gereksinimleri karşılaması gerekir:

   * Alfabetik bir karakter veya alt çizgi (sayı yok) ile başlayan bir ada sahiptir ve şu karakterleri kullanmaz: `<`, `>`, `%`, `&`, `\\`, `?`, `/`.

   * [Sınıfsız etki alanları arası yönlendirme (CIDR) biçimini](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) ve bir sınıf B adres alanını kullanır.

   * Her alt ağ *en*az 32 adres gerektirdiğinden adres alanındaki en az bir `/27` kullanır. Örnek:

     * `10.0.0.0/28` yalnızca 16 adrese sahiptir ve 2<sup>(32-28)</sup> 2<sup>4</sup> veya 16 olduğundan çok küçük.

     * 2<sup>(32-27)</sup> 2<sup>5</sup> veya 32 olduğundan `10.0.0.0/27` 32 adresi vardır.

     * 2<sup>(32-24)</sup> 2<sup>8</sup> veya 256 olduğundan `10.0.0.0/24` 256 adresi vardır. Ancak, daha fazla adres ek bir avantaj sağlamaz.

     Adresleri hesaplama hakkında daha fazla bilgi edinmek için bkz. [ıPV4 CIDR blokları](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * [ExpressRoute](../expressroute/expressroute-introduction.md)kullanırsanız, aşağıdaki rotayı içeren [bir yol tablosu oluşturmanız](../virtual-network/manage-route-table.md) ve bu tabloyu, Ise tarafından kullanılan her alt ağ ile bağlamanız gerekir:

     **Ad**: <*yol adı*><br>
     **Adres ön eki**: 0.0.0.0/0<br>
     **Sonraki durak**: Internet

   1. **Alt ağlar** listesinde, **alt ağ yapılandırmasını Yönet**' i seçin.

      ![Alt ağ yapılandırmasını yönetme](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. **Alt ağlar** bölmesinde **alt ağ**' ı seçin.

      ![Dört boş alt ağ ekleyin](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. **Alt ağ ekle** bölmesinde, bu bilgileri sağlayın.

      * **Ad**: alt ağınızın adı
      * **Adres aralığı (CIDR bloğu)** : alt ağınızın sanal AĞıNıZDA ve CIDR biçimindeki aralığı

      ![Alt ağ ayrıntıları ekleme](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. İşiniz bittiğinde **Tamam**’ı seçin.

   1. Üç alt ağ için bu adımları tekrarlayın.

      > [!NOTE]
      > Oluşturmayı denerseniz alt ağlar geçerli değilse, Azure portal bir ileti gösterir, ancak ilerleme durumunu engellemez.

   Alt ağlar oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ alt ağı ekleme](../virtual-network/virtual-network-manage-subnet.md).

1. Azure, ıSE bilgilerinizi başarıyla doğruladıktan sonra **Oluştur**' u seçin, örneğin:

   ![Doğrulama başarılı olduktan sonra "Oluştur" u seçin](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure, ortamınızı dağıtmaya başlar ve genellikle iki saat içinde tamamlanır. Bazen dağıtım dört saate kadar sürebilir. Dağıtım durumunu denetlemek için, Azure araç çubuğınızdan bildirimler bölmesini açan Bildirimler simgesini seçin.

   ![Dağıtım durumunu denetle](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Dağıtım başarıyla tamamlandığında, Azure bu bildirimi gösterir:

   ![Dağıtım başarılı oldu](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   Aksi takdirde, dağıtım sorunlarını gidermeye yönelik Azure portal yönergeleri izleyin.

   > [!NOTE]
   > Dağıtım başarısız olursa veya ıSE 'yi silerseniz, Azure, alt ağlarınızı serbest bırakmadan önce bir saat kadar sürebilir. Bu gecikme, başka bir ıSE içinde bu alt ağları yeniden kullanmadan önce beklemeniz gerekebilecek anlamına gelir.
   >
   > Sanal ağınızı silerseniz Azure, alt ağlarınızı serbest bırakmadan genellikle iki saate kadar sürer, ancak bu işlem daha uzun sürebilir. 
   > Sanal ağları silerken, hala bağlı hiçbir kaynak bulunmadığından emin olun. 
   > Bkz. [sanal ağı silme](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Ortamınızı görüntülemek için, dağıtım bittikten sonra Azure ortamınıza otomatik olarak gitmezse **Kaynağa Git** ' i seçin.

1. ISE 'nizin ağ durumunu denetlemek için bkz. [tümleştirme hizmeti ortamınızı yönetme](../logic-apps/ise-manage-integration-service-environment.md#check-network-health).

1. Çalışma ortamınızda mantıksal uygulamalar ve diğer yapıtlar oluşturmaya başlamak için bkz. [Integration Service ortamlarına kaynak ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md).

   > [!IMPORTANT]
   > ISE 'nizi oluşturduktan sonra kullanılabilir hale gelen yönetilen ıSE bağlayıcıları, mantıksal uygulama Tasarımcısı 'ndaki bağlayıcı seçicisinde otomatik olarak görünmez. Bu ıSE bağlayıcılarını kullanabilmeniz için, mantıksal uygulama Tasarımcısı 'nda görünmesi için [Bu BAĞLAYıCıLARı Ise 'nize](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment) el ile eklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Tümleştirme hizmeti ortamlarına kaynak ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Tümleştirme hizmeti ortamlarını yönetme](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* [Azure sanal ağ](../virtual-network/virtual-networks-overview.md) hakkında daha fazla bilgi edinin
* [Azure hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md) hakkında bilgi edinin
