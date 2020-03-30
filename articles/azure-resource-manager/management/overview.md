---
title: Genel Bakış
description: Azure’daki kaynakların dağıtımı, yönetimi ve erişim denetimi için Azure Resource Manager’ın nasıl kullanılacağı açıklanmaktadır.
ms.topic: overview
ms.date: 03/25/2020
ms.openlocfilehash: 1e2a6959117749b4e7d08a9768b4189b97ef08bd
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80258150"
---
# <a name="what-is-azure-resource-manager"></a>Azure Kaynak Yöneticisi nedir?

Azure Resource Manager, Azure için dağıtım ve yönetim hizmetidir. Azure aboneliğinizde kaynak oluşturmanıza, güncelleştirmenize ve silmenize olanak tanıyan bir yönetim katmanı sağlar. Dağıtımdan sonra kaynaklarınızı güvenli hale getirmek ve düzenlemek için erişim denetimi, kilitler ve etiketler gibi yönetim özelliklerini kullanırsınız.

Azure Kaynak Yöneticisi şablonları hakkında bilgi edinmek için [Şablon dağıtımına genel bakış](../templates/overview.md)'a bakın.

## <a name="consistent-management-layer"></a>Tutarlı yönetim katmanı

Bir kullanıcı Azure araçlarından, API'lerden veya SDK'lardan herhangi birinden istek gönderdiğinde, Kaynak Yöneticisi isteği alır. İsteğin doğruluğunu doğrular ve onaylar. Kaynak Yöneticisi isteği Azure hizmetine gönderir ve bu da istenen eylemi alır. Tüm istekler aynı API aracılığıyla işlendiğinden, tüm farklı araçlarda tutarlı sonuçlar ve yetenekler görürsünüz.

Aşağıdaki resim, Azure Kaynak Yöneticisi'nin Azure isteklerini işlemede oynadığı rolü gösterir.

![Resource Manager istek modeli](./media/overview/consistent-management-layer.png)

Portalda bulunan tüm özellikler PowerShell, Azure CLI, REST API'leri ve istemci SDK'ları aracılığıyla da kullanılabilir. İlk olarak API'lerle başlatılan işlevler 180 gün içinde portalda kullanıma sunulacaktır.

## <a name="terminology"></a>Terminoloji

Azure Resource Manager’ı kullanmaya yeni başladıysanız bilmiyor olabileceğiniz bazı terimler vardır.

