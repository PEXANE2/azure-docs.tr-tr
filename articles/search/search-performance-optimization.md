---
title: Performansı ölçeklendirme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama performansını ayarlaya bilmek ve optimum ölçeği yapılandırmak için teknikleri ve en iyi uygulamaları öğrenin.
manager: nitinme
author: LiamCavanagh
ms.author: liamca
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 7c2857de0613be400f83544e1dabe079b7497bbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77212380"
---
# <a name="scale-for-performance-on-azure-cognitive-search"></a>Azure Bilişsel Arama'da performans için ölçeklendirme

Bu makalede, ölçeklenebilirlik ve kullanılabilirlik için gelişmiş gereksinimleri olan gelişmiş senaryolar için en iyi uygulamalar açıklanmaktadır.

## <a name="start-with-baseline-numbers"></a>Temel sayılarla başlayın

Daha büyük bir dağıtım çabası gerçekleştirmeden önce, tipik bir sorgu yükünün neye benzediğini bildiğinizden emin olun. Aşağıdaki yönergeler, temel sorgu numaralarına ulaşmanıza yardımcı olabilir.

1. Tipik bir arama isteğinin tamamlanması için gereken bir hedef gecikme süresi (veya maksimum süre) seçin.

1. Bu gecikme oranlarını ölçmek için gerçekçi bir veri kümesiyle arama hizmetinize karşı gerçek bir iş yükü oluşturun ve test edin.

1. Saniyede az sayıda sorguyla (QPS) başlayın ve sorgu gecikmesi önceden tanımlanmış hedefin altına düşene kadar testte çalıştırılan sayıyı kademeli olarak artırın. Bu, uygulamanız kullanımda büyüdükçe ölçek lendirmenize yardımcı olacak önemli bir ölçüttür.

1. Mümkün olan her yerde HTTP bağlantılarını yeniden kullanın. Azure Bilişsel Arama .NET SDK kullanıyorsanız, bu bir örneği veya [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) örneğini yeniden kullanmanız gerektiği ve REST API'sini kullanıyorsanız, tek bir HttpClient'ı yeniden kullanmanız gerektiği anlamına gelir.

1. Aramanın dizininizin farklı bölümleri üzerinde gerçekleşmesi için sorgu isteklerinin öznünüzü değiştirin. Varyasyon önemlidir, çünkü aynı arama isteklerini sürekli olarak yürütürseniz, veri önbelleğe alma, performansı daha farklı bir sorgu kümesiyle olduğundan daha iyi görünmeye başlar.

1. Farklı türde sorgular elde etmek için sorgu isteklerinin yapısını değiştirin. Her arama sorgusu aynı düzeyde gerçekleştirmez. Örneğin, bir belge arama veya arama önerisi genellikle önemli sayıda farklı yönü ve filtresi olan bir sorgudan daha hızlıdır. Test kompozisyonu, üretimde beklediğiniz gibi kabaca aynı oranlarda çeşitli sorgular içermelidir.  

Bu test iş yüklerini oluştururken, Azure Bilişsel Arama'nın göz önünde bulundurulması gereken bazı özellikleri vardır:

+ Aynı anda çok fazla arama sorgusu iterek hizmetinizi aşırı yüklemeniz mümkündür. Bu durumda, HTTP 503 yanıt kodlarını görürsünüz. Test sırasında bir 503'ten kaçınmak için, daha fazla arama isteği ekledikçe gecikme oranlarındaki farklılıkları görmek için çeşitli arama istekleri aralıklarıyla başlayın.

+ Azure Bilişsel Arama, arka planda dizin oluşturma görevlerini çalıştırmaz. Hizmetiniz sorgu yu ve iş yüklerini dizine aynı anda işliyorsa, sorgu testlerinize dizin oluşturma işleri ekleyerek veya yoğun saatlerde dizin oluşturma seçeneklerini keşfederek bunu göz önünde bulundurun.

