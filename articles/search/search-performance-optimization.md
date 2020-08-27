---
title: Performansı ölçeklendirme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama performansını ayarlamaya ve optimum ölçeği yapılandırmaya yönelik teknikleri ve en iyi yöntemleri öğrenin.
manager: nitinme
author: LiamCavanagh
ms.author: liamca
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 5fd949466978714fe1dc0c4ccc67a3cb8f993314
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934965"
---
# <a name="scale-for-performance-on-azure-cognitive-search"></a>Azure Bilişsel Arama performans için ölçeklendirin

Bu makalede ölçeklenebilirlik ve kullanılabilirlik için gelişmiş gereksinimlere sahip gelişmiş senaryolar için en iyi yöntemler açıklanmaktadır.

## <a name="start-with-baseline-numbers"></a>Taban çizgisi numaralarıyla başla

Daha büyük bir dağıtım çabadan önce, tipik bir sorgu yükünün nasıl göründüğünü öğrendiğinizden emin olun. Aşağıdaki kılavuzlar, taban çizgisi sorgu numaralarına ulaşgetirmenize yardımcı olabilir.

1. Tipik bir arama isteğinin tamamlanması için gereken bir hedef gecikme süresi (veya en fazla süre) seçin.

1. Bu gecikme sürelerini ölçmek için gerçekçi bir veri kümesiyle, arama hizmetinize karşı gerçek bir iş yükü oluşturun ve test edin.

1. Saniye başına az sayıda sorgu (QPS) ile başlayın ve ardından sorgu gecikmesi önceden tanımlanmış hedefin altına düşene kadar testte yürütülen sayıyı kademeli olarak artırın. Bu, uygulamanızın kullanımda büyüdüğü sürece ölçek planlaması yapmanıza yardımcı olan önemli bir kıyaslamaya yöneliktir.

1. Mümkün olan yerlerde HTTP bağlantılarını yeniden kullanın. Azure Bilişsel Arama .NET SDK kullanıyorsanız bu, bir örneği veya [Searchındexclient](/dotnet/api/microsoft.azure.search.searchindexclient) örneğini yeniden kullanmanız gerektiği anlamına gelir ve REST API kullanıyorsanız, tek bir HttpClient kullanmanız gerekir.

1. Sorgunun farklı bölümlerinin üzerinde gerçekleşmesini sağlamak için sorgu isteklerinin bu şekilde çeşitini farklılık gösterir. Aynı arama isteklerini sürekli olarak yürütüyorsa, değişim önemlidir çünkü sürekli olarak aynı arama isteklerini yürütüyorsa, verilerin önbelleğe alınması, daha farklı bir sorgu kümesiyle olabileceği gibi performansı daha iyi hale getirmek için başlar.

1. Farklı sorgu türlerini alabilmeniz için sorgu isteklerinin yapısını farklılık gösterir. Her arama sorgusu aynı düzeyde uygulanmaz. Örneğin, bir belge arama veya arama önerisi genellikle önemli sayıda model ve filtre içeren bir sorgudan daha hızlıdır. Test kompozisyonu, kabaca, üretimde bekleeceğiniz oranlar halinde çeşitli sorgular içermelidir.  

Bu test iş yüklerini oluştururken aklınızda bulundurmanız gereken bazı Azure Bilişsel Arama özellikleri vardır:

+ Tek seferde çok fazla arama sorgusu göndererek hizmetinizi aşırı yükleme olasılığı vardır. Bu durumda, HTTP 503 yanıt kodları görürsünüz. Test sırasında 503 önlemek için, daha fazla arama isteği eklerken gecikme hızlarındaki farkları görmek üzere çeşitli arama istekleri aralıklarıyla başlayın.

