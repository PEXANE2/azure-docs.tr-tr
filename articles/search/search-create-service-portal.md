---
title: Portalda bir Azure Search hizmeti oluşturun-Azure Search
description: Azure portal bir Azure Search kaynağı sağlayın. Kaynak grupları, bölgeler ve SKU veya fiyatlandırma katmanı seçin.
manager: nitinme
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 08/09/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 0aabf10b3be8f2f13abbba15d514a008f193646c
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69649941"
---
# <a name="create-an-azure-search-service-in-the-portal"></a>Portalda Azure Search hizmeti oluşturma

Azure Search, özel uygulamalarda arama deneyimine bağlanmak için kullanılan tek başına kaynaktır. Azure Search diğer Azure hizmetleriyle kolayca tümleştirilebilse de, bunu tek başına bir bileşen olarak kullanabilir veya ağ sunucularındaki uygulamalarla veya diğer bulut platformlarında çalışan yazılımlarla tümleştirebilirsiniz.

Bu makalede, [Azure portal](https://portal.azure.com/)Azure Search kaynağı oluşturmayı öğrenin.

[![Animasyonlu GIF](./media/search-create-service-portal/AnimatedGif-AzureSearch-small.gif)](./media/search-create-service-portal/AnimatedGif-AzureSearch.gif#lightbox)

PowerShell’i mi tercih ediyorsunuz? Azure Resource Manager [hizmet şablonunu](https://azure.microsoft.com/resources/templates/101-azure-search-create/) kullanın. Kullanmaya başlama hakkında yardım için bkz. [PowerShell ile Azure Search yönetme](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Abone olma (ücretsiz veya ücretli)

[Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve ücretli Azure hizmetlerini denemek için ücretsiz krediler kullanın. Krediler bittikten sonra hesabı tutun ve Web Siteleri gibi ücretsiz Azure hizmetlerini kullanmaya devam edin. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak, [MSDN abone avantajlarınızı etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). MSDN aboneliği size her ay ücretli Azure hizmetleri için kullanabileceğiniz krediler verir. 

## <a name="find-azure-search"></a>Azure Search’ü bulma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol üst köşedeki artı işaretine ("+ kaynak oluştur") tıklayın.
3. "Azure Search" bulmak için arama çubuğunu kullanın veya **Web** > **Azure Search**aracılığıyla kaynağa gidin.

![Azure Search kaynağına gitme](./media/search-create-service-portal/find-search3.png "Azure Search Için gezinti yolu")

## <a name="select-a-subscription"></a>Bir abonelik seçin

Birden fazla aboneliğiniz varsa, veri veya dosya depolama hizmetleri de içeren bir abonelik seçin. Azure Search, Dizin [*oluşturucular*](search-indexer-overview.md)aracılığıyla Dizin oluşturma Için Azure Tablo ve BLOB depolama, SQL veritabanı ve Azure Cosmos DB otomatik olarak açabilir, ancak yalnızca aynı abonelikteki hizmetler için.

## <a name="set-a-resource-group"></a>Kaynak grubu ayarlama

Kaynak grubu gereklidir ve maliyet yönetimi dahil olmak üzere kaynakları yönetmek için faydalıdır. Bir kaynak grubu, bir hizmetten veya birlikte kullanılan birden fazla hizmetten oluşabilir. Örneğin, bir Azure Cosmos DB veritabanının dizinini oluşturmak için Azure Search kullanıyorsanız, her iki hizmeti de yönetim amaçlarıyla aynı kaynak grubunun bir parçası yapabilirsiniz. 

Kaynakları tek bir grupta birleştirmemişse veya mevcut kaynak grupları ilişkisiz çözümlerde kullanılan kaynaklarla doldurulduysa, yalnızca Azure Search kaynağınız için yeni bir kaynak grubu oluşturun. 

Hizmeti kullanırken, geçerli ve tasarlanan maliyetleri (ekran görüntüsünde gösterildiği gibi) izleyebilir veya tek tek kaynakların ücretlerini görüntülemek için aşağı kaydırın.

![Kaynak grubu düzeyinde maliyetleri yönetin](./media/search-create-service-portal/resource-group-cost-management.png "Kaynak grubu düzeyinde maliyetleri yönetin")

> [!TIP]
> Bir kaynak grubu silindiğinde, o kaynak grubunun içindeki hizmetler de silinir. Birden fazla hizmet kullanan prototip projeler için, tüm bunların aynı kaynak grubuna yerleştirilmesi, proje bittikten sonra temizleme işlemini kolaylaştırır.

## <a name="name-the-service"></a>Hizmeti adlandırın

Örnek ayrıntıları ' nda **URL** alanında bir hizmet adı belirtin. Ad, API çağrılarının verildiği URL uç noktasının bir parçasıdır: `https://your-service-name.search.windows.net`. Örneğin, uç noktanın olmasını `https://myservice.search.windows.net`istiyorsanız, girmeniz `myservice`gerekir.

Hizmet adı gereksinimleri:

* search.windows.net ad alanı içinde benzersiz olmalıdır
* 2 ila 60 karakter uzunluğunda olmalıdır
* Küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın
* İlk 2 karakter olarak veya sondaki tek karakter olarak tire ("-") kullanmaktan kaçının
* Hiçbir yerde art arda tire ("--") kullanmayın

> [!TIP]
> Birden çok hizmet kullandığınızı düşünüyorsanız, hizmet adına bölge (veya konum) bir adlandırma kuralı olarak dahil etmenizi öneririz. Aynı bölgedeki hizmetler ücretsiz olarak verileri değiş tokuş edebilir, bu nedenle Azure Search Batı ABD ve diğer hizmetleriniz Batı ABD de varsa, gibi `mysearchservice-westus` bir ad, kaynakları birleştirme veya iliştirme konusunda karar verirken Özellikler sayfasına seyahat edebilir.

## <a name="choose-a-location"></a>Konum seçin

Azure hizmeti olarak Azure Search, dünyanın dört bir yanındaki veri merkezlerinde barındırılabilir. Desteklenen bölgelerin listesi [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulunabilir. 

Birden fazla hizmet için aynı konumu seçerek bant genişliği ücretlerini en aza indirebilir veya önleyebilirsiniz. Örneğin, başka bir Azure hizmeti (Azure depolama, Azure Cosmos DB, Azure SQL veritabanı) tarafından sağlanmış verileri dizinlendirirken, Azure Search hizmetinizi aynı bölgede oluşturmak bant genişliği ücretlerini önler (hizmetler içinde olduğunda giden veriler için ücret alınmaz). aynı bölge).

Ek olarak, bilişsel arama AI 'nin zenginleştirme kullanıyorsanız, hizmetinizi bilişsel hizmetler kaynağınız ile aynı bölgede oluşturun. *Aynı bölgedeki Azure Search ve bilişsel hizmetlerin birlikte bulunması, AI zenginleştirme gereksinimidir*.

> [!Note]
> Orta Hindistan, yeni hizmetler için şu anda kullanılamıyor. Zaten Orta Hindistan olan hizmetler için, kısıtlama olmadan ölçeği ölçeklendirebilirsiniz ve hizmetiniz bu bölgede tam olarak desteklenmektedir. Bu bölgedeki kısıtlama geçicidir ve yalnızca yeni hizmetlerle sınırlıdır. Kısıtlama artık geçerli olmadığında bu notun kaldırılması gerekir.

## <a name="choose-a-pricing-tier-sku"></a>Fiyatlandırma Katmanı (SKU) seçin

[Azure Search Şu anda birden çok fiyatlandırma katmanlarında sunulmaktadır](https://azure.microsoft.com/pricing/details/search/): Ücretsiz, temel veya standart. Her katmanın kendi [kapasitesi ve sınırları](search-limits-quotas-capacity.md) vardır. Yönergeler için [Fiyatlandırma katmanı veya SKU seçme](search-sku-tier.md) bölümüne bakın.

Temel ve standart, üretim iş yükleri için en yaygın seçimlerdir, ancak çoğu müşteri ücretsiz hizmetle başlar. Katmanlar arasındaki temel farklılıklar bölüm boyutu ve hız, oluşturabileceğiniz nesne sayısı için sınırlar olur.

Hizmet oluşturulduktan sonra bir fiyatlandırma katmanının değiştirilemeyeceğini unutmayın. Daha yüksek veya daha düşük bir katmana ihtiyacınız olursa, hizmeti yeniden oluşturmanız gerekir.

## <a name="create-your-service"></a>Sitenizi oluşturma

Gerekli girişleri sağladıktan sonra, devam edin ve hizmeti oluşturun. 

![Hizmeti gözden geçirin ve oluşturun](./media/search-create-service-portal/new-service3.png "Hizmeti gözden geçirin ve oluşturun")

Hizmetiniz, Azure bildirimleri aracılığıyla izleyebilmeniz için dakikalar içinde dağıtılır. Gelecekte kolay erişim sağlamak için hizmeti panonuza sabitlemeyi göz önünde bulundurun.

![Hizmeti izleme ve sabitleme](./media/search-create-service-portal/monitor-notifications.png "Hizmeti izleme ve sabitleme")

## <a name="get-a-key-and-url-endpoint"></a>Anahtar ve URL uç noktası al

Portalı kullanmıyorsanız, yeni hizmetinize programlı erişim, URL uç noktasını ve bir kimlik doğrulama API anahtarı sağlamanızı gerektirir.

1. Hizmete genel bakış sayfasında, sayfanın sağ tarafındaki URL uç noktasını bulup kopyalayın.

2. Sol gezinti bölmesinde **anahtarlar** ' ı seçin ve ardından Yönetici anahtarlarından birini kopyalayın (eşdeğerdir). Yönetim API 'si-hizmetinize nesneleri oluşturmak, güncelleştirmek ve silmek için anahtarlar gereklidir.

   ![URL uç noktası Ile hizmete genel bakış sayfası](./media/search-create-service-portal/get-url-key.png "URL uç noktası ve diğer hizmet ayrıntıları")

Portal tabanlı görevler için bir uç nokta ve anahtar gerekli değildir. Portal, yönetici haklarıyla Azure Search kaynağına zaten bağlı. Portal Kılavuzu için [hızlı başlangıç: Portalda](search-get-started-portal.md)bir Azure Search dizini oluşturun.

## <a name="scale-your-service"></a>Hizmetinizi ölçeklendirme

Hizmetiniz sağlandıktan sonra ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilebilir. Azure Search hizmetiniz için standart katmanı seçerseniz, hizmetinizi iki boyutta ölçeklendirebilirsiniz: çoğaltmalar ve bölümler. Temel katmanı seçtiyseniz yalnızca çoğaltmalar ekleyebilirsiniz. Ücretsiz hizmeti sağladıysanız ölçek kullanılamaz.

***Bölümler***, hizmetinizin daha fazla belge depolamasına ve daha fazla belgede arama yapmasına olanak sağlar.

***Çoğaltmalar***, hizmetinizin daha yüksek arama sorgusu yükünü işlemesine olanak sağlar.

Kaynak eklemek aylık faturanız artırır. [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/), fatura konusunda kaynak eklemenin getirdiği sonuçları anlamanıza yardımcı olabilir. Kaynakları yüke göre ayarlayabildiğinizi unutmayın. Örneğin, tam bir ilk dizin oluşturmak için kaynakları artırabilir ve ardından artımlı dizin oluşturmak için daha uygun bir düzeye indirebilirsiniz.

> [!Important]
> Bir hizmetin [salt okunur SLA için 2 çoğaltması ve okuma/yazma SLA’sı için 3 çoğaltması](https://azure.microsoft.com/support/legal/sla/search/v1_0/) olmalıdır.

1. Azure portalında arama hizmeti sayfanıza gidin.
2. Sol gezinti bölmesinde **Ayarlar** > **Ölçek** seçeneklerini belirleyin.
3. Her iki türdeki kaynakları eklemek için kaydırma çubuğunu kullanın.

![Kapasite Ekle](./media/search-create-service-portal/settings-scale.png "Çoğaltmalar ve bölümler aracılığıyla kapasite ekleme")

> [!Note]
> Bölüm başına depolama ve hız daha yüksek katmanlarda artar. Daha fazla bilgi için bkz. [Kapasite ve sınırlar](search-limits-quotas-capacity.md).

## <a name="when-to-add-a-second-service"></a>Ne zaman ikinci bir hizmet eklenir?

Çoğu müşteri [, doğru kaynak dengesini](search-sku-tier.md)sağlayan bir katmanda sağlanan tek bir hizmeti kullanır. Bir hizmet, her bir dizinin diğerinden yalıtıldığı, [seçtiğiniz maksimum katman sınırlarına](search-capacity-planning.md) tabi olan birden fazla dizin barındırabilir. Azure Search’te istekler yalnızca bir dizine yönlendirilerek aynı hizmetteki diğer dizinlerden yanlışlıkla veya kasıtlı olarak veri alınması ihtimalini en aza indirir.

Müşterilerin çoğu yalnızca bir hizmet kullansa da, işletim gereksinimleri arasında aşağıdakiler yer alıyorsa hizmet yedekliliği gerekebilir:

* Olağanüstü durum kurtarma (veri merkezi kesintisi). Azure Search, bir kesinti olması durumunda anında yük devretme işlevi sağlamaz. Öneriler ve kılavuz için bkz. [Hizmet yönetim](search-manage.md).
* Çok kiracılı modelleme araştırması, ek hizmetlerin optimum tasarım olduğunu belirlemiştir. Daha fazla bilgi için bkz. [Çoklu kiracı tasarımı](search-modeling-multitenant-saas-applications.md).
* Genel olarak dağıtılan uygulamalar için, uygulamanızın uluslararası trafiğinin gecikme süresini en aza indirmek amacıyla birden fazla bölgede bir Azure Search örneği gerekebilir.

> [!NOTE]
> Azure Search, dizin oluşturma ve sorgulama işlemlerini ayırt edemez; Bu nedenle, ayrılmış iş yükleri için hiçbir şekilde birden çok hizmet oluşturmazsınız. Bir dizin her zaman oluşturulduğu hizmette sorgulanır (bir hizmette bir dizini oluşturup başka bir hizmete kopyalayamazsınız).

Yüksek düzeyde kullanılabilirlik için ikinci bir hizmet gerekmez. Aynı hizmette 2 veya daha fazla çoğaltma kullandığınızda sorguların yüksek kullanılabilirliği elde edilir. Çoğaltma güncelleştirmeleri sıralıdır; başka bir deyişle bir hizmet güncelleştirmesi kullanıma sunulduğunda en az biri işlevsel olur. Çalışma süresi hakkında daha fazla bilgi için bkz. [Hizmet Düzeyi Sözleşmeleri](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Sonraki adımlar

Azure Search bir hizmet sağlamadıktan sonra, ilk dizininizi oluşturmak için portalda devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Portalda Azure Search dizin oluşturma](search-get-started-portal.md)
