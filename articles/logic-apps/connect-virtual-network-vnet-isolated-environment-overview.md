---
title: Azure sanal ağlarına erişme
description: Tümleştirme hizmeti ortamlarının (sesleri) yardım Logic Apps 'in Azure sanal ağlarına (VNet 'ler) erişme konusunda genel bakış
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: b149757ccfc41587aa3ea6c5d18717fdecaba656
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84656637"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Tümleştirme hizmeti ortamlarını (sesleri) kullanarak Azure Logic Apps Azure sanal ağ kaynaklarına erişim

Bazen mantıksal uygulamalarınızın sanal makineler (VM) ve diğer sistemler veya hizmetler gibi bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md)içindeki güvenli kaynaklara erişmesi gerekir. Bu erişimi ayarlamak için [bir *tümleştirme hizmeti ortamı* (ISE) oluşturabilirsiniz](../logic-apps/connect-virtual-network-vnet-isolated-environment.md). ISE, adanmış kaynakları kullanan ve "küresel" çok kiracılı Logic Apps hizmetinden ayrı olarak çalıştırılan Logic Apps hizmetin adanmış bir örneğidir.

Mantıksal uygulamaları kendi ayrı ayrılmış Örneğinizde çalıştırmak, diğer Azure kiracılarının ["gürültülü komşular" etkisi](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors)olarak da bilinen uygulamalarınızın performansı üzerinde sahip olabileceği etkiyi azaltmaya yardımcı olur. ISE Ayrıca bu avantajları sağlar:

* Çoklu kiracı hizmetindeki Logic Apps tarafından paylaşılan statik IP adreslerinden ayrı olan kendi statik IP adresleriniz. Hedef sistemlerle iletişim kurmak için tek bir genel, statik ve öngörülebilir giden IP adresi de ayarlayabilirsiniz. Bu şekilde, her bir ıSE için bu hedef sistemlerde ek güvenlik duvarı açılışlarını ayarlamanız gerekmez.

* Çalışma süresi, depolama tutma, aktarım hızı, HTTP isteği ve yanıt zaman aşımları, ileti boyutları ve özel bağlayıcı istekleri için artan sınırlar. Daha fazla bilgi için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](logic-apps-limits-and-config.md).

> [!NOTE]
> Azure depolama, Azure Cosmos DB veya Azure SQL veritabanı, iş ortağı hizmetleri veya Azure 'da barındırılan müşteri hizmetleri gibi Azure PaaS hizmetlerine erişim sağlamak için bazı Azure sanal ağları özel uç noktalar ([Azure özel bağlantı](../private-link/private-link-overview.md)) kullanır. Mantıksal uygulamalarınızın özel uç noktaları kullanan sanal ağlara erişmesi gerekiyorsa, bu mantık uygulamalarını bir ıSE içinde oluşturmanız, dağıtmanız ve çalıştırmanız gerekir.

Bir ıSE oluşturduğunuzda, Azure bu ıSE 'yi Azure sanal ağınıza *çıkartır* veya dağıtır. Daha sonra bu ıSE 'yi, erişmesi gereken Logic Apps ve tümleştirme hesaplarının konumu olarak kullanabilirsiniz.