+ Azure Bilişsel Arama, dizin oluşturma görevlerini arka planda çalıştırmaz. Hizmetiniz sorgu ve dizin oluşturma iş yüklerini eşzamanlı olarak işlerinizde, sorgu testleriniz için dizin oluşturma işleri sunarak ya da yoğun saatlerde dizin oluşturma işlerini çalıştırmaya yönelik seçenekleri inceleyerek bunu hesaba sunun.

> [!Tip]
> Yük testi araçlarını kullanarak gerçekçi bir sorgu yükünün benzetimini yapabilirsiniz. [Azure DevOps ile yük testi](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=azure-devops) yapmayı deneyin veya bu [alternatifden](/azure/devops/test/load-test/overview?view=azure-devops#alternatives)birini kullanın.

## <a name="scale-for-high-query-volume"></a>Yüksek sorgu hacmi için ölçeklendirin

Sorgular çok uzun sürmeye başladığında veya hizmet istekleri bırakmaya başladığında bir hizmet aşırı kullanılıyor. Böyle bir durumla karşılaşırsanız, sorunu iki şekilde ele alabilirsiniz:

+ **Çoğaltmalar ekleme**  

  Her çoğaltma, verilerinizin birden çok kopyaya karşı yük dengelemesi yapmasına izin veren verilerinizin bir kopyasıdır.  Tüm yük dengeleme ve veri çoğaltma işlemi Azure Bilişsel Arama tarafından yönetilir ve hizmetiniz için ayrılan çoğaltmaların sayısını dilediğiniz zaman değiştirebilirsiniz. Standart bir arama hizmetinde 12 ' ye kadar çoğaltma ve temel bir arama hizmetinde 3 çoğaltma ayırabilirsiniz. Çoğaltmalar [Azure Portal](search-create-service-portal.md) ya da [PowerShell](search-manage-powershell.md)'den ayarlanabilir.

+ **Daha yüksek bir katmanda yeni bir hizmet oluşturun**  

  Azure Bilişsel Arama [birkaç katmanda](https://azure.microsoft.com/pricing/details/search/) gelir ve her biri farklı performans düzeyleri sunar. Bazı durumlarda, üzerinde yaptığınız katmanda çoğaltmalar ne zaman olsa bile yeterli sayıda sorgu sağlayamadığımdan çok sayıda sorguya sahip olabilirsiniz. Bu durumda, çok sayıda belge ve çok yüksek sorgu iş yükleri gibi senaryolar için tasarlanan Standart S3 katmanı gibi daha yüksek performanslı bir katmana geçmeyi göz önünde bulundurun.

## <a name="scale-for-slow-individual-queries"></a>Yavaş tek sorgular için ölçeklendirin

Yüksek gecikme oranları için bir diğer neden, tek bir sorgunun tamamlanmasını çok uzun sürüyor. Bu durumda, çoğaltmaları eklemek yardımcı olmayacaktır. Aşağıdakileri içeren olası iki seçenek vardır:

+ **Bölümleri artırma**

  Bir bölüm, verileri ek bilgi işlem kaynakları arasında böler. İki bölüm, verileri yarı bir şekilde ayırır, üçüncü bir bölüm onu üç ve diğerleri olarak böler. Bir pozitif yan etki, daha yavaş sorguların bazen paralel bilgi işlem nedeniyle daha hızlı bir şekilde gerçekleştirilemesidir. Çok sayıda belgeyle eşleşen sorgular veya çok sayıda belge üzerinde sayı sağlayan modeller gibi düşük seçiciliği sorgularında paralelleştirme belirtiyoruz. Belgelerin yeniden dengeliğine veya belge numaralarını saymaya yönelik önemli bir hesaplama gerektiğinden, ek bölümler eklemek sorguların daha hızlı tamamlanmasını sağlar.  
   
  Temel arama hizmetinde standart arama hizmetinde ve 1 bölümde en fazla 12 bölüm olabilir. Bölümler [Azure Portal](search-create-service-portal.md) ya da [PowerShell](search-manage-powershell.md)'den ayarlanabilir.

+ **Yüksek kardinalite alanlarını sınırla**

  Yüksek bir kardinalite alanı, önemli sayıda benzersiz değere sahip çok yönlü veya filtrelenebilir bir alandan oluşur ve sonuç olarak sonuçları hesaplarken önemli kaynakları tüketir. Örneğin, bir ürün KIMLIĞI veya açıklama alanını çok yönlü tablo/filtre olarak ayarlamak, belgedeki değerlerin çoğu benzersiz olduğundan yüksek önem düzeyi olarak sayılır. Mümkün olan yerlerde, yüksek kardinalite alanlarının sayısını sınırlandırın.

+ **Arama katmanını artır**  

  Daha yüksek bir Azure Bilişsel Arama katmanına taşımak, yavaş sorguların performansını artırmanın başka bir yolu olabilir. Her iki katman da daha hızlı CPU ve daha fazla bellek sağlar ve bunların her ikisi de sorgu performansı üzerinde olumlu bir etkiye sahiptir.

## <a name="scale-for-availability"></a>Kullanılabilirlik için ölçeklendirin

Çoğaltmalar yalnızca sorgu gecikmesini azaltmaya yardımcı olur, ancak yüksek kullanılabilirliğe de izin verebilir. Tek bir çoğaltmayla, yazılım güncelleştirmelerinden sonra veya gerçekleşen diğer bakım olayları için sunucu yeniden başlatılması nedeniyle dönemsel kapalı kalma süresi beklemelisiniz. Sonuç olarak, uygulamanızın çok yüksek düzeyde aramalar (sorgular) ve yazmaları (Dizin oluşturma olayları) gerektirip gerektirmediğini göz önünde bulundurmanız önemlidir. Azure Bilişsel Arama, tüm ücretli arama tekliflerindeki SLA seçeneklerini aşağıdaki özniteliklerle sunar:

+ Salt okuma iş yüklerinin yüksek kullanılabilirliği için iki çoğaltma (sorgular)

+ Okuma/yazma iş yüklerinin yüksek kullanılabilirliği için üç veya daha fazla çoğaltma (sorgular ve dizin oluşturma)

Bunun hakkında daha fazla bilgi için lütfen [Azure Bilişsel Arama hizmet düzeyi sözleşmesi](https://azure.microsoft.com/support/legal/sla/search/v1_0/)ziyaret edin.

Çoğaltmalar verilerinizin kopyaları olduğundan, birden fazla kopyaya sahip olmak Azure Bilişsel Arama 'nin makine yeniden başlatmaları ve bakım yapmasına izin verdiğinden, sorgu yürütme diğer çoğaltmalarda devam eder. Buna karşılık, çoğaltmaları dışarıda bırakırsanız, bu çoğaltmaların bir veya daha fazla kaynak olduğu varsayıldığında sorgu performansı düşüşüne tabi olursunuz.

## <a name="scale-for-geo-distributed-workloads-and-geo-redundancy"></a>Coğrafi olarak dağıtılan iş yükleri ve coğrafi yedeklilik için ölçeklendirin

Coğrafi olarak dağıtılan iş yükleri için, ana bilgisayar veri merkezinden uzakta bulunan kullanıcılar daha yüksek gecikme süresine sahip olur. Bir hafifletme, bölgelere bu kullanıcılara daha yakından yaklaşarak birden çok arama hizmeti sağlamak.

Azure Bilişsel Arama, bölge genelinde Azure Bilişsel Arama dizinlerinin coğrafi olarak çoğaltılmasının otomatik bir yöntemini sağlamıyor, ancak bu işlemi uygulamak ve yönetmek için basit hale getirmek üzere kullanılabilecek bazı teknikler vardır. Bunlar, sonraki birkaç bölümde özetlenmiştir.

Coğrafi olarak dağıtılmış bir arama hizmetleri kümesinin amacı, iki veya daha fazla bölgede iki veya daha fazla dizine sahip olmaktır. Bu durumda, bir kullanıcının Azure Bilişsel Arama hizmetine yönlendirildiği, bu örnekte görüldüğü gibi en düşük gecikme süresi sağlar:

   ![Bölgeye göre hizmetlerin çapraz sekmesi][1]

### <a name="keep-data-synchronized-across-multiple-services"></a>Verileri birden çok hizmet arasında eşitlenmiş durumda tut

[Azure bilişsel arama Dizin oluşturucuyu](search-indexer-overview.md) veya anında iletme API 'Sini ( [Azure bilişsel arama REST API](/rest/api/searchservice/)olarak da bilinir) kullanarak, dağıtılmış arama hizmetlerinizi eşitlenmiş halde tutmanın iki seçeneği vardır.  

### <a name="use-indexers-for-updating-content-on-multiple-services"></a>Birden çok hizmet üzerinde içerik güncelleştirmek için Dizin oluşturucular kullanma

Bir hizmette zaten Dizin Oluşturucu kullanıyorsanız, ikinci bir hizmette ikinci bir Dizin Oluşturucu yapılandırarak aynı veri kaynağı nesnesini kullanarak aynı konumdan veri çekmesini sağlayabilirsiniz. Her bölgedeki her bir hizmetin kendi Dizin Oluşturucusu ve bir hedef dizini vardır (arama dizininiz paylaşılmaz, bu da veriler yinelenir), ancak her Dizin Oluşturucu aynı veri kaynağına başvurur.

Mimarinin nasıl görüneceğine ilişkin üst düzey bir görsel aşağıda verilmiştir.

   ![Dağıtılmış Dizin Oluşturucu ve hizmet bileşimleri içeren tek veri kaynağı][2]

### <a name="use-rest-apis-for-pushing-content-updates-on-multiple-services"></a>Birden çok hizmete içerik güncelleştirmelerini göndermek için REST API 'Lerini kullanma

Azure [bilişsel arama dizininizdeki içeriği göndermek](/rest/api/searchservice/update-index)için azure bilişsel arama REST API kullanıyorsanız, her bir güncelleştirme gerektiğinde tüm arama hizmetlerine değişiklikleri göndererek çeşitli arama hizmetlerinizi eşitlenmiş halde tutabilirsiniz. Kodunuzda, bir arama hizmetine yapılan bir güncelleştirmenin başarısız olduğu ancak diğer arama hizmetleri için başarılı olduğu durumları işlediğinizden emin olun.

## <a name="leverage-azure-traffic-manager"></a>Azure Traffic Manager 'ten yararlanın

[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) , istekleri birden çok arama hizmeti tarafından desteklenen, coğrafi olarak bulunan birden çok Web sitesine yönlendirmenize olanak tanır. Traffic Manager avantajlarından biri, kullanılabilir olmasını sağlamak için Azure Bilişsel Arama araştırma yapabilir ve kapalı kalma durumunda kullanıcıları alternatif arama hizmetleri 'ne yönlendirebilir. Ayrıca, Azure Web siteleri aracılığıyla arama isteklerini yönlendirçalışıyorsanız Azure Traffic Manager, Web sitesinin en fazla Azure Bilişsel Arama olmadığı, Yük Dengeleme durumlarını yüklemenize izin verir. Traffic Manager yararlanan mimarinin bir örneği aşağıda verilmiştir.

   ![Merkezi Traffic Manager ile bölgelere göre hizmetlerin çapraz sekmesi][3]

## <a name="next-steps"></a>Sonraki adımlar

Her biri için fiyatlandırma katmanları ve hizmet limitleri hakkında daha fazla bilgi edinmek için bkz. [hizmet limitleri](search-limits-quotas-capacity.md). Bölüm ve çoğaltma birleşimleri hakkında daha fazla bilgi edinmek için bkz. [kapasiteyi planlayın](search-capacity-planning.md) .

Bu makalede ele alınan tekniklerin performansı ve gösterileri hakkında bir tartışma için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png