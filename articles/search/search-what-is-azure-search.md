---
title: Azure Bilişsel Arama’ya giriş
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, Microsoft 'un tam olarak yönetilen bir bulut arama hizmetidir. Kullanım örnekleri, geliştirme iş akışı, diğer Microsoft arama ürünleriyle karşılaştırmalar ve nasıl başlaleyeceğinizi hakkında bilgi edinin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 11/24/2020
ms.custom: contperfq1
ms.openlocfilehash: 1d5047a9be6b4cf53304858d12a0cb7b49b575ca
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499926"
---
# <a name="what-is-azure-cognitive-search"></a>Azure Bilişsel Arama nedir?

Azure Bilişsel Arama ([eskiden "Azure Search" olarak bilinirdi](whats-new.md#new-service-name)), geliştiriciler için Web, mobil ve kurumsal uygulamalarda özel, heterojen içerik üzerinde zengin arama deneyimi oluşturmaya yönelik API 'ler ve araçlar sağlayan bir bulut arama hizmetidir. 

Bir Bilişsel Arama hizmeti oluşturduğunuzda şunları alırsınız:

+ Dizin oluşturma ve sorgu yürütmeyi gerçekleştiren bir arama altyapısı
+ Dizin oluşturma sırasında görüntülerin ve ayırt edilmemiş metnin AI-ortalanmış Analizi ve dönüştürmesi
+ oluşturduğunuz ve yönettiğiniz arama dizinlerinin kalıcı depolaması
+ karmaşık sorguları basit sorgular oluşturmak için bir sorgu dili

Mimari türsel olarak, bir arama hizmeti, dizini oluşturulmamış verilerinizi içeren dış veri depoları arasında ve bir arama dizinine sorgu istekleri gönderen ve yanıtı işleyen bir istemci uygulaması arasında yer alır.

![Azure Bilişsel Arama mimarisi](media/search-what-is-azure-search/azure-search-diagram.svg "Azure Bilişsel Arama mimarisi")

Outwardly, bir arama hizmeti, Azure veri kaynaklarından veri alımı/alımı otomatikleştiren diğer Azure hizmetleriyle ve görüntü ve metin analizi gibi bilişsel hizmetlerden ya da Azure Machine Learning veya Azure Işlevleri içinde oluşturduğunuz özel *AI 'nin* TÜKETILEBILIR AI olan *becerileri* 'teki diğer Azure hizmetleriyle tümleşir.

Arama hizmetinin kendisinde, iki birincil iş yükü *Dizin oluşturma* ve *sorgulama* yapılır. 

+ Dizin oluşturma, metni arama hizmetinize getirir ve aranabilir hale getirir. Dahili olarak, gelen metinler belirteçlerde işlenir ve hızlı taramalar için ters dizinler halinde depolanır. 

  Dizin oluşturma içinde, bilişsel yetenekler aracılığıyla *AI zenginleştirme* ekleme seçeneğiniz vardır. bu [beceri](cognitive-search-working-with-skillsets.md), Microsoft 'tan veya oluşturduğunuz özel becerilerden önceden tanımlanmış olanlardan yararlanın. Sonraki analizler ve dönüştürmeler, daha önce mevcut olmayan yeni bilgi ve yapılara neden olabilir ve birçok arama ve bilgi araştırma senaryosu için yüksek yardımcı program sağlar.

+ Bir dizin aranabilir verilerle doldurulduktan sonra, istemci uygulamanız bir arama hizmetine sorgu istekleri gönderir ve yanıtları işler. Tüm sorgu yürütmesi, hizmetinize oluşturduğunuz, sahip olduğunuz ve depoladığınız bir arama dizininden daha fazla. İstemci uygulamanızda, arama deneyimi Azure Bilişsel Arama API 'Leri kullanılarak tanımlanır ve ilgi ayarlama, otomatik tamamlama, eş anlamlı eşleştirme, belirsiz eşleştirme, model eşleştirme, filtre ve sıralama dahil olabilir.

Bilgi alma sürecinin karmaşıklığını maskeleyen basit bir [REST API’si](/rest/api/searchservice/) veya [.NET SDK’sı](search-howto-dotnet-sdk.md) aracılığıyla bu işlev sunulur. Ayrıca, hizmet yönetimi ve içerik yönetimi için Azure portal kullanarak, prototiplerinizi prototip ve becerileri sorgulama ve dizin oluşturma araçları da kullanabilirsiniz. Hizmet bulutta çalıştığından, altyapı ve kullanılabilirlik Microsoft tarafından yönetilir.

## <a name="why-use-cognitive-search"></a>Bilişsel Arama neden kullanılmalıdır

Azure Bilişsel Arama aşağıdaki uygulama senaryoları için uygundur:

+ Heterojen içeriği özel, Kullanıcı tanımlı bir arama dizinine birleştirin. Bir arama dizinini, herhangi bir kaynaktaki JSON belgelerinin akışları ile doldurabilirsiniz. Azure 'da desteklenen kaynaklar için dizin oluşturmayı otomatikleştirmek üzere bir *Dizin Oluşturucu* kullanın. Dizin şeması üzerinde denetim ve yenileme zamanlaması, Bilişsel Arama kullanmanın önemli bir nedenidir.

+ Aramayla ilgili özelliklerin kolay uygulanması. Arama API 'Leri sorgu oluşturmayı, çok yönlü gezintiyi, filtreleri (coğrafi uzamsal arama dahil), eş anlamlı eşleme, otomatik tamamlama ve ilgi ayarlamayı basitleştirir. Yerleşik özellikleri kullanarak, ticari Web araması altyapılarına benzer bir arama deneyimine yönelik son kullanıcı beklentilerini karşılamasını sağlayabilirsiniz.

+ Ham içerik, Azure Blob depolamada veya Cosmos DB depolanan büyük metin veya görüntü dosyaları ya da uygulama dosyalarıdır. Metin tanımlamak ve ayıklamak, yapı oluşturmak veya çevrilmiş metin veya varlıklar gibi yeni bilgiler oluşturmak için dizin oluşturma sırasında bilişsel [becerileri](cognitive-search-concept-intro.md) uygulayabilirsiniz.

+ İçerik dile veya özel metin çözümlemesine ihtiyaç duyuyor. Ingilizce olmayan içeriğiniz varsa Azure Bilişsel Arama hem Lucene Çözümleyicileri hem de Microsoft 'un doğal dil işlemcilerini destekler. Ayrıca, vurguları, aksanların filtrelenmesi veya dizelerde desenler tanınması veya korunması gibi ham içeriğin özel işlemesini elde etmek için de yapılandırabilirsiniz.

Belirli işlevler hakkında daha fazla bilgi için bkz. [Azure bilişsel arama özellikleri](search-features-list.md)

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Temel arama özelliklerinin uçtan uca incelenmesi, dört adımda elde edilebilir:

1. Diğer aboneler ile paylaşılan ücretsiz katmanda veya yalnızca hizmetiniz tarafından kullanılan adanmış kaynaklar için [ücretli bir katmanda](https://azure.microsoft.com/pricing/details/search/) [**bir arama hizmeti oluşturun**](search-create-service-portal.md) . Tüm hızlı başlangıçlar ve öğreticiler ücretsiz hizmetle tamamlanabilir.

1. Portalı kullanarak [**bir arama dizini oluşturun**](search-what-is-an-index.md) , [REST API](/rest/api/searchservice/create-index). [.NET SDK](search-howto-dotnet-sdk.md)veya başka bir SDK. Dizin şeması Aranabilir içeriğin yapısını tanımlar.

1. [**İçeriği dizine yükleyin**](search-what-is-data-import.md) . Herhangi bir kaynaktaki JSON belgelerini göndermek için ["Push" modelini](tutorial-optimize-indexing-push-api.md) kullanın veya kaynak verileriniz Azure üzerinde ise ["çekme" modelini (Dizin oluşturucular)](search-indexer-overview.md) kullanın.

1. Portal, [REST API](search-get-started-rest.md), [.NET SDK](/dotnet/api/azure.search.documents.searchclient.search)veya başka bir SDK 'daki [Arama Gezgini](search-explorer.md) 'ni kullanarak [**bir dizini sorgulayın**](search-query-overview.md) .

> [!TIP]
> Birkaç dakika içinde bir dizin oluşturmak, yüklemek ve sorgulamak için [**veri alma Sihirbazı 'nı**](search-get-started-portal.md) ve bir Azure veri kaynağını kullanmaya başlayarak adımları en aza indirin.

## <a name="how-it-compares"></a>Karşılaştırma

Müşteriler genellikle Azure Bilişsel Arama diğer aramayla ilgili diğer çözümlerle nasıl karşılaştırdığını sorar. Aşağıdaki tabloda temel farklılıklar özetlenmiştir.

| Karşılaştırılan | Temel farklılıklar |
|-------------|-----------------|
| Microsoft Arama | [Microsoft Search](/microsoftsearch/overview-microsoft-search) , SharePoint 'te içerik üzerinde sorgu yapması gereken Microsoft 365 kimliği doğrulanmış kullanıcılar içindir. Microsoft ve diğer kaynaklardan gelen bağlayıcılar aracılığıyla dış içeriği kabul etme olanağı sunan yöneticiler tarafından etkinleştirilmiş ve yapılandırılmış kullanıma sunulan bir arama deneyimi olarak sunulur. Senaryonuzu açıkladıysanız Microsoft Search 'ü Microsoft 365 keşfetmeye yönelik etkileyici bir seçenektir.<br/><br/>Buna karşılık, Azure Bilişsel Arama sorguları, tanımladığınız bir dizin üzerinde yürütür, genellikle farklı kaynaklardan, sahip olduğunuz veriler ve belgelerle doldurulur. Azure Bilişsel Arama, bazı Azure veri kaynakları için Dizin [oluşturucular](search-indexer-overview.md)aracılığıyla gezgin özelliklerine sahiptir, ancak dizin şemanıza uyan HERHANGI bir JSON belgesini tek ve birleştirilmiş bir aranabilir kaynağa gönderebilirsiniz. Ayrıca, dizin oluşturma işlem hattını makine öğrenimi ve sözlü Çözümleyicileri içerecek şekilde özelleştirebilirsiniz. Bilişsel Arama, büyük çözümlerde eklenti bileşeni olacak şekilde oluşturulduğundan, aramayı dilediğiniz platformda neredeyse her uygulamayla tümleştirebilirsiniz.|
|Bing | [Bing Web Araması API'si](../cognitive-services/bing-web-search/index.yml), Bing.com adresindeki dizinlerde gönderdiğiniz eşleşen terimleri arar. Dizinler, HTML, XML ve genel sitelerdeki diğer web içeriklerinden derlenir. Aynı temel üzerine oluşturulan [Bing Özel Arama](/azure/cognitive-services/bing-custom-search/), tek tek web sitelerine kapsamı belirlenmiş şekilde web içeriği türleri için aynı gezgin teknolojisini sunar.<br/><br/>Bilişsel Arama, dizini tanımlayabilir ve doldurabilirsiniz. Dizin [oluşturucular](search-indexer-overview.md) kullanarak Azure veri kaynaklarında verileri arayabilir veya herhangi bir DIZINLE uyumlu JSON belgesini arama hizmetinize gönderebilirsiniz. |
|Veritabanı araması | Birçok veritabanı platformu yerleşik bir arama deneyimi içerir. SQL Server'ın [tam metin araması](/sql/relational-databases/search/full-text-search) vardır. Cosmos DB ve benzeri teknolojilerin sorgulanabilir dizinleri vardır. Arama ile depolamayı birleştiren ürünler değerlendirilirken, hangisinin tercih edileceğini saptamak zor olabilir. Birçok çözüm, her ikisini de kullanır: depolama için DBMS ve özelleştirilmiş arama özellikleri için Azure Bilişsel Arama.<br/><br/>Azure Bilişsel Arama, DBMS aramasına kıyasla, heterojen kaynaklardan içerik depolar ve [56 dilde](/rest/api/searchservice/language-support)dile duyarlı metin işleme (sözcük kullanımı, katileştirme, Word Forms) gibi özel metin işleme özellikleri sunar. Ayrıca yanlış yazılmış sözcükleri otomatik düzeltme, [eş anlamlılar](/rest/api/searchservice/synonym-map-operations), [öneriler](/rest/api/searchservice/suggestions), [puanlama denetimleri](/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [modeller](./search-filters-facets.md) ve [özel belirteçlere ayırma](/rest/api/searchservice/custom-analyzers-in-azure-search) özelliklerini de destekler. Azure Bilişsel Arama 'daki [tam metin arama altyapısı](search-lucene-query-architecture.md) , bilgi alma bölümünde sektör standardı olan Apache Lucene üzerinde oluşturulmuştur. Ancak, Azure Bilişsel Arama ters bir dizin biçimindeki verileri sürdürüyor olsa da, doğru veri depolama alanı için bir değiştirme değildir ve bu kapasitede kullanılması önerilmez. Daha fazla bilgi için bu [forum gönderisine](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data) bakın. <br/><br/>Kaynak kullanımı, bu kategorideki başka bir çekim noktasıdır. Dizin oluşturma ve bazı sorgu işlemleri çoğunlukla yoğun bilgi işlem içerir. Arama DBMS'den buluttaki ayrılmış bir çözüme aktarıldığında işlem yürütme için sistem kaynakları korunur. Üstelik, aramayı harici hale getirerek, sorgu hacmiyle eşleşecek şekilde kolayca ölçeği ayarlayabilirsiniz.|
|Ayrılmış arama çözümü | Tüm işlevleriyle birlikte ayrılmış arama kullanmaya karar verdiğinizi varsayarak, son kategorik karşılaştırma şirket içi çözümlerle bulut hizmetleri arasındadır. Birçok arama teknolojisi dizin oluşturma ve sorgu işlem hatları üzerinde kontrol, daha zengin sorgulama ve filtreleme söz dizimine erişim, derece ve ilgi düzeyi üzerinde kontrol ve kendinden yönlendirmeli ve akıllı arama özellikleri sunar. <br/><br/>Minimum ek yük ve bakım ile anahtar teslim bir çözüm istiyorsanız bulut hizmeti doğru seçimdir. <br/><br/>Bulut paradigması içinde birçok sağlayıcı, tam metin araması, coğrafi arama ve arama girişlerindeki belirli bir belirsizlik düzeyini işleme yeteneğiyle birlikte karşılaştırılabilir temel özellikler sunar. Genellikle bu [özel bir özellik olup](search-features-list.md) en iyi uygunluğu belirleyen yönetimin, araçların ve API’lerin kolaylaştırılmasını sağlar. |

Bulut sağlayıcıları arasında Azure Bilişsel Arama, birincil olarak hem bilgi alımı hem de içerik gezintisi için arama yapan uygulamalar için Azure 'daki içerik depoları ve veritabanları üzerinde tam metin araması iş yükleri için en güçlü bir güvendir. 

Temel güçlü yönleri şunlardır:

+ Dizin oluşturma katmanında Azure veri tümleştirmesi (gezginler)
+ Merkezi yönetim için Azure portalı
+ Azure ölçekleme, güvenilirlik ve birinci sınıf kullanılabilirlik
+ Resimlerin metin veya yapılandırılmamış içerikte desenler bulma dahil olmak üzere, ham verileri daha fazla aranabilir hale getirmek için AI işleme.
+ 56 dilde güçlü metin araması için çözümleyicilerle birlikte linguistik ve özel analiz
+ [Arama odaklı uygulamalarda ortak olan temel özellikler](search-features-list.md): puanlama, modelleme, öneriler, eş anlamlılar, coğrafi arama ve daha fazlası.

Müşterilerimiz arasında, Azure Bilişsel Arama ' deki en geniş özelliklerden faydalanabilir. Çevrimiçi Kataloglar, iş kolu programları ve belge bulma uygulamaları vardır.

## <a name="watch-this-video"></a>Bu videoyu izleyin

Bu 15 dakikalık videoda, program Yöneticisi Lusıs Cabrera, Azure Bilişsel Arama tanıtır.

>[!VIDEO https://www.youtube.com/embed/kOJU0YZodVk?version=3]