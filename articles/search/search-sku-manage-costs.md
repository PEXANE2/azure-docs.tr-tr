---
title: Kapasiteyi ve maliyetleri tahmin etme
titleSuffix: Azure Cognitive Search
description: Azure 'daki altyapı ve araçlar da dahil olmak üzere arama hizmeti maliyetlerinin yanı sıra, kaynakları daha verimli bir şekilde kullanmaya yönelik en iyi yöntemleri kapsayan kapasiteyi tahmin etme ve yönetme kılavuzunu inceleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.openlocfilehash: d48ae71a979a2d0f1457b0cefa8a98a02710dd96
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577751"
---
# <a name="how-to-estimate-capacity-and-costs-of-an-azure-cognitive-search-service"></a>Azure Bilişsel Arama hizmetinin kapasitesini ve maliyetlerini tahmin etme

Azure Bilişsel Arama için kapasite planlamasının bir parçası olarak aşağıdaki kılavuz maliyetleri düşürmenize veya maliyetleri daha etkin bir şekilde yönetmenize yardımcı olabilir:

+ Bant genişliği ücretlerini en aza indirmek veya ortadan kaldırmak için, aynı bölgedeki veya mümkün olduğunca az sayıda bölgede tüm kaynakları oluşturun.

+ Tüm hizmetleri Azure Bilişsel Arama, bilişsel hizmetler ve çözümünüzde kullanılan diğer Azure hizmetleri gibi tek bir kaynak grubunda birleştirin. Azure portal, kaynak grubunu bulun ve gerçek ve tahmini harcama hakkında bilgi edinmek için **maliyet yönetimi** komutlarını kullanın.

+ İstek ve yanıtların veri merkezi sınırının içinde kalması için, ön uç uygulamanız için Azure Web App 'i göz önünde bulundurun.

+ Dizin oluşturma gibi yoğun kaynak gerektiren işlemlere göre ölçeklendirin ve normal sorgu iş yükleri için hemen aşağı doğru. Azure Bilişsel Arama için en düşük yapılandırma (bir bölümden ve bir çoğaltmayla bir SU) ile başlayın ve ardından daha fazla kapasiteye ihtiyacı olan kullanım düzenlerini belirlemek için Kullanıcı etkinliğini izleyin. Tahmin edilebilir bir model varsa, ölçeklendirmeyi etkinlikle eşitlenebilir (Bunu otomatikleştirmek için kod yazmanız gerekir).

Faturalanabilir olaylar, faturalandırma formülü ve faturalandırılabilir ücret [fiyatlandırma katmanı seçme](search-sku-tier.md)bölümünde açıklanmaktadır. Ayrıca, harcama ile ilgili yerleşik araçlar ve özellikler için [faturalandırma ve maliyet yönetimini](../cost-management-billing/cost-management-billing-overview.md) de ziyaret edebilirsiniz.

Arama hizmetini geçici olarak kapatmak mümkün değildir. Adanmış kaynaklar her zaman çalışır ve hizmetinizin kullanım ömrü boyunca özel kullanım için ayrılır. Bir hizmetin silinmesi kalıcıdır ve ayrıca ilişkili verileri de silinir.