* **kaynak** - Azure üzerinden kullanılabilen yönetilebilir bir öğe. Sanal makineler, depolama hesapları, web uygulamaları, veritabanları ve sanal ağlar kaynak örnekleridir.
* **kaynak grubu** - Azure çözümü için ilgili kaynakları tutan bir kapsayıcı. Kaynak grubu, grup olarak yönetmek istediğiniz kaynakları içerir. Kuruluşunuz açısından en mantıklı duruma göre hangi kaynakların bir kaynak grubuna ait olduğuna siz karar verirsiniz. [Bkz. Kaynak grupları.](#resource-groups)
* **kaynak sağlayıcısı** - Azure kaynakları sağlayan bir hizmet. Örneğin, sanal makine kaynağını sağlayan microsoft.compute ortak bir kaynak sağlayıcısıdır. Microsoft.Storage başka bir ortak kaynak sağlayıcısıdır. [Bkz. Kaynak sağlayıcıları ve türleri.](resource-providers-and-types.md)
* **Kaynak Yöneticisi şablonu** - Kaynak grubuna veya aboneye dağıtılabilmek için bir veya daha fazla kaynak tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyası. Şablon, kaynakları tutarlı ve sürekli olarak dağıtmak için kullanılabilir. Bkz. [Şablon dağıtımına genel bakış](../templates/overview.md).
* **bildirim temelli söz dizimi** - Oluşturmaya yönelik programlama komutları dizisini yazmak zorunda kalmadan "Oluşturmak istediğiniz şeyi" belirtmenize imkan tanıyan söz dizimi. Resource Manager şablonu, bildirim temelli söz diziminin bir örneğidir. Dosya içinde Azure’a dağıtılacak altyapının özelliklerini tanımlarsınız.  Bkz. [Şablon dağıtımına genel bakış](../templates/overview.md).

## <a name="the-benefits-of-using-resource-manager"></a>Resource Manager’ı kullanmanın avantajları

Kaynak Yöneticisi ile şunları yapabilirsiniz:

* Altyapınızı komut dosyaları yerine bildirim şablonları aracılığıyla yönetin.

* Bu kaynakları tek tek işlemek yerine, çözümünüzün tüm kaynaklarını grup olarak dağıtın, yönetin ve izleyin.

* Çözümünüzü geliştirme yaşam döngüsü boyunca yeniden dağıtın ve kaynaklarınızın tutarlı bir durumda dağıtıldıklarından emin olun.

* Doğru sırada dağıtılmak üzere kaynaklar arasındaki bağımlılıkları tanımlayın.

* Role Tabanlı Erişim Denetimi (RBAC) yerel olarak yönetim platformuna entegre olduğundan kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayın.

* Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklara etiketler uygulayın.

* Aynı etiketi paylaşan bir grup kaynağın maliyetlerini görüntüleyerek kuruluşunuzun faturalandırmasını netleştirin.

## <a name="understand-scope"></a>Kapsamı anlama

Azure dört kapsam düzeyi sağlar: [yönetim grupları,](../../governance/management-groups/overview.md)abonelikler, [kaynak grupları](#resource-groups)ve kaynaklar. Aşağıdaki resimde bu katmanlara ait bir örnek gösterilir.

![Kapsam](./media/overview/scope-levels.png)

Yönetim ayarlarını bu kapsam düzeylerinden birinde uygularsınız. Seçtiğiniz düzey, ayarın ne kadar yaygın olarak uygulanacağını belirler. Düşük düzeyler, yüksek düzeylerdeki ayarları devralır. Örneğin, aboneye bir [ilke](../../governance/policy/overview.md) uyguladığınızda, ilke aboneliğinizdeki tüm kaynak gruplarına ve kaynaklara uygulanır. Kaynak grubuna bir ilke uyguladığınız zaman, bu ilke kaynak grubu ve tüm kaynakları uygulanır. Ancak, başka bir kaynak grubunun bu ilke ataması yoktur.

Şablonları yönetim gruplarına, aboneliklere veya kaynak gruplarına dağıtabilirsiniz.

## <a name="resource-groups"></a>Kaynak grupları

Kaynak gruplarınızı tanımlarken göz önüne almanız gereken bazı önemli faktörler bulunur:

* Grubunuzdaki tüm kaynaklar aynı yaşam döngüsünü paylaşmalıdır. Bunları birlikte dağıtır, güncelleştirir ve silersiniz. Veritabanı sunucusu gibi bir kaynağın farklı bir dağıtım döngüsünde bulunması gerekiyorsa, bu kaynak farklı bir kaynak grubuna konulmalıdır.

* Her kaynak yalnızca bir kaynak grubunda yer alabilir.

* Bir kaynak grubuna dilediğiniz zaman kaynak ekleyebilir veya kaldırabilirsiniz.

* Bir kaynağı, bir kaynak grubundan bir diğerine taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

* Kaynak grubu, farklı bölgelerde bulunan kaynaklar içerebilir.

* Bir kaynak grubu, yönetim eylemleri için erişim denetimini incelemek üzere kullanılabilir.

* Bir kaynak diğer kaynak gruplarındaki kaynaklarla etkileşim kurabilir. İki kaynak ilişkili olmasına karşın aynı yaşam döngüsünü paylaşmadığında (örneğin, bir veritabanına bağlanan web uygulamaları) bu etkileşim yaygın olarak görülür.

Bir kaynak grubu oluştururken bu kaynak grubu için bir konum belirtmeniz gerekir. "Bir kaynak grubu için neden konum gerekli olsun? Ayrıca kaynaklar kaynak grubundan farklı konumlarda olabiliyorsa kaynak grubu konumu neden önemli olsun?" diye soruyor olabilirsiniz Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğiniz zaman, bu meta verilerin nerede depolandığınızı belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubunun bölgesi geçici olarak kullanılamıyorsa, meta veriler kullanılamadığı ndan kaynak grubundaki kaynakları güncelleştiremezsiniz. Diğer bölgelerdeki kaynaklar beklendiği gibi çalışmaya devam eder, ancak bunları güncelleştiremezsiniz. Güvenilir uygulamalar oluşturma hakkında daha fazla bilgi [için](/azure/architecture/checklist/resiliency-per-service)bkz.

## <a name="resiliency-of-azure-resource-manager"></a>Azure Kaynak Yöneticisinin Esnekliği

Azure Kaynak Yöneticisi hizmeti esneklik ve sürekli kullanılabilirlik için tasarlanmıştır. REST API'sında Kaynak Yöneticisi ve kontrol düzlemi işlemleri (management.azure.com gönderilen istekler) şunlardır:

* Bölgeler arasında dağıtılır. Bazı hizmetler bölgeseldir.

* Birden çok Kullanılabilirlik Bölgesi olan konumlarda Kullanılabilirlik Bölgeleri (ve bölgeler) arasında dağıtılır.

* Tek bir mantıksal veri merkezine bağlı değil.

* Bakım faaliyetleri için asla indirilme.

Bu esneklik, Kaynak Yöneticisi aracılığıyla istek alan hizmetler için geçerlidir. Örneğin, Key Vault bu esneklikten yararlanır.

## <a name="next-steps"></a>Sonraki adımlar

* Kaynakları taşıma hakkında bilgi edinmek [için](move-resource-group-and-subscription.md)bkz.

* Kaynakları etiketleme hakkında bilgi edinmek için [Azure kaynaklarınızı düzenlemek için etiketleri kullan'a](tag-resources.md)bakın.

* Kaynakları kilitleme hakkında bilgi edinmek için [beklenmeyen değişiklikleri önlemek için kaynakları kilitle'ye](lock-resources.md)bakın.
