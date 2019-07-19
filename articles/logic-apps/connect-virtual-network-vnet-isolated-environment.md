---
title: Tümleştirme hizmeti ortamı (ıSE) üzerinden Azure Logic Apps Azure sanal ağlarına bağlanma
description: Bir tümleştirme hizmeti ortamı (ıSE) oluşturun. bu nedenle, Logic Apps ve tümleştirme hesapları Azure sanal ağlarına (VNet) erişerek özel ve genel veya "Global" Azure 'tan yalıtılmış bir şekilde olabilir
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/19/2019
ms.openlocfilehash: fe92d36eca05b47f928f6644053fb9b0149d6db9
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326742"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Tümleştirme hizmeti ortamı (ıSE) kullanarak Azure Logic Apps Azure sanal ağlarına bağlanma

Mantıksal uygulamalarınızın ve tümleştirme hesaplarınızın bir [Azure sanal ağına](../virtual-network/virtual-networks-overview.md)erişmesi gereken senaryolarda, bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturun. ISE, adanmış depolama ve genel veya "küresel" Logic Apps hizmetinden ayrı tutulan diğer kaynakları kullanan özel ve yalıtılmış bir ortamdır. Bu ayrım Ayrıca diğer Azure kiracılarının uygulamalarınızın performansı üzerinde sahip olabileceği etkileri azaltır.

Bir ıSE oluşturduğunuzda, Azure sanal ağınıza ait olan ve Logic Apps hizmetini sanal ağınıza dağıtan Azure sanal ağınız üzerinde yer alan Azure 'u sanal ağınıza *çıkartır* . Bir mantıksal uygulama veya tümleştirme hesabı oluşturduğunuzda, çalışma alanınızı konum olarak seçin. Mantıksal uygulamanız veya tümleştirme hesabınız, sanal ağınızdaki sanal makineler (VM 'Ler), sunucular, sistemler ve hizmetler gibi kaynaklara doğrudan erişim sağlayabilir.

