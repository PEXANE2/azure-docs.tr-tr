---
title: 'Quickstart: Portalda bir arama hizmeti oluşturma'
titleSuffix: Azure Cognitive Search
description: Bu portalda hızlı bir başlangıç yapın, Azure portalında bir Azure Bilişsel Arama kaynağının nasıl ayarlayabildiğini öğrenin. Kaynak gruplarını, bölgeleri ve SKU'yı veya fiyatlandırma katmanını seçin.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 02/10/2020
ms.openlocfilehash: 3bc3edcd0e75d8f6e3e4d6f9b200032909318040
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77209367"
---
# <a name="quickstart-create-an-azure-cognitive-search-service-in-the-portal"></a>Quickstart: Portalda bir Azure Bilişsel Arama hizmeti oluşturun

Azure Bilişsel Arama, bir arama deneyimini özel uygulamalara takmak için kullanılan bağımsız bir kaynaktır. Azure Bilişsel Arama, diğer Azure hizmetleriyle, ağ sunucularında uygulamalarla veya diğer bulut platformlarında çalışan yazılımlarla kolayca entegre edilmiştir.

Bu makalede, [Azure portalında](https://portal.azure.com/)nasıl bir kaynak oluşturulacak öğrenin.

[![Animasyonlu GIF](./media/search-create-service-portal/AnimatedGif-AzureSearch-small.gif)](./media/search-create-service-portal/AnimatedGif-AzureSearch.gif#lightbox)

PowerShell’i mi tercih ediyorsunuz? Azure Resource Manager [hizmet şablonunu](https://azure.microsoft.com/resources/templates/101-azure-search-create/) kullanın. Başlangıç konusunda yardım için [PowerShell ile Azure Bilişsel Aramayı Yönet'e](search-manage-powershell.md)bakın.

## <a name="subscribe-free-or-paid"></a>Abone olma (ücretsiz veya ücretli)

[Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ve ücretli Azure hizmetlerini denemek için ücretsiz krediler kullanın. Krediler bittikten sonra hesabı tutun ve Web Siteleri gibi ücretsiz Azure hizmetlerini kullanmaya devam edin. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak, [MSDN abone avantajlarınızı etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). MSDN aboneliği size her ay ücretli Azure hizmetleri için kullanabileceğiniz krediler verir. 

## <a name="find-azure-cognitive-search"></a>Azure Bilişsel Arama bul

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Sol üst köşedeki artı işaretini ("+ Kaynak Oluştur") tıklatın.
3. "Azure Bilişsel Arama"yı bulmak veya **Web** > **Azure Bilişsel Arama**aracılığıyla kaynağa gitmek için arama çubuğunu kullanın.

![Portalda kaynak oluşturma](./media/search-create-service-portal/find-search3.png "Portalda kaynak oluşturma")

## <a name="choose-a-subscription"></a>Abonelik seçin

Birden fazla aboneliğiniz varsa, arama hizmetiniz için bir tane seçin.

## <a name="set-a-resource-group"></a>Kaynak grubu ayarlama

Kaynak grubu, Azure çözümünüz için ilgili kaynakları barındıran bir kapsayıcıdır. Arama hizmeti için gereklidir. Maliyetler de dahil olmak üzere tüm kaynakları yönetmek için de yararlıdır. Kaynak grubu, bir hizmetten veya birlikte kullanılan birden çok hizmetten oluşabilir. Örneğin, bir Azure Cosmos DB veritabanını dizine dahil etmek için Azure Bilişsel Arama'yı kullanıyorsanız, her iki hizmeti de yönetim amacıyla aynı kaynak grubunun bir parçası yapabilirsiniz. 

Kaynakları tek bir grupta birleştirmiyorsanız veya varolan kaynak grupları ilgisiz çözümlerde kullanılan kaynaklarla doluysa, sadece Azure Bilişsel Arama kaynağınız için yeni bir kaynak grubu oluşturun. 

![Yeni bir kaynak grubu oluşturma](./media/search-create-service-portal/new-resource-group.png "Yeni bir kaynak grubu oluşturma")

Zaman içinde, geçerli ve öngörülen maliyetleri tüm-up izleyebilir veya tek tek kaynaklar için ücretleri görüntüleyebilirsiniz. Aşağıdaki ekran görüntüsü, birden çok kaynağı tek bir grupta birleştirdiğinizde görmeyi bekleyebileceğiniz maliyet bilgilerini gösterir.

![Maliyetleri kaynak grubu düzeyinde yönetme](./media/search-create-service-portal/resource-group-cost-management.png "Maliyetleri kaynak grubu düzeyinde yönetme")

> [!TIP]
> Bir grubu silmek içindeki tüm hizmetleri sildiği için kaynak grupları temizlemeyi basitleştirir. Birden fazla hizmet kullanan prototip projeler için, tüm bunların aynı kaynak grubuna yerleştirilmesi, proje bittikten sonra temizleme işlemini kolaylaştırır.

## <a name="name-the-service"></a>Hizmeti adlandırın

Örnek Ayrıntılar'da, **URL** alanında bir hizmet adı sağlayın. Ad, API çağrılarının verildiği URL bitiş noktasının `https://your-service-name.search.windows.net`bir parçasıdır: . Örneğin, bitiş noktasının olmasını `https://myservice.search.windows.net`istiyorsanız, . `myservice`

Hizmet adı gereksinimleri:

* search.windows.net ad alanı içinde benzersiz olmalıdır
* Uzunluğu 2 ile 60 karakter arasında olmalıdır.
* Küçük harfleri, rakamları veya tireleri ("-") kullanmalısınız
* Tire ("-") ilk 2 karakterde veya son tek karakter olarak kullanmayın
* Ardışık tire ("--") hiçbir yerde kullanamazsınız

> [!TIP]
> Birden çok hizmet kullanacağınızı düşünüyorsanız, hizmet adına bir adlandırma kuralı olarak bölge (veya konum) dahil öneririz. Aynı bölgedeki hizmetler ücretsiz olarak veri alışverişinde bulunabilir, bu nedenle Azure Bilişsel Arama Batı ABD'deyse ve `mysearchservice-westus` Batı ABD'de başka hizmetleriniz varsa, bu tür bir ad kaynakları nasıl birleştirebileceğinize veya ekleyebileceğinize karar verirken sizi özellikler sayfasına seyahatten kurtarabilir.

## <a name="choose-a-location"></a>Bir konum seçin

Azure Hizmeti olarak Azure Bilişsel Arama, dünyanın dört bir yanındaki veri merkezlerinde barındırılabilir. Desteklenen bölgelerin listesi [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulunabilir. 

Birden çok hizmet için aynı konumu seçerek bant genişliği ücretlerini en aza indirebilir veya önleyebilirsiniz. Örneğin, başka bir Azure hizmeti (Azure depolama, Azure Cosmos DB, Azure SQL Veritabanı) tarafından sağlanan verileri dizine ekiyorsanız, aynı bölgede Azure Bilişsel Arama hizmetioluşturmak bant genişliği ücretlerinden kaçınır (giden veriler için ücret ödenmesin hizmetler aynı bölgededir).

AI zenginleştirme kullanıyorsanız, Arama hizmetinizi Bilişsel Hizmetlerle aynı bölgede oluşturun. *Azure Bilişsel Arama ve Bilişsel Hizmetlerin aynı bölgede birlikte konumlanması, AI zenginleştirme için bir gerekliliktir.*

> [!Note]
> Orta Hindistan şu anda yeni hizmetler için kullanılamaz. Orta Hindistan'daki hizmetler için hiçbir kısıtlama olmaksızın ölçeklendirebilirsiniz ve hizmetiniz bu bölgede tam olarak desteklenir. Bu bölgeye kısıtlama geçicidir ve yalnızca yeni hizmetlerle sınırlıdır. Kısıtlama artık geçerli olmadığında bu notu kaldıracağız.

## <a name="choose-a-pricing-tier-sku"></a>Bir fiyatlandırma katmanı (SKU) seçin

[Azure Bilişsel Arama şu anda birden çok fiyatlandırma katmanında sunulmaktadır:](https://azure.microsoft.com/pricing/details/search/)Ücretsiz, Temel veya Standart. Her katmanın kendi [kapasitesi ve sınırları](search-limits-quotas-capacity.md) vardır. Yönergeler için [Fiyatlandırma katmanı veya SKU seçme](search-sku-tier.md) bölümüne bakın.

Temel ve Standart, üretim iş yükleri için en yaygın seçeneklerdir, ancak çoğu müşteri Ücretsiz hizmetle başlar. Katmanlar arasındaki temel farklar, bölüm boyutu ve hızı dır ve oluşturabileceğiniz nesne sayısının sınırlarıdır.

Hizmet oluşturulduktan sonra fiyatlandırma katmanıdeğiştirilemeyeceğini unutmayın. Daha yüksek veya daha düşük bir katmana ihtiyacınız varsa, hizmeti yeniden oluşturmanız gerekir.

## <a name="create-your-service"></a>Sitenizi oluşturma

Gerekli girişleri sağladıktan sonra, devam edin ve hizmeti oluşturun. 

![Hizmeti gözden geçirin ve oluşturun](./media/search-create-service-portal/new-service3.png "Hizmeti gözden geçirin ve oluşturun")

Hizmetiniz birkaç dakika içinde dağıtılır. Azure bildirimleri aracılığıyla ilerlemeyi izleyebilirsiniz. Gelecekte kolay erişim için hizmeti panonuza sabitlemeyi düşünün.

![Hizmeti izleme ve sabitleme](./media/search-create-service-portal/monitor-notifications.png "Hizmeti izleme ve sabitleme")

## <a name="get-a-key-and-url-endpoint"></a>Bir anahtar ve URL bitiş noktası alın

Portalı kullanmıyorsanız, yeni hizmetinize programlı erişim, URL bitiş noktasını ve kimlik doğrulama api anahtarını sağlamanızı gerektirir.

1. Genel **Bakış** sayfasında, sayfanın sağ tarafındaki URL bitiş noktasını bulun ve kopyalayın.

2. **Keys** sayfasında, yönetici anahtarlarından birini kopyalayın (eşdeğerdir). Yönetici api anahtarları, hizmetinizde nesneler oluşturmak, güncelleştirmek ve siler için gereklidir. Buna karşılık, sorgu anahtarları dizin içeriğine okuma erişimi sağlar.

   ![URL bitiş noktası ile hizmete genel bakış sayfası](./media/search-create-service-portal/get-url-key.png "URL bitiş noktası ve diğer hizmet ayrıntıları")

Portal tabanlı görevler için bir bitiş noktası ve anahtar gerekmez. Portal zaten yönetici haklarıyla Azure Bilişsel Arama kaynağınıza bağlıdır. Portal gözden geçirme için [Quickstart: Portalda bir Azure Bilişsel Arama dizini oluşturun.](search-get-started-portal.md)

## <a name="scale-your-service"></a>Hizmetinizi ölçeklendirme

Hizmetiniz sağlandıktan sonra ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilebilir. Azure Bilişsel Arama hizmetiniz için Standart katmanı seçtiyseniz, hizmetinizi iki boyutta ölçeklendirebilirsiniz: yinelemeler ve bölümler. Temel katmanı seçtiyseniz yalnızca çoğaltmalar ekleyebilirsiniz. Ücretsiz hizmeti sağladıysanız ölçek kullanılamaz.

***Bölümler***, hizmetinizin daha fazla belge depolamasına ve daha fazla belgede arama yapmasına olanak sağlar.

***Çoğaltmalar***, hizmetinizin daha yüksek arama sorgusu yükünü işlemesine olanak sağlar.

Kaynak eklemek aylık faturanız artırır. [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/), fatura konusunda kaynak eklemenin getirdiği sonuçları anlamanıza yardımcı olabilir. Kaynakları yüke göre ayarlayabildiğinizi unutmayın. Örneğin, tam bir ilk dizin oluşturmak için kaynakları artırabilir ve ardından artımlı dizin oluşturmak için daha uygun bir düzeye indirebilirsiniz.

> [!Important]
> Bir hizmetin [salt okunur SLA için 2 çoğaltması ve okuma/yazma SLA’sı için 3 çoğaltması](https://azure.microsoft.com/support/legal/sla/search/v1_0/) olmalıdır.

1. Azure portalında arama hizmeti sayfanıza gidin.
2. Sol gezinme bölmesinde **Ayarlar** > **Ölçeği'ni**seçin.
3. Her iki türdeki kaynakları eklemek için kaydırma çubuğunu kullanın.

![Kapasite ekleme](./media/search-create-service-portal/settings-scale.png "Yinelemeler ve bölümler aracılığıyla kapasite ekleme")

> [!Note]
> Bölüm başına depolama ve hız daha yüksek katmanlarda artar. Daha fazla bilgi için [kapasite ve sınırlara](search-limits-quotas-capacity.md)bakın.

## <a name="when-to-add-a-second-service"></a>Ne zaman ikinci bir hizmet eklenir?

Çoğu müşteri, [doğru kaynak dengesini](search-sku-tier.md)sağlayan bir katmanda sağlanan tek bir hizmeti kullanır. Bir hizmet, her bir dizinin diğerinden yalıtıldığı, [seçtiğiniz maksimum katman sınırlarına](search-capacity-planning.md) tabi olan birden fazla dizin barındırabilir. Azure Bilişsel Arama'da, istekler yalnızca bir dizin için yönlendirilebilir ve bu da aynı hizmetteki diğer dizinlerden yanlışlıkla veya kasıtlı veri alma olasılığını en aza indirir.

Müşterilerin çoğu yalnızca bir hizmet kullansa da, işletim gereksinimleri arasında aşağıdakiler yer alıyorsa hizmet yedekliliği gerekebilir:

* Olağanüstü durum kurtarma (veri merkezi kesintisi). Azure Bilişsel Arama, bir kesinti durumunda anında başarısızlık sağlamaz. Öneriler ve kılavuz için bkz. [Hizmet yönetim](search-manage.md).
* Çok kiracılı modelleme araştırması, ek hizmetlerin optimum tasarım olduğunu belirlemiştir. Daha fazla bilgi için bkz. [Çoklu kiracı tasarımı](search-modeling-multitenant-saas-applications.md).
* Genel olarak dağıtılan uygulamalar için, uygulamanızın uluslararası trafiğinin gecikmesini en aza indirmek için birden çok bölgede Azure Bilişsel Arama örneği gerekebilir.

> [!NOTE]
> Azure Bilişsel Arama'da dizin oluşturma ve sorgulama işlemlerini ayıramazsınız; böylece, ayrılmış iş yükleri için asla birden çok hizmet oluşturmazsınız. Bir dizin her zaman oluşturulduğu hizmette sorgulanır (bir hizmette bir dizini oluşturup başka bir hizmete kopyalayamazsınız).

Yüksek düzeyde kullanılabilirlik için ikinci bir hizmet gerekmez. Aynı hizmette 2 veya daha fazla çoğaltma kullandığınızda sorguların yüksek kullanılabilirliği elde edilir. Yineleme güncelleştirmeleri sıralıdır, bu da bir hizmet güncelleştirmesi kullanıma sunulduğunda en az birinin çalışır durumda olduğu anlamına gelir. Çalışma süresi hakkında daha fazla bilgi için [Hizmet Düzeyi Sözleşmeleri'ne](https://azure.microsoft.com/support/legal/sla/search/v1_0/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bir hizmeti sağlamadan sonra, ilk dizini oluşturmak için portalda devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Portalda Azure Bilişsel Arama dizini oluşturma](search-get-started-portal.md)