Hizmetin kendisi açısından, faturanızı düşürmenin tek yolu, çoğaltmaları ve bölümleri, kabul edilebilir performans ve [SLA uyumluluğu](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sağlayan bir düzeye düşürmeniz veya daha düşük bir katmanda bir hizmet oluşturmanız (S1 saatlik fiyatlar S2 veya S3 oranlarından daha düşüktür). Hizmetinizi yük projeksiyonlarınızın alt ucunda sağladığınızda, hizmeti büyüyerek ikinci bir daha büyük katmanlı hizmet oluşturabilir, dizinlerinizi ikinci hizmette yeniden oluşturabilir ve sonra birincisini silebilirsiniz.

## <a name="how-to-evaluate-capacity-requirements"></a>Kapasite gereksinimlerini değerlendirme

Azure Bilişsel Arama, kapasite *çoğaltmalar* ve *bölümler* olarak yapılandırılır.

+ Çoğaltmalar, arama hizmetinin örnekleridir. Her çoğaltma, bir dizinin yük dengeli bir kopyasını barındırır. Örneğin, altı çoğaltmaya sahip bir hizmetin, hizmete yüklenen her dizinin altı kopyası vardır.

+ Bölümler depolar ve aranabilir verileri otomatik olarak böler. İki bölüm, dizininizi yarı bir şekilde böler, üç bölüm onu üçe ayırır ve bu şekilde devam eder. Kapasite açısından *bölüm boyutu* , katmanlar arasında birincil farklılaştırıcı özelliktir.

> [!NOTE]
> Tüm standart ve depolama için Iyileştirilmiş katmanlar, [çoğaltma ve bölümlerin esnek birleşimlerini](search-capacity-planning.md#chart) destekler, böylece dengeyi değiştirerek [sisteminizi hız veya depolama için iyileştirebilirsiniz](search-performance-optimization.md) . Temel katman, yüksek kullanılabilirlik için en fazla üç çoğaltma sunar, ancak yalnızca bir bölüme sahiptir. Ücretsiz katmanlar adanmış kaynaklar sağlamıyor: bilgi işlem kaynakları birden çok abone tarafından paylaşılır.

### <a name="evaluating-capacity"></a>Kapasiteyi değerlendirme

Hizmetin kapasitesi ve maliyetleri, el ile çalışmaya devam ediyor. Katmanlar iki düzeyde sınırlar yapar: depolama ve içerik (örneğin, dizin sayısı). Her ikisi de düşünmeniz gerekir, çünkü ilk ulaşılan sınır etkin sınırdır.

İş gereksinimleri genellikle ihtiyaç duyacağınız dizin sayısını belirler. Örneğin, büyük bir belge deposu için genel bir dizine ihtiyacınız bulunabilir. Ya da bölge, uygulama veya iş kolu tabanlı birden çok dizine ihtiyacınız bulunabilir.

Bir dizinin boyutunu öğrenmek için [bir tane oluşturmanız](search-what-is-an-index.md)gerekir. Boyutu, içeri aktarılan verileri ve öneri araçları, filtrelemesini ve sıralamayı etkinleştirip etkinleştirmeyeceğinizi, dizin yapılandırmasını temel alır.

Tam metin arama için, birincil veri yapısı, kaynak verilerden farklı özelliklere sahip olan [ters bir dizin](https://en.wikipedia.org/wiki/Inverted_index) yapısıdır. Ters bir dizin için boyut ve karmaşıklık, içeriğe göre belirlenir, bu, içinde yer alan veri miktarına göre değildir. Yüksek artıklığa sahip büyük bir veri kaynağı, yüksek oranda değişken içerik içeren küçük bir veri kümesinden daha küçük bir dizin oluşmasına neden olabilir. Bu nedenle, özgün veri kümesinin boyutuna bağlı olarak dizin boyutunu çıkarsmak nadiren mümkündür.

> [!NOTE] 
> Dizinler ve depolama için gelecekteki ihtiyaçları tahmin etmek de tahmin etmek gibi görünse de bunun yapılması gerekir. Bir katmanın kapasitesi çok düşük olursa, daha yüksek bir katmanda yeni bir hizmet sağlamanız ve ardından [dizinlerinizi yeniden yüklemeniz](search-howto-reindex.md)gerekir. Bir hizmetin bir katmandan diğerine yerinde yükseltilmesi yoktur.
>

## <a name="estimate-with-the-free-tier"></a>Ücretsiz katman ile tahmin edin

Kapasiteyi tahmin etmek için bir yaklaşım ücretsiz katmanla başlamadır. Ücretsiz hizmetin üç Dizin, 50 MB depolama alanı ve 2 dakikalık dizin oluşturma süresi sunduğunu unutmayın. Tahmini bir dizin boyutunu bu kısıtlamalarla tahmin etmek zor olabilir, ancak bunlar şu adımlardan biri:

+ [Ücretsiz bir hizmet oluşturun](search-create-service-portal.md).
+ Küçük, temsili bir veri kümesi hazırlayın.
+ [Portalda bir başlangıç dizini oluşturun](search-get-started-portal.md) ve boyutunu aklınızda edin. Özellikler ve özniteliklerin depolama üzerinde bir etkisi vardır. Örneğin, öneri araçları (arama-yazma sorguları) ekleme, depolama gereksinimlerini artıracaktır. Aynı veri kümesini kullanarak, Depolama gereksinimlerinin nasıl değişeceğini görmek için, her bir alanda farklı özniteliklere sahip bir dizinin birden çok sürümünü oluşturmayı deneyebilirsiniz. Daha fazla bilgi için [temel dizin oluşturma içindeki "depolama etkileri"](search-what-is-an-index.md#index-size)başlığına bakın.

El ile kabaca bir tahmin sayesinde, bu miktarı iki dizin (geliştirme ve üretim) için bütçeye katmanızı ve ardından katmanınızı uygun şekilde seçmenizi sağlayabilirsiniz.

## <a name="estimate-with-a-billable-tier"></a>Faturalandırılabilir katman ile tahmin

Adanmış kaynaklar, geliştirme sırasında dizin miktarının, boyutunun ve sorgu birimlerinin daha gerçekçi tahminleri için daha büyük örnekleme ve işleme sürelerine uyum sağlayabilir. Bazı müşteriler, faturalandırılabilir bir katmanda doğrudan geçiş yaparken geliştirme projesi olarak yeniden değerlendirilir.

1. Alt katmanların ihtiyacınız olan dizin sayısını destekleyip desteklemediğini öğrenmek için [Her katmandaki hizmet sınırlarını gözden geçirin](./search-limits-quotas-capacity.md#index-limits) . Temel, S1 ve S2 katmanlarında Dizin sınırları sırasıyla 15, 50 ve 200 ' dir. Depolama için Iyileştirilmiş katmanın, az sayıda çok büyük dizini destekleyecek şekilde tasarlandığından 10 Dizin sınırı vardır.

1. [Faturalanabilir katmanda bir hizmet oluşturun](search-create-service-portal.md):

    + Tahmini yük hakkında emin değilseniz, temel veya S1 ' te düşük ' ı başlatın.
    + Büyük ölçekli dizin oluşturma ve sorgu yüklerine sahip olduğunuzu biliyorsanız, S2 veya hatta S3 ' da yüksek bir başlangıç yapın.
    + Depolama ile en Iyileştirilmiş, L1 veya L2 ile başlayın; büyük miktarda veri dizinleniyor ve sorgu yükü görece düşükse, dahili bir iş uygulamasında olduğu gibi.

1. Kaynak verilerin bir dizine nasıl çevrilip dönüştürmediğine yönelik [bir başlangıç dizini oluşturun](search-what-is-an-index.md) . Bu, dizin boyutunu tahmin etmenin tek yoludur.

1. Portalda [depolama, hizmet limitleri, sorgu hacmi ve gecikme süresini izleyin](search-monitor-usage.md) . Portal, saniye başına sorgular, kısıtlanmış sorgular ve arama gecikme süresi gösterir. Bu değerlerin tümü, doğru katmanı seçtiğinizden karar vermenize yardımcı olabilir. 

Dizin numarası ve boyutu, analizinizi eşit derecede önemlidir. Bunun nedeni, en fazla depolama (bölüm) kullanımı veya kaynakların (dizinler, Dizin oluşturucular, vb.) en üst limitlerinin (hangisi önce geldiğini) tam olarak ulaşılmasını içerir. Portal, Genel Bakış sayfasında geçerli kullanımı ve en fazla sınırları yan yana göstererek her ikisini de izlemenize yardımcı olur.

> [!NOTE]
> Belgeler, fazlalık veriler içeriyorsa depolama gereksinimleri daraltılabilir. İdeal olarak, belgeler yalnızca arama deneyimi için gereken verileri içerir. İkili veriler aranabilir değildir ve ayrı ayrı depolanmalıdır (Belki de bir Azure tablosu veya blob depolamada olabilir). Bir alan, dış verilere bir URL başvurusu tutmak için dizine eklenmelidir. Tek bir belgenin en büyük boyutu 16 MB 'tır (veya bir istekte birden çok belgeyi toplu olarak karşıya yüklüyorsanız, daha az). Daha fazla bilgi için bkz. [Azure bilişsel arama hizmet limitleri](search-limits-quotas-capacity.md).
>

**Sorgu hacmi konuları**

Saniyedeki sorgu sayısı (QPS), performans ayarlama sırasında önemli bir ölçümdür, ancak genellikle bir katman, mantıksal olarak yüksek sorgu hacmi beklemeniz durumunda yalnızca bir katman önem taşımaktadır.

Standart katmanlar, çoğaltmaları ve bölümleri dengeleyebilir. Yük Dengeleme için çoğaltmalar ekleyerek veya paralel işleme için bölüm eklemek yoluyla sorgu kapatma işlemini artırabilirsiniz. Daha sonra hizmet sağlandıktan sonra performansı ayarlayabilirsiniz.

Outset 'ten yüksek hacimli sorgu birimleri düşünüyorsanız, daha güçlü donanımlar tarafından desteklenen, daha yüksek standart katmanları göz önünde bulundurmanız gerekir. Daha sonra, bu sorgu birimleri gerçekleşmese de bölümleri ve çoğaltmaları çevrimdışı duruma geçirebilir veya alt katman bir hizmete geçebilirsiniz. Sorgu verimini hesaplama hakkında daha fazla bilgi için bkz. [Azure bilişsel arama performansı ve iyileştirmesi](search-performance-optimization.md).

Depolama için Iyileştirilmiş katmanlar, büyük veri iş yükleri için yararlıdır ve sorgu gecikme süresi gereksinimleri daha az önemli olduğunda için daha fazla genel kullanılabilir dizin depolaması destekler. Yük Dengeleme için ek çoğaltmalar ve paralel işleme için ek bölümler kullanmanız gerekir. Daha sonra hizmet sağlandıktan sonra performansı ayarlayabilirsiniz.

**Hizmet düzeyi sözleşmeleri**

Ücretsiz katman ve Önizleme özellikleri [hizmet düzeyi sözleşmeleri (SLA 'lar)](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sağlamaz. Tüm faturalanabilir katmanlar için, hizmet için yeterli artıklık sağladığınızda SLA 'Lar devreye girer. Sorgu (okuma) SLA 'Ları için iki veya daha fazla kopyaya sahip olmanız gerekir. Sorgu ve dizin oluşturma (okuma-yazma) SLA 'Ları için üç veya daha fazla kopyaya sahip olmanız gerekir. Bölüm sayısı SLA 'Ları etkilemez.

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirmesi için ipuçları

+ Ölçümlerin sorguları etrafında derleme yapmasına ve kullanım desenlerinde (iş saatleri sırasında sorgular, yoğun olmayan saatlerde dizin oluşturma) veri toplamasına izin verin. Hizmet sağlama kararlarını bilgilendirmek için bu verileri kullanın. Saatlik veya günlük temposunda pratik olmasa da, sorgu birimlerindeki planlı değişikliklere uyum sağlamak için bölümleri ve kaynakları dinamik olarak ayarlayabilirsiniz. Ayrıca, düzeylerin işlem yapmak için yeterince uzun sürmesi durumunda planlanmamış ancak sürekli değişikliklere de uyum sağlayabilirsiniz.

+ Sağlama altındaki tek alt tarafının, gerçek gereksinimler tahmine göre daha büyükse bir hizmeti bölmek zorunda kalmamanız gerektiğini unutmayın. Hizmet kesintisi yaşamamak için, daha yüksek bir katmanda yeni bir hizmet oluşturun ve tüm uygulamalar ve istekler yeni uç noktayı hedefleyerek yan yana çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ücretsiz bir katmanla başlayın ve özelliklerini anlamak için verilerinizin bir alt kümesini kullanarak bir başlangıç dizini oluşturun. Azure Bilişsel Arama veri yapısı ters bir dizin yapısıdır. Ters çevrilen bir dizinin boyutu ve karmaşıklığı içeriğe göre belirlenir. Yüksek oranda yedekli içeriğin çok sayıda düzensiz içerikten daha küçük bir dizine neden olduğunu unutmayın. Bu nedenle, veri kümesinin boyutu yerine içerik özellikleri dizin depolama gereksinimlerini belirlemektir.

Dizin boyutunuzu ilk tahmine sahip olduktan sonra, bu makalede ele alınan katmanlardan birinde [faturalandırılabilir bir hizmet sağlayın](search-create-service-portal.md) : temel, standart veya depolama için iyileştirilmiş. Veri boyutlandırmasına ilişkin yapay kısıtlamaları rahatlaın ve dizinizi, aranabilir olmasını istediğiniz tüm verileri içerecek şekilde [yeniden derleyin](search-howto-reindex.md) .

İhtiyaç duyduğunuz performans ve ölçeği almak için gereken [bölümleri ve çoğaltmaları ayırın](search-capacity-planning.md) .

Performans ve kapasite iyi durumdaysa, işiniz bitti demektir. Aksi takdirde, gereksinimlerinize göre daha yakından hizalanan bir arama hizmetini farklı bir katmanda yeniden oluşturun.

> [!NOTE]
> Sorularınız varsa, [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) ' e gönderin veya [Azure desteği ile iletişime geçin](https://azure.microsoft.com/support/options/).