![Tümleştirme hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Bir ıSE çalışma süresi, depolama tutma, aktarım hızı, HTTP isteği ve yanıt zaman aşımları, ileti boyutları ve özel bağlayıcı istekleri için sınırları artmıştır. Daha fazla bilgi için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](logic-apps-limits-and-config.md). Sesleri hakkında daha fazla bilgi edinmek için bkz. [Azure Logic Apps Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

Bu makalede, bu görevlerin nasıl tamamlanacağı gösterilmektedir:

* Bir sanal ağ üzerindeki gerekli bağlantı noktalarının açık olduğundan emin olun. bu sayede trafik, bu sanal ağdaki alt ağlar genelinde tümleştirme hizmeti ortamınızdan (ıSE) gezinir.

* Tümleştirme hizmeti ortamınızı (ıSE) oluşturun.

* ISE 'de çalışabilecek bir mantıksal uygulama oluşturun.

* ISE ortamınızda Logic Apps için bir tümleştirme hesabı oluşturun.

> [!IMPORTANT]
> Logic Apps, yerleşik Tetikleyiciler, yerleşik Eylemler ve ıSE 'de çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. Fiyatlandırma ve faturalandırma işinin nasıl sesleri olduğunu öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma fiyatları için bkz. [Logic Apps fiyatlandırması](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bir sanal ağınız yoksa, [Azure sanal ağı oluşturmayı](../virtual-network/quick-create-portal.md)öğrenin. 

  * Sanal ağınızın, ıSE 'de kaynak oluşturmak ve dağıtmak için dört *boş* alt ağa sahip olması gerekir. Bu alt ağları önceden oluşturabilirsiniz veya aynı anda alt ağlar oluşturabileceğiniz için ıSE 'yi oluşturmaya kadar bekleyebilirsiniz. [Alt ağ gereksinimleri](#create-subnet)hakkında daha fazla bilgi edinin. 
  
    > [!NOTE]
    > Microsoft bulut hizmetlerine özel bir bağlantı sağlayan [ExpressRoute](../expressroute/expressroute-introduction.md)kullanırsanız, aşağıdaki rotayı içeren [bir yol tablosu oluşturmanız](../virtual-network/manage-route-table.md) ve o tabloyu Ise tarafından kullanılan her alt ağ ile bağlamanız gerekir:
    > 
    > **Ad**: <*yol adı*><br>
    > **Adres ön eki**: 0.0.0.0/0<br>
    > **Sonraki atlama**: Internet

  * Sanal ağınızın [Bu bağlantı noktalarını uygun hale getiren](#ports) ve Ise 'nin düzgün bir şekilde çalıştığından emin olun.

* Azure sanal ağınız için özel DNS sunucuları kullanmak istiyorsanız, ıSE 'nizi sanal ağınıza dağıtmadan önce [Bu adımları izleyerek bu sunucuları ayarlayın](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) . Aksi halde, DNS sunucunuzu her değiştirdiğinizde, Ise genel önizleme ile kullanılabilen bir özellik olan ıSE 'nizi de yeniden başlatmanız gerekir.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

<a name="ports"></a>

## <a name="check-network-ports"></a>Ağ bağlantı noktalarını denetle

Bir sanal ağ ile bir tümleştirme hizmeti ortamı (ıSE) kullandığınızda, yaygın bir kurulum sorunu bir veya daha fazla engellenen bağlantı noktasına sahip olur. ISE ve hedef sistem arasında bağlantı oluşturmak için kullandığınız bağlayıcılar kendi bağlantı noktası gereksinimlerine da sahip olabilir. Örneğin, FTP bağlayıcısını kullanarak bir FTP sistemiyle iletişim kurabiliyorsa, bu FTP sisteminde kullandığınız bağlantı noktasının, komut göndermek için 21 numaralı bağlantı noktası için kullanılabilir olduğundan emin olun.

ISE 'yi dağıttığınız sanal ağın alt ağları arasındaki trafiği denetlemek için, [alt ağlar arasında ağ trafiğini filtreleyerek](../virtual-network/tutorial-filter-network-traffic.md)sanal ağınızda [ağ güvenlik grupları (NSG 'ler)](../virtual-network/security-overview.md) ayarlayabilirsiniz. Bu yolu seçerseniz, ıSE 'nizin, NSG 'leri kullanan sanal ağ üzerinde aşağıdaki tabloda açıklandığı gibi belirli bağlantı noktalarını açtığından emin olun. Sanal ağınızda mevcut NSG 'ler veya güvenlik duvarları varsa, bu bağlantı noktalarını açdıklarından emin olun. Bu şekilde, ıSE 'niz erişilebilir kalır ve e-postalarınız için erişiminizi kaybetmemek için düzgün bir şekilde çalışabilir. Aksi takdirde, gerekli bağlantı noktaları kullanılamaz durumdaysa, ıSE çalışmanız durdu.

Bu tablolar, sanal ağınızdaki ıSE 'nin kullandığı ve bu bağlantı noktalarının kullanıldığı bağlantı noktalarını anlatmaktadır. [Kaynak Yöneticisi hizmet etiketleri](../virtual-network/security-overview.md#service-tags) , güvenlik kuralları oluştururken karmaşıklığı en aza indirmeye yardımcı olan bir IP adresi önekleri grubunu temsil eder.

> [!IMPORTANT]
> Alt ağlarınızdaki iç iletişim için ıSE, bu alt ağların içindeki tüm bağlantı noktalarını açmanızı gerektirir.

| Amaç | Direction | Bağlantı Noktaları | Kaynak hizmet etiketi | Hedef hizmet etiketi | Notlar |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Azure Logic Apps iletişim | Giden | 80 & 443 | VirtualNetwork | Internet | Bağlantı noktası, Logic Apps hizmetinin iletişim kurduğu dış hizmete bağlıdır |
| Azure Active Directory | Giden | 80 & 443 | VirtualNetwork | AzureActiveDirectory | |
| Azure depolama bağımlılığı | Giden | 80 & 443 | VirtualNetwork | Depolama | |
| Intersubnet iletişimi | Gelen & giden | 80 & 443 | VirtualNetwork | VirtualNetwork | Alt ağlar arasındaki iletişim için |
| Azure Logic Apps iletişim | Gelen | 443 | Internet | VirtualNetwork | Mantıksal uygulamanızda mevcut olan herhangi bir istek tetikleyicisi veya Web kancasını çağıran bilgisayar veya hizmetin IP adresi. Bu bağlantı noktasını kapatmak veya engellemek, istek tetikleyicilerine sahip mantıksal uygulamalara HTTP çağrıları yapılmasını engeller.  |
| Mantıksal uygulama çalıştırma geçmişi | Gelen | 443 | Internet | VirtualNetwork | Mantıksal uygulamanın çalıştırma geçmişini görüntülediğiniz bilgisayarın IP adresi. Bu bağlantı noktasını kapatmak veya engellemek, çalıştırma geçmişini görüntülemenize engel olmakla birlikte, bu çalıştırma geçmişindeki her adımın giriş ve çıkışlarını görüntüleyemezsiniz. |
| Bağlantı yönetimi | Giden | 443 | VirtualNetwork  | Internet | |
| Ölçümleri & tanılama günlüklerini yayımlayın | Giden | 443 | VirtualNetwork  | AzureMonitor | |
| Azure Traffic Manager iletişimi | Gelen | 443 | AzureTrafficManager | VirtualNetwork | |
| Logic Apps Designer-Dynamic özellikleri | Gelen | 454 | Internet  | VirtualNetwork | İstekler, [Bu bölgedeki Logic Apps erişim uç noktası gelen IP adreslerinden](../logic-apps/logic-apps-limits-and-config.md#inbound)gelir. |
| App Service yönetimi bağımlılığı | Gelen | 454 & 455 | AppServiceManagement | VirtualNetwork | |
| Bağlayıcı dağıtımı | Gelen | 454 & 3443 | Internet  | VirtualNetwork | Bağlayıcıları dağıtmak ve güncelleştirmek için gereklidir. Bu bağlantı noktasını kapatmak veya engellemek ıSE dağıtımlarının başarısız olmasına neden olur ve bağlayıcı güncelleştirmelerinin veya düzeltmelerinin yapılmasını önler. |
| Azure SQL bağımlılığı | Giden | 1433 | VirtualNetwork | SQL |
| Azure Kaynak Durumu | Giden | 1886 | VirtualNetwork | Internet | Kaynak Durumu sistem durumunu yayımlama için |
| API Management yönetim uç noktası | Gelen | 3443 | APIManagement  | VirtualNetwork | |
| Günlüğe Olay Hub 'ı ilke ve izleme aracısına bağımlılık | Giden | 5672 | VirtualNetwork  | EventHub | |
| Rol örnekleri arasında Redsıs örnekleri için Azure önbelleğine erişin | Gelen <br>Giden | 6379-6383 | VirtualNetwork  | VirtualNetwork | Ayrıca, ıSE 'nin redde Azure Cache ile çalışması için, [REDSıS SSS Için Azure önbelleğinde açıklanan bu giden ve gelen bağlantı noktalarını](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements)açmanız gerekir. |
| Azure Load Balancer | Gelen | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>ISE 'nizi oluşturun

Tümleştirme hizmeti ortamınızı (ıSE) oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com)ana Azure menüsünde **kaynak oluştur**' u seçin.
Arama kutusuna filtreniz olarak "Integration Service Environment" yazın.

   ![Yeni kaynak oluştur](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Tümleştirme Hizmeti Ortamı oluşturma bölmesinde **Oluştur**' u seçin.

   !["Oluştur" öğesini seçin](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Ortamınız için bu ayrıntıları girin ve ardından **gözden geçir + oluştur**' u seçin. Örneğin:

   ![Ortam ayrıntılarını sağlama](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Özellik | Gerekli | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Abonelik** | Evet | <*Azure-subscription-name*> | Ortamınız için kullanılacak Azure aboneliği |
   | **Kaynak grubu** | Evet | <*Azure-Resource-Group-Name*> | Ortamınızı oluşturmak istediğiniz Azure Kaynak grubu |
   | **Tümleştirme Hizmeti Ortamı adı** | Evet | <*ortam-adı*> | Ortamınıza verilecek ad |
   | **Location** | Evet | <*Azure-Datacenter-Region*> | Ortamınızı dağıtacağınız Azure veri merkezi bölgesi |
   | **SKU** | Evet | **Premium** veya **Geliştirici (SLA yok)** | Oluşturulacak ve kullanılacak ıSE SKU 'SU. Bu SKU 'Lar arasındaki farklar için bkz. [Ise SKU 'ları](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). |
   | **Ek kapasite** | Premium: <br>Evet <p><p>Geliştirici: <br>Geçerli değil | Premium: <br>0 ila 10 <p><p>Geliştirici: <br>Geçerli değil | Bu ıSE kaynağı için kullanılacak ek işleme birimi sayısı. Oluşturulduktan sonra kapasite eklemek için, bkz. [Ise kapasitesi ekleme](#add-capacity). |
   | **Sanal ağ** | Evet | <*Azure-sanal-ağ-adı*> | Ortamınızı eklemek istediğiniz Azure sanal ağı, bu ortamdaki Logic Apps 'in sanal ağınıza erişebilmesi için kullanabilirsiniz. Ağınız yoksa, [önce bir Azure sanal ağı oluşturun](../virtual-network/quick-create-portal.md). <p>**Önemli**: Bu ekleme işlemini *yalnızca* Ise 'nizi oluştururken gerçekleştirebilirsiniz. |
   | **Alt ağlar** | Evet | <*alt ağ-kaynak listesi*> | ISE, ortamınızda kaynak oluşturmak ve dağıtmak için dört *boş* alt ağ gerektirir. Her bir alt ağı oluşturmak için [Bu tablodaki adımları izleyin](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Alt ağ oluştur**

   Ortamınızda kaynak oluşturup dağıtmak için, ıSE 'niz, hiçbir hizmete temsilci olmayan dört *boş* alt ağa ihtiyaç duyuyor. 
   Ortamınızı oluşturduktan sonra bu alt ağ *adreslerini değiştiremezsiniz.* Her alt ağ şu ölçütlere uymalıdır:

   * Alfabetik bir karakter veya alt çizgi ile başlayan bir ada sahiptir ve şu karakterleri içermemelidir: `<`, `%` `&` `>`,,, `\\`, `?`,`/`

   * [Sınıfsız etki alanları arası yönlendirme (CIDR) biçimini](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) ve bir sınıf B adres alanını kullanır.

   * Her alt ağın `/27` *Minimum*olarak 32 adresi olması gerektiğinden, adres alanında en az bir kullanır. Örneğin:

     * `10.0.0.0/27`2<sup>(32-27)</sup> 2<sup>5</sup> veya 32 olduğundan 32 adresi vardır.

     * `10.0.0.0/24`2<sup>(32-24)</sup> 2<sup>8</sup> veya 256 olduğundan 256 adresi vardır.

     * `10.0.0.0/28`yalnızca 16 adrese sahiptir ve 2<sup>(32-28)</sup> 2<sup>4</sup> veya 16 olduğundan çok küçük.

     Adresleri hesaplama hakkında daha fazla bilgi edinmek için bkz. [ıPV4 CIDR blokları](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * [ExpressRoute](../expressroute/expressroute-introduction.md)kullanıyorsanız, aşağıdaki rotayı içeren [bir yol tablosu oluşturmayı](../virtual-network/manage-route-table.md) ve o tabloyu Ise tarafından kullanılan her alt ağ ile bağlamayı unutmayın:

     **Ad**: <*yol adı*><br>
     **Adres ön eki**: 0.0.0.0/0<br>
     **Sonraki atlama**: Internet

   1. **Alt ağlar** listesinde, **alt ağ yapılandırmasını Yönet**' i seçin.

      ![Alt ağ yapılandırmasını yönetme](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. **Alt ağlar** bölmesinde **alt ağ**' ı seçin.

      ![Alt ağ ekleme](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. **Alt ağ ekle** bölmesinde, bu bilgileri sağlayın.

      * **Ad**: Alt ağınızın adı
      * **Adres aralığı (CIDR bloğu)** : Sanal ağınızdaki ve CıDR biçimindeki alt ağınızın aralığı

      ![Alt ağ ayrıntıları ekleme](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. İşiniz bittiğinde **Tamam**' ı seçin.

   1. Üç alt ağ için bu adımları tekrarlayın.

      > [!NOTE]
      > Oluşturmayı denerseniz alt ağlar geçerli değilse, Azure portal bir ileti gösterir, ancak ilerleme durumunu engellemez.

1. Azure, ıSE bilgilerinizi başarıyla doğruladıktan sonra **Oluştur**' u seçin, örneğin:

   ![Doğrulama başarılı olduktan sonra "Oluştur" u seçin](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure ortamınızı dağıtmaya başlar, ancak *Bu işlemin tamamlanması* iki saate kadar sürebilir. 
   Dağıtım durumunu denetlemek için, Azure araç çubuğınızdan bildirimler bölmesini açan Bildirimler simgesini seçin.

   ![Dağıtım durumunu denetle](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Dağıtım başarıyla tamamlandığında, Azure bu bildirimi gösterir:

   ![Dağıtım başarılı oldu](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   Aksi takdirde, dağıtım sorunlarını gidermeye yönelik Azure portal yönergeleri izleyin.

   > [!NOTE]
   > Dağıtım başarısız olursa veya ıSE 'yi silerseniz, Azure, alt ağlarınızı serbest bırakmadan önce bir saat kadar sürebilir. Bu gecikme, başka bir ıSE içinde bu alt ağları yeniden kullanmadan önce beklemeniz gerekebilecek anlamına gelir. 
   >
   > Sanal ağınızı silerseniz Azure, alt ağlarınızı serbest bırakmadan genellikle iki saate kadar sürer, ancak bu işlem daha uzun sürebilir. 
   > Sanal ağları silerken, hala bağlı hiçbir kaynak bulunmadığından emin olun. Bkz. [sanal ağı silme](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Ortamınızı görüntülemek için, dağıtım bittikten sonra Azure ortamınıza otomatik olarak gitmezse **Kaynağa Git** ' i seçin.  

Alt ağlar oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ alt ağı ekleme](../virtual-network/virtual-network-manage-subnet.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Mantıksal uygulama oluşturma-ıSE

Tümleştirme hizmeti ortamınızda (ıSE) çalışan Logic Apps oluşturmak için, **konum** özelliğini ayarladığınız durumlar dışında [mantıksal uygulamalarınızı her zamanki şekilde oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md) , **tümleştirme hizmeti ortamları** bölümünde Ise 'yi seçin, örneğin örneğinde

  ![Tümleştirme hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

Tetikleyiciler ve eylemlerin nasıl çalıştığı ve küresel Logic Apps hizmetiyle karşılaştırıldığında bir ıSE kullandığınızda nasıl etiketlendiği hakkında farklılıklar için bkz. [Ise 'ye genel bakış](connect-virtual-network-vnet-isolated-environment-overview.md#difference).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Tümleştirme hesabı oluştur-ıSE

Tümleştirme hizmeti ortamında (ıSE) Logic Apps ile bir tümleştirme hesabı kullanmak istiyorsanız, bu tümleştirme hesabının mantıksal uygulamalarla *aynı ortamı* kullanması gerekir. Bir ıSE içindeki Logic Apps, aynı ıSE 'de yalnızca tümleştirme hesaplarına başvurabilir. Oluşturma sırasında seçilen [ıSE SKU 'su](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) temel alınarak, Ise 'niz hiçbir ek ücret ödemeden belirli tümleştirme hesabı kullanımını içerir. Fiyatlandırma ve faturalama ile tümleştirme hesaplarında nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma fiyatları için bkz. [Logic Apps fiyatlandırması](https://azure.microsoft.com/pricing/details/logic-apps/).

ISE kullanan bir tümleştirme hesabı oluşturmak için, **konum** özelliğini ayarladığınız durumlar haricinde [tümleştirme hesabınızı her zamanki şekilde oluşturun](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , **tümleştirme hizmeti ortamları** bölümünde Ise 'yi seçin, örneğin:

![Tümleştirme hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>ISE kapasitesi ekleme

Premium ıSE temel birimi sabit kapasiteye sahiptir, bu nedenle daha fazla işleme ihtiyacınız varsa, oluşturma sırasında veya daha sonra daha fazla ölçek birimi ekleyebilirsiniz. Performans ölçümlerine göre veya bir dizi ek işleme birimine göre otomatik ölçeklendirme yapabilirsiniz. Ölçümleri temel alarak otomatik ölçeklendirmeyi seçerseniz, çeşitli ölçütler arasından seçim yapabilir ve bu ölçütü karşılamak için eşik koşullarını belirtebilirsiniz. Geliştirici SKU 'SU ölçek birimleri ekleme özelliğini içermez.

1. Azure portal, ıSE 'nizi bulun.

1. ISE 'nizin kullanım ve performans ölçümlerini gözden geçirmek için, ıSE 'nin ana menüsünde **genel bakış**' ı seçin.

   ![ISE için kullanımı görüntüleme](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Otomatik ölçeklendirmeyi ayarlamak için **Ayarlar**altında **ölçeği**genişletme ' yi seçin. **Yapılandır** sekmesinde **Otomatik ölçeklendirmeyi etkinleştir**' i seçin.

   ![Otomatik ölçeklendirmeyi aç](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. **Otomatik ölçeklendirme ayarı adı**için ayarınız için bir ad girin.

1. **Varsayılan** bölümde, **ölçüm temelinde ölçek** ' i veya **belirli bir örnek sayısına**göre ölçeklendirin ' ı seçin.

   * Örnek tabanlı ' i seçerseniz, 0 ile 10 (dahil) arasında işleme birimi sayısını girin.

   * Ölçüm tabanlı ' i seçerseniz, aşağıdaki adımları izleyin:

     1. **Kurallar** bölümünde **Kural Ekle**' yi seçin.

     1. **Kural ölçekleme** bölmesinde, kural tetiklendiğinde gerçekleştirilecek ölçütlerinizi ve eyleminizi ayarlayın.

     1. İşiniz bittiğinde **Ekle**' yi seçin.

1. Otomatik ölçeklendirme ayarlarınızla işiniz bittiğinde değişikliklerinizi kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal ağ](../virtual-network/virtual-networks-overview.md) hakkında daha fazla bilgi edinin
* [Azure hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md) hakkında bilgi edinin
