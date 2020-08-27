---
title: Portalda arama hizmeti oluşturma
titleSuffix: Azure Cognitive Search
description: Bu portal hızlı başlangıcı ' nda, Azure portal Azure Bilişsel Arama kaynağını ayarlamayı öğrenin. Kaynak grupları, bölgeler ve SKU veya fiyatlandırma katmanı seçin.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 07/14/2020
ms.openlocfilehash: 6813db0b7416695fb9433a701114f2684dd0e122
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88927128"
---
# <a name="quickstart-create-an-azure-cognitive-search-service-in-the-portal"></a>Hızlı Başlangıç: Portalda Azure Bilişsel Arama hizmeti oluşturma

Azure Bilişsel Arama, bir arama deneyimini özel uygulamalara bağlamak için kullanılan tek başına kaynaktır. Bilişsel Arama, diğer Azure hizmetleriyle, ağ sunucularındaki uygulamalarla veya diğer bulut platformlarında çalışan yazılımlarla kolayca tümleştirilir.

Bu makalede [Azure Portal](https://portal.azure.com/)kaynak oluşturmayı öğrenin.

[![Animasyonlu GIF](./media/search-create-service-portal/AnimatedGif-AzureSearch-small.gif)](./media/search-create-service-portal/AnimatedGif-AzureSearch.gif#lightbox)

PowerShell’i mi tercih ediyorsunuz? Azure Resource Manager [hizmet şablonunu](https://azure.microsoft.com/resources/templates/101-azure-search-create/) kullanın. Kullanmaya başlama hakkında yardım için bkz. [PowerShell Ile Azure bilişsel arama yönetme](search-manage-powershell.md).

## <a name="before-you-start"></a>Başlamadan önce

Aşağıdaki hizmet özellikleri, hizmetin kullanım ömrü boyunca düzeltilir; bunlardan herhangi birini değiştirmek yeni bir hizmet gerektirir. Düzeltildiklerinden, her bir özelliği doldururken kullanım etkilerini göz önünde bulundurun:

* Hizmet adı URL uç noktasının bir parçası olur (faydalı hizmet adları için[ipuçları gözden geçirin](#name-the-service) ).
* Hizmet katmanı, [faturalandırmayı etkiler](search-sku-tier.md) ve kapasite üzerinde yukarı bir sınır ayarlar. Ücretsiz katmanda bazı özellikler kullanılamaz.
* Hizmet bölgesi belirli senaryoların kullanılabilirliğini tespit edebilir. [Yüksek güvenlik özellikleri](search-security-overview.md) veya [AI zenginleştirmesi](cognitive-search-concept-intro.md)gerekiyorsa, Azure bilişsel arama diğer hizmetlerle aynı bölgeye veya söz konusu özelliği sağlayan bölgelere yerleştirmeniz gerekir. 

## <a name="subscribe-free-or-paid"></a>Abone olma (ücretsiz veya ücretli)

[Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve ücretli Azure hizmetlerini denemek için ücretsiz krediler kullanın. Krediler bittikten sonra hesabı tutun ve Web Siteleri gibi ücretsiz Azure hizmetlerini kullanmaya devam edin. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak, [MSDN abone avantajlarınızı etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). MSDN aboneliği size her ay ücretli Azure hizmetleri için kullanabileceğiniz krediler verir. 

## <a name="find-azure-cognitive-search"></a>Azure Bilişsel Arama bulun

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Sol üst köşedeki artı işaretine ("+ kaynak oluştur") tıklayın.

1. "Azure bilişsel arama" bulmak için arama çubuğunu kullanın veya **Web**  >  **Azure bilişsel arama**üzerinden kaynağa gidin.

![Portalda kaynak oluşturma](./media/search-create-service-portal/find-search3.png "Portalda kaynak oluşturma")

## <a name="choose-a-subscription"></a>Abonelik seçin

Birden fazla aboneliğiniz varsa, arama hizmetiniz için bir tane seçin. Yönetilen hizmet kimliklerine bağlı bir [Çift şifreleme](search-security-overview.md#double-encryption) veya diğer özellikler uygulamadıysanız, yönetilen kimliklerin kullanıldığı Azure Key Vault veya diğer hizmetler için kullanılan aboneliği seçin.

## <a name="set-a-resource-group"></a>Kaynak grubu ayarlama

Kaynak grubu, Azure çözümünüz için ilgili kaynakları tutan bir kapsayıcıdır. Arama hizmeti için gereklidir. Ayrıca, maliyetleri dahil olmak üzere kaynakları yönetmek için de kullanışlıdır. Bir kaynak grubu, bir hizmetten veya birlikte kullanılan birden fazla hizmetten oluşabilir. Örneğin, Azure Cosmos DB bir veritabanının dizinini oluşturmak için Azure Bilişsel Arama kullanıyorsanız, her iki hizmeti de yönetim amaçlarıyla aynı kaynak grubunun parçası yapabilirsiniz. 

Kaynakları tek bir grupta birleştirmemişse veya mevcut kaynak grupları ilişkisiz çözümlerde kullanılan kaynaklarla doldurulduysa, yalnızca Azure Bilişsel Arama kaynağınız için yeni bir kaynak grubu oluşturun. 

![Yeni bir kaynak grubu oluşturma](./media/search-create-service-portal/new-resource-group.png "Yeni bir kaynak grubu oluşturma")

Zaman içinde, geçerli ve öngörülen maliyetleri tamamen izleyebilir veya tek tek kaynaklar için ücretleri görüntüleyebilirsiniz. Aşağıdaki ekran görüntüsünde, birden çok kaynağı tek bir grupta birleştirdiğinizde görmeyi bekletirebileceğiniz maliyet bilgilerinin türü gösterilmektedir.

![Kaynak grubu düzeyinde maliyetleri yönetin](./media/search-create-service-portal/resource-group-cost-management.png "Kaynak grubu düzeyinde maliyetleri yönetin")

> [!TIP]
> Kaynak grupları temizleme işlemini basitleştirir çünkü bir grup silindiğinde, içindeki tüm hizmetler silinir. Birden fazla hizmet kullanan prototip projeler için, tüm bunların aynı kaynak grubuna yerleştirilmesi, proje bittikten sonra temizleme işlemini kolaylaştırır.

## <a name="name-the-service"></a>Hizmeti adlandırın

Örnek ayrıntıları ' nda **URL** alanında bir hizmet adı belirtin. Ad, API çağrılarının verildiği URL uç noktasının bir parçasıdır: `https://your-service-name.search.windows.net` . Örneğin, uç noktanın olmasını istiyorsanız `https://myservice.search.windows.net` , girmeniz gerekir `myservice` .

Hizmet adı gereksinimleri:

* search.windows.net ad alanı içinde benzersiz olmalıdır
* Uzunluğu 2 ile 60 karakter arasında olmalıdır
* Küçük harf, rakam veya kısa çizgi ("-") kullanmanız gerekir
* İlk 2 karakterde veya son tek karakter olarak tire ("-") kullanmayın
* Her yerde ardışık çizgiler ("--") kullanamazsınız

> [!TIP]
> Birden çok hizmet kullandığınızı düşünüyorsanız, hizmet adına bölge (veya konum) bir adlandırma kuralı olarak dahil etmenizi öneririz. Aynı bölgedeki hizmetler ücretsiz olarak verileri değiş tokuş edebilir, bu nedenle Azure Bilişsel Arama Batı ABD ve aynı zamanda Batı ABD de başka hizmetleriniz varsa, gibi bir ad, `mysearchservice-westus` kaynakları birleştirme veya iliştirme konusunda karar verirken Özellikler sayfasına seyahat edebilir.

## <a name="choose-a-location"></a>Konum seçin

Azure Bilişsel Arama çoğu bölgede kullanılabilir. Desteklenen bölgelerin listesi [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulunabilir.

> [!Note]
> Orta Hindistan ve BAE Kuzey şu anda yeni hizmetlerde kullanılamıyor. Bu bölgelerde zaten bulunan hizmetler için, kısıtlama olmadan ölçeği ölçeklendirebilirsiniz ve hizmetiniz bu bölgede tam olarak desteklenmektedir. Kısıtlamalar geçicidir ve yalnızca yeni hizmetlerle sınırlıdır. Kısıtlamalar artık uygulanmadıysa bu notun kaldırılması gerekir.
>
> Çift şifreleme yalnızca belirli bölgelerde kullanılabilir. Daha fazla bilgi için bkz. [Çift şifreleme](search-security-overview.md#double-encryption).

### <a name="requirements"></a>Gereksinimler

 AI zenginleştirme kullanıyorsanız, arama hizmetinizi bilişsel hizmetler ile aynı bölgede oluşturun. *Azure bilişsel arama ve bilişsel hizmetler 'in aynı bölgedeki birlikte bulunması, AI zenginleştirme için bir gereksinimdir*.

 İş sürekliliği ve olağanüstü durum kurtarma (BCDR) gereksinimlerinin bulunduğu müşterilerin hizmetlerini [Bölgesel çiftlerde](../best-practices-availability-paired-regions.md#azure-regional-pairs)oluşturması gerekir. Örneğin, Kuzey Amerika çalıştırıyorsanız, her hizmet için Doğu ABD ve Batı ABD ya da Orta Kuzey ABD ve Güney Centra US ' i seçebilirsiniz.

### <a name="recommendations"></a>Öneriler

Birden çok Azure hizmeti kullanıyorsanız, veri veya uygulama hizmetinizi de barındıran bir bölge seçin. Bunun yapılması, giden veriler için bant genişliği ücretlerini en aza indirir veya azaltır (hizmetler aynı bölgedeyse giden veriler için ücret alınmaz).

## <a name="choose-a-pricing-tier-sku"></a>Fiyatlandırma Katmanı (SKU) seçin

[Azure bilişsel arama şu anda birden çok fiyatlandırma katmanlarında sunulmaktadır](https://azure.microsoft.com/pricing/details/search/): ücretsiz, temel veya standart. Her katmanın kendi [kapasitesi ve sınırları](search-limits-quotas-capacity.md) vardır. Yönergeler için [Fiyatlandırma katmanı veya SKU seçme](search-sku-tier.md) bölümüne bakın.

Temel ve standart, üretim iş yükleri için en yaygın seçimlerdir, ancak çoğu müşteri ücretsiz hizmetle başlar. Katmanlar arasındaki temel farklılıklar bölüm boyutudur ve hızlardır ve oluşturabileceğiniz nesne sayısı için sınırlar vardır.

Hizmet oluşturulduktan sonra bir fiyatlandırma katmanının değiştirilemeyeceğini unutmayın. Daha yüksek veya daha düşük bir katmana ihtiyacınız varsa, hizmeti yeniden oluşturmanız gerekecektir.

## <a name="create-your-service"></a>Sitenizi oluşturma

Gerekli girişleri sağladıktan sonra, devam edin ve hizmeti oluşturun. 

![Hizmeti gözden geçirin ve oluşturun](./media/search-create-service-portal/new-service3.png "Hizmeti gözden geçirin ve oluşturun")

Hizmetiniz dakikalar içinde dağıtılır. İlerlemeyi Azure bildirimleri aracılığıyla izleyebilirsiniz. Gelecekte kolay erişim sağlamak için hizmeti panonuza sabitlemeyi göz önünde bulundurun.

![Hizmeti izleme ve sabitleme](./media/search-create-service-portal/monitor-notifications.png "Hizmeti izleme ve sabitleme")

## <a name="get-a-key-and-url-endpoint"></a>Anahtar ve URL uç noktası al

Portalı kullanmıyorsanız, yeni hizmetinize programlı erişim, URL uç noktasını ve bir kimlik doğrulama API anahtarı sağlamanızı gerektirir.

1. **Genel bakış** sayfasında, sayfanın sağ tarafındaki URL uç noktasını bulup kopyalayın.

2. **Anahtarlar** sayfasında, yönetici anahtarlarından birini (eşdeğerdir) kopyalayın. Yönetim API 'si-hizmetinize nesneleri oluşturmak, güncelleştirmek ve silmek için anahtarlar gereklidir. Buna karşılık sorgu anahtarları, Dizin içeriğine okuma erişimi sağlar.

   ![URL uç noktası ile hizmete genel bakış sayfası](./media/search-create-service-portal/get-url-key.png "URL uç noktası ve diğer hizmet ayrıntıları")

Portal tabanlı görevler için bir uç nokta ve anahtar gerekli değildir. Portal, Azure Bilişsel Arama kaynağına yönetici haklarıyla zaten bağlı. Portal Kılavuzu için [hızlı başlangıç: portalda bir Azure bilişsel arama dizini oluşturun](search-get-started-portal.md).

## <a name="scale-your-service"></a>Hizmetinizi ölçeklendirme

Hizmetiniz sağlandıktan sonra ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilebilir. Azure Bilişsel Arama hizmetiniz için standart katmanı seçerseniz, hizmetinizi iki boyutta ölçeklendirebilirsiniz: çoğaltmalar ve bölümler. Temel katmanı seçtiyseniz yalnızca çoğaltmalar ekleyebilirsiniz. Ücretsiz hizmeti sağladıysanız ölçek kullanılamaz.

***Bölümler***, hizmetinizin daha fazla belge depolamasına ve daha fazla belgede arama yapmasına olanak sağlar.

***Çoğaltmalar***, hizmetinizin daha yüksek arama sorgusu yükünü işlemesine olanak sağlar.

Kaynak eklemek aylık faturanız artırır. [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/), fatura konusunda kaynak eklemenin getirdiği sonuçları anlamanıza yardımcı olabilir. Kaynakları yüke göre ayarlayabildiğinizi unutmayın. Örneğin, tam bir ilk dizin oluşturmak için kaynakları artırabilir ve ardından artımlı dizin oluşturmak için daha uygun bir düzeye indirebilirsiniz.

> [!Important]
> Bir hizmetin [salt okunur SLA için 2 çoğaltması ve okuma/yazma SLA’sı için 3 çoğaltması](https://azure.microsoft.com/support/legal/sla/search/v1_0/) olmalıdır.

1. Azure portalında arama hizmeti sayfanıza gidin.
2. Sol gezinti bölmesinde **Ayarlar**  >  **Ölçek**' i seçin.
3. Her iki türdeki kaynakları eklemek için kaydırma çubuğunu kullanın.

![Kapasite Ekle](./media/search-create-service-portal/settings-scale.png "Çoğaltmalar ve bölümler aracılığıyla kapasite ekleme")

> [!Note]
> Bölüm başına depolama ve hız daha yüksek katmanlarda artar. Daha fazla bilgi için bkz. [Kapasite ve sınırlar](search-limits-quotas-capacity.md).

## <a name="when-to-add-a-second-service"></a>Ne zaman ikinci bir hizmet eklenir?

Çoğu müşteri [, doğru kaynak dengesini](search-sku-tier.md)sağlayan bir katmanda sağlanan tek bir hizmeti kullanır. Bir hizmet, her bir dizinin diğerinden yalıtıldığı, [seçtiğiniz maksimum katman sınırlarına](search-capacity-planning.md) tabi olan birden fazla dizin barındırabilir. Azure Bilişsel Arama, istekler yalnızca bir dizine yönlendirilebilir ve aynı hizmette bulunan diğer dizinlerden yanlışlıkla veya kasıtlı olarak veri alma olasılığını en aza indirir.

Müşterilerin çoğu yalnızca bir hizmet kullansa da, işletim gereksinimleri arasında aşağıdakiler yer alıyorsa hizmet yedekliliği gerekebilir:

+ [İş sürekliliği ve olağanüstü durum kurtarma (BCDR)](../best-practices-availability-paired-regions.md). Azure Bilişsel Arama, kesinti durumunda anlık yük devretme sağlamaz.

+ [Çok kiracılı mimariler](search-modeling-multitenant-saas-applications.md) bazen iki veya daha fazla hizmet için çağrı yapılır.

+ Küresel olarak dağıtılan uygulamalar, gecikme süresini en aza indirmek için her Coğrafya içinde arama hizmetleri gerektirebilir.

> [!NOTE]
> Azure Bilişsel Arama, dizin oluşturma ve sorgulama işlemlerini ayırt edemez; Bu nedenle, ayrılmış iş yükleri için hiçbir şekilde birden çok hizmet oluşturmazsınız. Bir dizin her zaman oluşturulduğu hizmette sorgulanır (bir hizmette bir dizini oluşturup başka bir hizmete kopyalayamazsınız).

Yüksek düzeyde kullanılabilirlik için ikinci bir hizmet gerekmez. Aynı hizmette 2 veya daha fazla çoğaltma kullandığınızda sorguların yüksek kullanılabilirliği elde edilir. Çoğaltma güncelleştirmeleri sıralıdır, bu da hizmet güncelleştirmesi kullanıma alındığında en az bir işlem çalışır. Çalışma süresi hakkında daha fazla bilgi için bkz. [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Sonraki adımlar

Bir hizmet sağlamaktan sonra, ilk dizininizi oluşturmak için portalda devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: portalda Azure Bilişsel Arama dizini oluşturma](search-get-started-portal.md)