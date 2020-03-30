---
title: Azure sanal ağlarına erişim
description: Entegrasyon hizmeti ortamlarının (ISEs) mantık uygulamalarının Azure sanal ağlarına (VNETs) erişmelerine nasıl yardımcı olduğuna genel bakış
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: 9d5e0c088fe773f16e1fc57f292ca812906aa09c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127256"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Tümleştirme hizmet ortamlarını (ISEs) kullanarak Azure Logic Apps'tan Azure Sanal Ağ kaynaklarına erişim

Bazen, mantıksal uygulamalarınızın azure [sanal ağının](../virtual-network/virtual-networks-overview.md)içinde bulunan sanal makineler (VM'ler) ve diğer sistemler veya hizmetler gibi güvenli kaynaklara erişmeye ihtiyacı vardır. Bu erişimi ayarlamak için [bir *tümleştirme hizmeti ortamı* (Ise) oluşturabilirsiniz.](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) İmKB, özel kaynakları kullanan ve "genel" çok kiracılı Logic Apps hizmetinden ayrı çalışan Logic Apps hizmetinin yalıtılmış bir örneğidir.

Kendi ayrı yalıtılmış örneğinizde mantık uygulamaları çalıştırmak, diğer Azure kiracılarının uygulamalarınızın performansı üzerindeki etkisini azaltmaya yardımcı olur( [ayrıca "gürültülü komşular" etkisi](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors)olarak da bilinir. Bir İmKB da şu avantajları sağlar:

* Çok kiracılı hizmetteki mantık uygulamaları tarafından paylaşılan statik IP adreslerinden ayrı olan kendi statik IP adresleriniz. Ayrıca, hedef sistemlerle iletişim kurmak için tek bir genel, statik ve öngörülebilir giden IP adresi ayarlayabilirsiniz. Bu şekilde, her ise için bu hedef sistemlerde ek güvenlik duvarı açıklıkları ayarlamanız gerekmez.

* Çalışma süresi, depolama bekletme, iş ortası, HTTP isteği ve yanıt zaman zaman ları, ileti boyutları ve özel bağlayıcı isteklerinde artırılmış sınırlar. Daha fazla bilgi için [Azure Mantık Uygulamaları için Sınırlar ve yapılandırma ya](logic-apps-limits-and-config.md)da yapılandırma ya da

> [!NOTE]
> Bazı Azure sanal ağları, Azure Depolama, Azure Cosmos DB veya Azure SQL Veritabanı, iş ortağı hizmetleri veya Azure'da barındırılan müşteri hizmetleri gibi Azure PaaS hizmetlerine erişim sağlamak için özel uç noktaları[(Azure Özel Bağlantı)](../private-link/private-link-overview.md)kullanır. Mantık uygulamalarınızın özel uç noktaları kullanan sanal ağlara erişmesi gerekiyorsa, bu mantık uygulamalarını bir İmKB içinde oluşturmanız, dağıtmanız ve çalıştırmanız gerekir.

Bir İmKB oluşturduğunuzda, Azure, O İmKB'yi Azure sanal ağınıza *enjekte eder* veya dağıtır. Daha sonra bu İmKB'yi, erişılması gereken mantık uygulamaları ve tümleştirme hesapları için konum olarak kullanabilirsiniz.

![Entegrasyon hizmeti ortamını seçin](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Mantık uygulamaları, mantık uygulamalarınızla aynı İmKB'de çalışan bu öğeleri kullanarak sanal ağınızın içindeki veya bağlı kaynaklara erişebilir:

* HTTP tetikleyicisi veya eylem gibi **CORE**etiketli yerleşik tetikleyici veya eylem
* Bu sistem veya hizmet için **İmKB**etiketli bağlayıcı
* Özel bir bağlayıcı

CORE **veya** **ISE** etiketine sahip olmayan konektörleri, İmKB'nizdeki mantık uygulamalarıyla birlikte kullanmaya devam edebilirsiniz. Bu bağlayıcılar bunun yerine çok kiracılı Logic Apps hizmetinde çalışır. Daha fazla bilgi için şu bölümlere bakın:

* [Çok kiracıya karşı yalıtılmış](#difference)
* [Entegrasyon hizmeti ortamından bağlanma](../connectors/apis-list.md#integration-service-environment)
* [İmKB konektörleri](../connectors/apis-list.md#ise-connectors)

> [!IMPORTANT]
> Logic uygulamaları, yerleşik tetikleyiciler, yerleşik eylemler ve İmKB'nizde çalışan bağlayıcılar, tüketim tabanlı fiyatlandırma planından farklı bir fiyatlandırma planı kullanır. Daha fazla bilgi için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma ayrıntıları için [Logic Apps fiyatlandırması'na](../logic-apps/logic-apps-pricing.md)bakın.

Bu genel bakış, Birİm'in mantıksal uygulamalarınıza Azure sanal ağınıza nasıl doğrudan erişim sağladığı hakkında daha fazla bilgiyi açıklar ve Bir İmKB ile çok kiracılı Logic Apps hizmeti arasındaki farkları karşılaştırır.

<a name="difference"></a>

## <a name="isolated-versus-multi-tenant"></a>Çok kiracıya karşı yalıtılmış

Bir İmKB'de mantık uygulamaları oluşturup çalıştırdığınızda, çok kiracılı Logic Apps hizmetiyle aynı kullanıcı deneyimlerine ve benzer özelliklere sahip olursunuz. Çok kiracılı Logic Apps hizmetinde kullanılabilen aynı yerleşik tetikleyicileri, eylemleri ve yönetilen bağlayıcıları kullanabilirsiniz. Bazı yönetilen konektörler ek ISE sürümleri sunar. İmKB konektörleri ile İmKB olmayan konektörler arasındaki fark, çalıştıkları yerde ve Bir İmKB içinde çalışırken Mantık Uygulama Tasarımcısı'nda bulunan etiketlerde bulunur.

![İmKB'de etiketli ve etiketsiz bağlayıcılar](./media/connect-virtual-network-vnet-isolated-environment-overview/labeled-trigger-actions-integration-service-environment.png)

* Yerleşik tetikleyiciler ve eylemler **CORE** etiketini görüntüler. Her zaman mantık uygulamanızla aynı İmKB'de çalışırlar. **İmKB** etiketini görüntüleyen yönetilen konektörler de mantık uygulamanızla aynı İmKB'de çalışır.

  Örneğin, İmKB sürümleri sunan bazı konektörler şunlardır:

  * Azure Blob Depolama, Dosya Depolama ve Tablo Depolama
  * Azure Kuyrukları, Azure Hizmet Veri Önü, Azure Etkinlik Hub'ları ve IBM MQ
  * FTP ve SFTP-SSH
  * SQL Server, Azure SQL Veri Ambarı, Azure Cosmos DB
  * AS2, X12 ve EDIFACT

* Ek etiket görüntülemeyen yönetilen bağlayıcılar her zaman çok kiracılı Logic Apps hizmetinde çalışır, ancak bu bağlayıcıları İmKB tarafından barındırılan bir mantık uygulamasında kullanmaya devam edebilirsiniz.

<a name="on-premises"></a>

### <a name="access-to-on-premises-systems"></a>Şirket içi sistemlere erişim

Bir Azure sanal ağına bağlı şirket içi sistemlere veya veri kaynaklarına erişmek için, Bir İmKB'deki mantık uygulamaları bu öğeleri kullanabilir:

* HTTP eylem

* Bu sistem için İmKB etiketli konektör

  > [!NOTE]
  > [Bir entegrasyon hizmeti ortamında (Ise)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)SQL Server bağlayıcısı ile Windows kimlik doğrulamasını kullanmak için, konektörün Ise olmayan sürümünü şirket içi veri ağ [geçidiyle](../logic-apps/logic-apps-gateway-install.md)kullanın. İmKB etiketli sürüm Windows kimlik doğrulamasını desteklemez.

* Özel bağlayıcı

  * Şirket içi veri ağ geçidi gerektiren özel bağlayıcılarınız varsa ve bu bağlayıcıları Bir İmKB dışında oluşturduysanız, İmKB'deki mantıksal uygulamalar da bu bağlayıcıları kullanabilir.

  * İmKB'de oluşturulan özel konektörler şirket içi veri ağ geçidiyle çalışmaz. Ancak, bu bağlayıcılar İmKB'yi barındıran sanal ağa bağlı şirket içi veri kaynaklarına doğrudan erişebilir. Bu nedenle, Bir İmKB'deki mantık uygulamaları büyük olasılıkla bu kaynaklarla iletişim kurarken veri ağ geçidine ihtiyaç duymaz.

Sanal ağa bağlı olmayan veya İmKB etiketli bağlayıcıları olmayan şirket içi sistemlerde, mantık uygulamalarınız bu sistemlere bağlanabilmesi için önce [şirket içi veri ağ geçidini kurmanız](../logic-apps/logic-apps-gateway-install.md) gerekir.

<a name="ise-level"></a>

## <a name="ise-skus"></a>İmKB SKUs

İmKB'nizi oluşturduğunuzda, Geliştirici SKU veya Premium SKU'yu seçebilirsiniz. Bu SK'ler arasındaki farklar şunlardır:

* **Geliştirici**

  Deneme, geliştirme ve sınama için kullanabileceğiniz, ancak üretim veya performans testi için kullanamadığınız daha düşük maliyetli bir Ise sağlar. Geliştirici SKU yerleşik tetikleyiciler ve eylemler, Standart bağlayıcılar, Kurumsal bağlayıcılar ve sabit bir aylık fiyat için tek bir [Serbest katman](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) tümleştirme hesabı içerir. Ancak, bu SKU herhangi bir hizmet düzeyi sözleşmesi (SLA), kapasiteyi artırmak için seçenekler veya geri dönüşüm sırasında fazlalık içermez, bu da gecikmeler veya kapalı kalma süreleri karşılaşabileceğiniz anlamına gelir.

* **Premium**

  Üretim için kullanabileceğiniz bir İmKB sağlar ve SLA desteği, yerleşik tetikleyiciler ve eylemler, Standart bağlayıcılar, Kurumsal bağlayıcılar, tek bir [Standart katman](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) tümleştirme hesabı, kapasiteyi yükseltme seçenekleri ve sabit bir aylık fiyat karşılığında geri dönüşüm sırasında artıklık içerir.

> [!IMPORTANT]
> SKU seçeneği yalnızca İmKB oluşturmada kullanılabilir ve daha sonra değiştirilemez.

Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps/)bakın. ISE'ler için fiyatlandırma ve faturalandırmanın nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın.

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>İmKB uç nokta erişimi

İmKB'nizi oluşturduğunuzda, dahili veya harici erişim uç noktalarını kullanmayı seçebilirsiniz. Seçiminiz, İmKB'nizdeki mantık uygulamalarındaki istek veya webhook tetikleyicilerinin sanal ağınızın dışından çağrı alıp alamayabileceğini belirler.

Bu uç noktalar, mantık uygulamalarınızın çalışma geçmişindeki giriş ve çıktılara erişme şeklinizi de etkiler.

* **Dahili**: İmKB'inizdeki mantık uygulamalarına yapılan aramalara izin veren ve mantık uygulamalarınızın giriş ve çıktılarını *yalnızca sanal ağınızın içinden* çalıştıran geçmişolarak görüntüleyebileceğiniz ve erişebileceğiniz özel uç noktalar

* **Harici**: Sanal *ağınızın dışından*çalışma geçmişindeki mantık uygulamalarınızın giriş ve çıkışlarını görüntüleyip erişebileceğiniz İmKB'inizdeki mantık uygulamalarına çağrılara izin veren genel uç noktalar. Ağ güvenlik grupları (NSGs) kullanıyorsanız, çalışma geçmişinin giriş ve çıktılarına erişimsağlamak için gelen kurallarla ayarlandıklarından emin olun. Daha fazla bilgi [Enable access for ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access)için bk.

> [!IMPORTANT]
> Erişim bitiş noktası seçeneği yalnızca Ise oluşturmada kullanılabilir ve daha sonra değiştirilemez.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>İmKB ile entegrasyon hesapları

Entegrasyon hizmeti ortamı (İmKB) içinde mantık uygulamaları olan tümleştirme hesaplarını kullanabilirsiniz. Ancak, bu tümleştirme hesapları bağlantılı mantık uygulamaları *ile aynı İmKB'yi* kullanmalıdır. İmKB'deki mantık uygulamaları yalnızca aynı İmKB'deki tümleştirme hesaplarına başvurur. Bir entegrasyon hesabı oluşturduğunuzda, Bİm'inizi entegrasyon hesabınızın konumu olarak seçebilirsiniz. Bir İmKB'li entegrasyon hesapları için fiyatlandırma ve faturalandırmanın nasıl çalıştığını öğrenmek için [Logic Apps fiyatlandırma modeline](../logic-apps/logic-apps-pricing.md#fixed-pricing)bakın. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps'tan Azure sanal ağlarına bağlanma](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* [Azure Sanal Ağı](../virtual-network/virtual-networks-overview.md) hakkında daha fazla bilgi edinin
* Azure [hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md) hakkında bilgi edinin