> [!Tip]
> Yük test araçlarını kullanarak gerçekçi bir sorgu yükünü simüle edebilirsiniz. [Azure DevOps ile yük testi'ni](https://docs.microsoft.com/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=azure-devops) deneyin veya bu [alternatiflerden](https://docs.microsoft.com/azure/devops/test/load-test/overview?view=azure-devops#alternatives)birini kullanın.

## <a name="scale-for-high-query-volume"></a>Yüksek sorgu hacmi için ölçeklendirme

Sorgular çok uzun sürdüğünde veya hizmet isteklerini bırakmaya başladığında hizmet aşırı yüklenir. Bu durumda, sorunu iki şekilde giderebilirsiniz:

+ **Yineleme ekleme**  

  Her yineleme, hizmetin bakiye isteklerini birden çok kopyaya karşı yüklemesine olanak tanıyan verilerinizin bir kopyasıdır.  Tüm yük dengeleme ve verilerin çoğaltılması Azure Bilişsel Arama tarafından yönetilir ve istediğiniz zaman hizmetiniz için ayrılan çoğaltma sayısını değiştirebilirsiniz. Standart arama hizmetinde en fazla 12 yineleme ve Temel arama hizmetinde 3 yineleme ayırabilirsiniz. Yinelemeler [Azure portalından](search-create-service-portal.md) veya [PowerShell'den](search-manage-powershell.md)ayarlanabilir.

+ **Daha yüksek bir katmanda yeni bir hizmet oluşturma**  

  Azure Bilişsel Arama çeşitli [katmanlarda](https://azure.microsoft.com/pricing/details/search/) gelir ve her biri farklı performans düzeyleri sunar. Bazı durumlarda, üzerinde olduğunuz katman, yinelemelerin limiti dolsa bile yeterli geri dönüş sağlayamadığında o kadar çok sorgunuz olabilir. Bu durumda, çok sayıda belgeye ve son derece yüksek sorgu iş yüklerine sahip senaryolar için tasarlanmış Standart S3 katmanı gibi daha yüksek performanslı bir katmana taşınmayı düşünün.

## <a name="scale-for-slow-individual-queries"></a>Yavaş ayrı sorgular için ölçeklendirme

Yüksek gecikme oranlarının bir diğer nedeni de, tamamlanması çok uzun süren tek bir sorgudur. Bu durumda, yinelemeler ekleme yardımcı olmaz. Yardımcı olabilecek iki olası seçenek şunlardır:

+ **Bölümleri Artırma**

  Bir bölüm verileri ekstra bilgi işlem kaynaklarına böler. İki bölüm verileri ikiye böler, üçüncü bir bölüm üçüncülere böler ve saire. Olumlu yan etkilerden biri, paralel bilgi işlem nedeniyle yavaş sorguların bazen daha hızlı performans göstermesidir. Birçok belgeyle eşleşen sorgular veya çok sayıda belge üzerinde sayım sağlayan yüzler gibi düşük seçicilik sorgularında paralelleştirme kaydettik. Belgelerin alaka puanını almak veya belge sayısını saymak için önemli hesaplama gerektiğinden, ek bölümler eklemek sorguların daha hızlı tamamlanmasına yardımcı olur.  
   
  Standart arama hizmetinde en fazla 12 bölüm ve Temel arama hizmetinde 1 bölüm olabilir. Bölümler [Azure portalından](search-create-service-portal.md) veya [PowerShell'den](search-manage-powershell.md)ayarlanabilir.

+ **Yüksek Kardinallik Alanlarını Sınırla**

  Yüksek önem düzeyi alanı, önemli sayıda benzersiz değere sahip bir yüz tablolu veya filtrelenebilir alandan oluşur ve sonuç olarak, sonuçları hesaplarken önemli kaynaklar tüketir. Örneğin, bir Ürün Kimliği veya Açıklama alanını facetable/filterable olarak ayarlamak, belgeden belgeye değerlerin çoğu benzersiz olduğundan, yüksek ciddiyet olarak sayılır. Mümkün olan her yerde, yüksek kardinallik alanlarının sayısını sınırlayın.

+ **Arama Katmanını Artırın**  

  Daha yüksek bir Azure Bilişsel Arama katmanına geçmek, yavaş sorguların performansını artırmanın başka bir yolu olabilir. Her bir üst katman, sorgu performansı üzerinde olumlu bir etkiye sahip olan daha hızlı CPU'lar ve daha fazla bellek sağlar.

## <a name="scale-for-availability"></a>Kullanılabilirlik için ölçeklendir

Yinelemeler yalnızca sorgu gecikmesüresini azaltmaya yardımcı olmakla birlikte, yüksek kullanılabilirlik için de izin verebilir. Tek bir yinelemeyle, yazılım güncelleştirmeleri veya oluşacak diğer bakım olayları sonrasında sunucu yeniden başlatmaları nedeniyle periyodik olarak kapalı kalma süresi beklemelisiniz. Sonuç olarak, uygulamanızın aramaların (sorguların) yanı sıra yazma (dizin oluşturma olayları) yüksek kullanılabilirlik gerektirip gerektirip gerektirmeyişe bilip olmadığını göz önünde bulundurmanız önemlidir. Azure Bilişsel Arama, aşağıdaki özelliklere sahip tüm ücretli arama teklifleri üzerinde SLA seçenekleri sunar:

+ Salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirliği için iki yineleme

+ Okuma yazma iş yüklerinin yüksek kullanılabilirliği için üç veya daha fazla yineleme (sorgular ve dizin oluşturma)

Bu konuda daha fazla bilgi için lütfen [Azure Bilişsel Arama Hizmeti Düzeyi Sözleşmesi'ni](https://azure.microsoft.com/support/legal/sla/search/v1_0/)ziyaret edin.

Yinelemeler verilerinizin kopyaları olduğundan, birden çok yinelemeye sahip olmak Azure Bilişsel Arama'nın bir yinelemeye karşı makine yeniden başlatma ve bakım yapmasına olanak sağlarken, sorgu yürütme diğer yinelemelerde de devam eder. Tersine, yinelemeleri uzaklara götürürseniz, bu yinelemelerin az kullanılan bir kaynak olduğunu varsayarak sorgu performansı bozulmasına uğrarsınız.

## <a name="scale-for-geo-distributed-workloads-and-geo-redundancy"></a>Coğrafi olarak dağıtılan iş yükleri ve coğrafi artıklık için ölçeklendirme

Coğrafi olarak dağıtılan iş yükleri için, ana bilgisayar veri merkezinden uzakta bulunan kullanıcılar daha yüksek gecikme gecikme oranlarına sahip olur. Bir azaltma, bu kullanıcılara daha yakın olan bölgelerde birden çok arama hizmeti sağlamaktır.

Azure Bilişsel Arama şu anda bölgeler arasında Azure Bilişsel Arama dizinlerini coğrafi olarak çoğaltmak için otomatik bir yöntem sağlamaz, ancak bu işlemin uygulanmasını ve yönetilmesini kolaylaştırabilecek bazı teknikler vardır. Bunlar sonraki birkaç bölümde özetlenmiştir.

Coğrafi olarak dağıtılan bir arama hizmeti kümesinin amacı, bir kullanıcının bu örnekte görüldüğü gibi en düşük gecikme gecikmesini sağlayan Azure Bilişsel Arama hizmetine yönlendirildiği iki veya daha fazla bölgede iki veya daha fazla dizin kullanılabilir olmasıdır:

   ![Bölgelere göre hizmetlerin çapraz sekmesi][1]

### <a name="keep-data-synchronized-across-multiple-services"></a>Verileri birden çok hizmet arasında senkronize tutma

Azure [Bilişsel Arama Dizinleyicisi](search-indexer-overview.md) veya Push API 'yi [(Azure Bilişsel Arama REST API](https://docs.microsoft.com/rest/api/searchservice/)olarak da adlandırılır) kullanmaktan oluşan dağıtılmış arama hizmetlerinizi eşit tutmak için iki seçenek vardır.  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Birden çok hizmetteki içeriği güncelleştirmek için dizin oluşturma

Bir hizmette zaten dizinleyici kullanıyorsanız, aynı veri kaynağı nesnesini kullanmak için ikinci bir hizmetüzerinde ikinci bir dizinleyiciyi yapılandırabilirsiniz ve aynı konumdan veri çekebilirsiniz. Her bölgedeki her hizmetin kendi dizinleyicisi ve hedef dizini vardır (arama dizininiz paylaşılmaz, bu da verilerin çoğaltıldığı anlamına gelir), ancak her dizinleyici aynı veri kaynağına başvurur.

İşte bu mimarinin neye benzeyeceğine dair üst düzey bir görsel.

   ![Dağıtılmış dizinleyici ve hizmet birleşimleri ile tek veri kaynağı][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>Birden çok hizmette içerik güncelleştirmelerini zorlamak için REST API'lerini kullanın

[Azure Bilişsel Arama Dizininizdeki içeriği zorlamak](https://docs.microsoft.com/rest/api/searchservice/update-index)için Azure Bilişsel Arama REST API'sını kullanıyorsanız, bir güncelleştirme gerektiğinde tüm arama hizmetlerinde değişiklik yaparak çeşitli arama hizmetlerinizi eşit tutabilirsiniz. Kodunuzda, bir arama hizmetine güncelleştirmenin başarısız olduğu ancak diğer arama hizmetlerinde başarılı olduğu servis taleplerini işlediğinizden emin olun.

## <a name="leverage-azure-traffic-manager"></a>Azure Trafik Yöneticisi'ni kaldır

[Azure Trafik Yöneticisi,](../traffic-manager/traffic-manager-overview.md) istekleri birden çok arama hizmeti tarafından desteklenen birden çok coğrafi konumdaki web sitelerine yönlendirmenize olanak tanır. Trafik Yöneticisi'nin bir avantajı, kullanılabilir olduğundan emin olmak için Azure Bilişsel Arama'yı inceleyebilir ve kapalı kalma süresi durumunda kullanıcıları alternatif arama hizmetlerine yönlendirebilmesidir. Ayrıca, Azure Web Siteleri üzerinden arama isteklerini yönlendirmeniz durumunda, Azure Trafik Yöneticisi, Web Sitesinin açık olduğu ancak Azure Bilişsel Arama'nın olmadığı bakiye durumlarını yüklemenize olanak tanır. Trafik Yöneticisi'ni kaldıran mimarinin ne olduğuna bir örnek aşağıda verilmiştir.

   ![Merkezi Trafik Yöneticisi ile bölgelere göre hizmetlerin çapraz sekmesi][3]

## <a name="next-steps"></a>Sonraki adımlar

Her biri için fiyatlandırma katmanları ve hizmet sınırları hakkında daha fazla bilgi edinmek için [Hizmet sınırlarına](search-limits-quotas-capacity.md)bakın. Bölüm ve çoğaltma kombinasyonları hakkında daha fazla bilgi edinmek [için kapasite için Plan'a](search-capacity-planning.md) bakın.

Performans ve bu makalede tartışılan tekniklerin gösterileri hakkında bir tartışma için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