![Tümleştirme hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Logic Apps, mantıksal uygulamalarınızla aynı ıSE çalıştıran bu öğeleri kullanarak sanal ağınıza ait veya bağlı olan kaynaklara erişebilir:

* HTTP tetikleyicisi veya eylemi gibi **çekirdek**etiketli bir yerleşik tetikleyici veya eylem
* Bu sistem veya hizmet için **Ise**etiketli bağlayıcı
* Özel bir bağlayıcı

Ayrıca, ıSE 'de Logic Apps ile **çekirdek** veya **Ise** etiketi olmayan bağlayıcılar da kullanabilirsiniz. Bu bağlayıcılar bunun yerine çok kiracılı Logic Apps hizmetinde çalışır. Daha fazla bilgi için aşağıdaki bölümlere bakın:

* [Adanmış ve çok kiracılı](#difference)
* [Tümleştirme hizmeti ortamından Bağlan](../connectors/apis-list.md#integration-service-environment)
* [ISE bağlayıcıları](../connectors/apis-list.md#ise-connectors)

> [!IMPORTANT]
> Logic Apps, yerleşik Tetikleyiciler, yerleşik Eylemler ve ıSE 'de çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. Daha fazla bilgi için bkz. [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md#fixed-pricing). Fiyatlandırma ayrıntıları için bkz. [Logic Apps fiyatlandırması](../logic-apps/logic-apps-pricing.md).

Bu genel bakışta, bir ıSE 'nin Logic Apps 'in Azure sanal ağınıza doğrudan erişimine nasıl verdiği hakkında daha fazla bilgi açıklanmakta ve bir ıSE ile çok kiracılı Logic Apps hizmeti arasındaki farkları karşılaştırılmaktadır.

<a name="difference"></a>

## <a name="dedicated-versus-multi-tenant"></a>Adanmış ve çok kiracılı

Bir ıSE 'de Logic Apps oluşturup çalıştırdığınızda, aynı kullanıcı deneyimlerini ve çok kiracılı Logic Apps hizmetiyle benzer özellikleri alırsınız. Çok kiracılı Logic Apps hizmetinde bulunan tüm yerleşik Tetikleyicileri, eylemleri ve yönetilen bağlayıcıları kullanabilirsiniz. Bazı yönetilen bağlayıcılar ek ıSE sürümlerini sunar. Ise bağlayıcılar ve ıSE olmayan bağlayıcılar arasındaki fark, çalıştırıldıkları yerde ve bir ıSE içinde çalışırken Logic App Designer 'da sahip oldukları Etiketler arasında bulunur.

![ISE 'de etiketleri olan ve olmayan bağlayıcılar](./media/connect-virtual-network-vnet-isolated-environment-overview/labeled-trigger-actions-integration-service-environment.png)

* Yerleşik Tetikleyiciler ve eylemler **temel** etiketi görüntüler. Bunlar her zaman mantıksal uygulamanız ile aynı ıSE çalıştırırlar. **Ise** etiketini görüntüleyen yönetilen bağlayıcılar, mantıksal UYGULAMANıZLA aynı Ise içinde de çalışır.

  Örneğin, ıSE sürümlerini sunan bazı bağlayıcılar aşağıda verilmiştir:

  * Azure Blob depolama, dosya depolama ve tablo depolama
  * Azure kuyrukları, Azure Service Bus, Azure Event Hubs ve IBM MQ
  * FTP ve SFTP-SSH
  * SQL Server, Azure SQL veri ambarı Azure Cosmos DB
  * AS2, x12 ve EDIOLGU

* Her zaman çok kiracılı Logic Apps hizmetinde herhangi bir ek etiket görüntülememe yönetilen bağlayıcılar, ancak bu bağlayıcıları ıSE barındırılan bir mantıksal uygulamada kullanmaya devam edebilirsiniz.

<a name="on-premises"></a>

### <a name="access-to-on-premises-systems"></a>Şirket içi sistemlere erişim

Bir Azure sanal ağına bağlı şirket içi sistemlere veya veri kaynaklarına erişmek için, bir ıSE içindeki Logic Apps şu öğeleri kullanabilir:

* HTTP eylemi

* O sistem için ıSE etiketli bağlayıcı

  > [!NOTE]
  > Windows kimlik doğrulamasını bir [tümleştirme hizmeti ortamında (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)SQL Server Bağlayıcısı ile kullanmak için, Şirket [içi veri ağ geçidiyle](../logic-apps/logic-apps-gateway-install.md)bağlayıcının Ise sürümünü kullanın. ISE etiketli sürüm Windows kimlik doğrulamasını desteklemez.

* Özel bağlayıcı

  * Şirket içi veri ağ geçidini gerektiren özel bağlayıcılarınız varsa ve bu bağlayıcıları bir ıSE dışında oluşturduysanız, bir ıSE içindeki Logic Apps de bu bağlayıcıları kullanabilir.

  * Bir ıSE içinde oluşturulan özel bağlayıcılar şirket içi veri ağ geçidi ile çalışmaz. Ancak, bu bağlayıcılar, ıSE 'yi barındıran sanal ağa bağlı şirket içi veri kaynaklarına doğrudan erişebilir. Bu nedenle, bir ıSE içindeki Logic Apps, bu kaynaklarla iletişim kurarken veri ağ geçidine ihtiyaç duymamasından kaynaklanıyor olabilir.

Bir sanal ağa bağlı olmayan veya ıSE etiketli bağlayıcılar bulunmayan şirket içi sistemler için, mantıksal uygulamalarınızın bu sistemlere bağlanabilmesi için önce şirket [içi veri ağ geçidini ayarlamanız](../logic-apps/logic-apps-gateway-install.md) gerekir.

<a name="ise-level"></a>

## <a name="ise-skus"></a>ISE SKU 'Ları

ISE 'yi oluşturduğunuzda, geliştirici SKU 'SU veya Premium SKU 'YU seçebilirsiniz. Bu SKU 'Lar arasındaki farklar şunlardır:

* **Geliştirici**

  Deneme, geliştirme ve test için kullanabileceğiniz, ancak üretim ya da performans testi için kullanabileceğiniz düşük maliyetli bir ıSE sağlar. Geliştirici SKU 'SU, sabit bir aylık fiyat için yerleşik Tetikleyiciler ve Eylemler, standart bağlayıcılar, kurumsal bağlayıcılar ve tek bir [ücretsiz katman](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) tümleştirme hesabı içerir. Ancak, bu SKU herhangi bir hizmet düzeyi sözleşmesi (SLA), kapasite ölçekleme seçenekleri veya geri dönüştürme sırasında, gecikme veya kapalı kalma süresi yaşayabileceğiniz anlamına gelir.

* **Premium**

  Üretim için kullanabileceğiniz ve SLA desteği, yerleşik Tetikleyiciler ve Eylemler, standart bağlayıcılar, kurumsal bağlayıcılar, tek bir [Standart katman](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) tümleştirme hesabı, kapasiteyi ölçeklendirmeye yönelik seçenekler ve sabit bir aylık fiyat için geri dönüştürme sırasında yedeklilik dahil olmak üzere bir Ise sağlar.

> [!IMPORTANT]
> SKU seçeneği yalnızca ıSE oluşturma sırasında kullanılabilir ve daha sonra değiştirilemez.

Fiyatlandırma fiyatları için bkz. [Logic Apps fiyatlandırması](https://azure.microsoft.com/pricing/details/logic-apps/). Fiyatlandırma ve faturalandırma işinin nasıl sesleri olduğunu öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın.

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>ISE uç noktası erişimi

ISE 'yi oluşturduğunuzda, iç veya dış erişim uç noktaları kullanmayı tercih edebilirsiniz. Seçiminiz, işinizdeki Logic Apps 'teki istek veya Web kancasının, sanal ağınızın dışından çağrı alıp alamayacağını belirler. Bu uç noktalar Ayrıca, Logic Apps 'in çalışma geçmişinden gelen girişlere ve çıkışlara erişmenin yolunu da etkiler.

> [!IMPORTANT]
> Erişim uç noktasını yalnızca ıSE oluşturma sırasında seçebilir ve bu seçeneği daha sonra değiştiremezsiniz.

* **İç**: özel uç noktalar, ortamınızdaki Logic Apps çağrılarına izin verir ve Logic Apps 'in çalışma geçmişinden gelen giriş ve çıkışları *yalnızca sanal ağınızın*içinden görüntüleyebilir ve bunlara erişebilirsiniz. Özel uç noktalar ve çalıştırma geçmişine erişmek istediğiniz bilgisayar arasında ağ bağlantısına sahip olduğunuzdan emin olun. Örneğin, istemci bilgisayarınız ıSE 'nin sanal ağı içinde veya ıSE 'nin sanal ağına bağlı bir sanal ağ içinde (örneğin, eşleme veya bir sanal özel ağ) bulunabilir.

* **Dış**: genel uç noktaları, *sanal ağınızın dışından*Logic Apps 'in çalışma geçmişinden gelen giriş ve çıkışları görüntüleyebileceğiniz ve erişebileceğiniz, ortamınızda Logic Apps çağrılarına izin verir. Ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, çalıştırma geçmişinin giriş ve çıkışlarına erişime izin vermek için gelen kurallarla ayarlandıklarından emin olun. Daha fazla bilgi için bkz. [Ise için erişimi etkinleştirme](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access).

ISE 'nizin iç veya dış erişim uç noktası kullanıp kullanmadığını belirlemek için, ıSE menüsünde, **Ayarlar**' ın altında **Özellikler**' i seçin ve **erişim uç noktası** özelliğini bulun:

![ISE erişim uç noktası bul](./media/connect-virtual-network-vnet-isolated-environment-overview/find-ise-access-endpoint.png)

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>ISE ile tümleştirme hesapları

Tümleştirme hesaplarını bir tümleştirme hizmeti ortamı (ıSE) içinde Logic Apps ile kullanabilirsiniz. Ancak, bu tümleştirme hesaplarının bağlı Logic Apps ile *aynı Ise* kullanması gerekir. Bir ıSE içindeki Logic Apps yalnızca aynı ıSE içindeki tümleştirme hesaplarına başvurabilir. Bir tümleştirme hesabı oluşturduğunuzda, çalışma hesabınızı tümleştirme hesabınızın konumu olarak seçebilirsiniz. Fiyatlandırma ve faturalandırma 'nin bir ıSE ile tümleştirme hesaplarında nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma fiyatları için bkz. [Logic Apps fiyatlandırması](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* [Azure sanal ağ](../virtual-network/virtual-networks-overview.md) hakkında daha fazla bilgi edinin
* [Azure hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md) hakkında bilgi edinin
