---
title: Azure Bilişsel Arama’ya giriş
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, Microsoft 'un tam olarak yönetilen bir bulut arama hizmetidir. Kullanım örnekleri, geliştirme iş akışı, diğer Microsoft arama ürünleriyle karşılaştırmalar ve nasıl başlaleyeceğinizi hakkında bilgi edinin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 09/15/2020
ms.openlocfilehash: e4cee699bf18b340d0bb1cbe783bdedcca537db6
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90602957"
---
# <a name="what-is-azure-cognitive-search"></a>Azure Bilişsel Arama nedir?

Azure Bilişsel Arama ([eskiden "Azure Search" olarak bilinirdi](whats-new.md#new-service-name)), geliştiriciler için Web, mobil ve kurumsal uygulamalarda özel, heterojen içerik üzerinde zengin arama deneyimi oluşturmaya yönelik API 'ler ve araçlar sağlayan bir bulut arama hizmetidir.

Bir Bilişsel Arama hizmeti oluşturduğunuzda, bir dizin oluşturma ve sorgu altyapısı, oluşturduğunuz ve yönettiğiniz arama dizinlerinin kalıcı olarak depolanması ve basit ve karmaşık sorgular oluşturmaya yönelik bir sorgu dili elde edersiniz. Bir arama hizmeti, Azure veri kaynaklarından veri alımı/alımı otomatikleştiren *Dizin oluşturucular* ve görüntü ve metin analizi gibi bilişsel hizmetlerden AI *Işleme ekleyen diğer* Azure hizmetleriyle tümleşir.

![Azure Bilişsel Arama mimarisi](media/search-what-is-azure-search/azure-search-diagram.svg "Azure Bilişsel Arama mimarisi")

Mimari türsel olarak, bir arama hizmeti, verilerinizi içeren dış veri depoları arasında ve sorgu istekleri gönderen ve yanıtları işleyen bir istemci uygulaması arasında yer alır. Bir arama hizmetinin iki birincil iş yükü *dizine* alınır ve *sorgulama*yapılır.

Dizin oluşturma, arama hizmetinize içerik ekler ve aranabilir hale gelir. Dahili olarak, gelen metinler belirteçlerde işlenir ve hızlı eşleştirme için ters dizinler halinde depolanır. Bir dizin şeması Aranabilir içeriğin yapısını belirler. Dizin oluşturma sırasında Microsoft 'un veya oluşturduğunuz özel becerilerin önceden tanımlanmış olduğu bilişsel *yetenekler*ekleme seçeneğiniz vardır. Elde edilen analiz ve dönüştürmeler, daha önce mevcut olmayan yeni bilgi ve yapılar oluşturabilir ve birçok arama ve bilgi araştırma senaryosu için yüksek yardımcı program sağlar.

Bir dizin aranabilir verilerle doldurulduktan sonra, istemci uygulamanız bir arama hizmetine sorgu istekleri gönderir ve yanıtları işler. Arama deneyimi, Azure Bilişsel Arama API 'Leri kullanarak istemcinizde tanımlanmıştır ve ilgi ayarlama, otomatik tamamlama, eş anlamlı eşleştirme, belirsiz eşleştirme, model eşleştirme, filtre ve sıralama içerebilir.

Bilgi alma sürecinin karmaşıklığını maskeleyen basit bir [REST API’si](/rest/api/searchservice/) veya [.NET SDK’sı](search-howto-dotnet-sdk.md) aracılığıyla bu işlev sunulur. Ayrıca, hizmet yönetimi ve içerik yönetimi için Azure portal kullanarak, prototiplerinizi prototip ve becerileri sorgulama ve dizin oluşturma araçları da kullanabilirsiniz. Hizmet bulutta çalıştığından, altyapı ve kullanılabilirlik Microsoft tarafından yönetilir.

## <a name="when-to-use-cognitive-search"></a>Ne zaman kullanılacağı Bilişsel Arama

Azure Bilişsel Arama aşağıdaki uygulama senaryoları için uygundur:

+ Heterojen içerik türlerini özel, Kullanıcı tanımlı bir arama dizinine birleştirme. Bir arama dizinini, herhangi bir kaynaktaki JSON belgelerinin akışları ile doldurabilirsiniz. Azure 'da desteklenen kaynaklar için dizin oluşturmayı otomatikleştirmek üzere bir *Dizin Oluşturucu* kullanın. Dizin şeması üzerinde denetim ve yenileme zamanlaması, Bilişsel Arama kullanmanın önemli bir nedenidir.

+ Aramayla ilgili özelliklerin kolay uygulanması. Arama API 'Leri sorgu oluşturmayı, çok yönlü gezintiyi, filtreleri (coğrafi uzamsal arama dahil), eş anlamlı eşleme, otomatik tamamlama ve ilgi ayarlamayı basitleştirir. Yerleşik özellikleri kullanarak, ticari Web araması altyapılarına benzer bir arama deneyimine yönelik son kullanıcı beklentilerini karşılamasını sağlayabilirsiniz.

+ Ham içerik, Azure Blob depolamada veya Cosmos DB depolanan büyük metin veya görüntü dosyaları ya da uygulama dosyalarıdır. Metin tanımlamak ve ayıklamak, yapı oluşturmak veya çevrilmiş metin veya varlıklar gibi yeni bilgiler oluşturmak için dizin oluşturma sırasında bilişsel [becerileri](cognitive-search-concept-intro.md) uygulayabilirsiniz.

+ İçerik dile veya özel metin çözümlemesine ihtiyaç duyuyor. Ingilizce olmayan içeriğiniz varsa Azure Bilişsel Arama hem Lucene Çözümleyicileri hem de Microsoft 'un doğal dil işlemcilerini destekler. Ayrıca, vurguları, aksanların filtrelenmesi veya dizelerde desenler tanınması veya korunması gibi ham içeriğin özel işlemesini elde etmek için de yapılandırabilirsiniz.

Belirli işlevler hakkında daha fazla bilgi için bkz. [Azure bilişsel arama özellikleri](search-features-list.md)

## <a name="how-to-use-cognitive-search"></a>Bilişsel Arama kullanma

### <a name="step-1-provision-service"></a>1. Adım: Hizmeti sağlama

Diğer aboneler ile paylaşılan [ücretsiz bir hizmet](search-create-service-portal.md) veya yalnızca hizmetiniz tarafından kullanılan kaynakları ayıran [ücretli bir katman](https://azure.microsoft.com/pricing/details/search/) oluşturabilirsiniz. Tüm hızlı başlangıçlar ve öğreticiler ücretsiz hizmetle tamamlanabilir.

Ücretli katmanlar için, üretim gereksinimlerine göre kaynak kullanımı ayarlamak üzere bir hizmeti iki boyutta ölçeklendirebilirsiniz:

+ Yoğun sorgu yüklerini işlemek için kapasitenizi büyütmek üzere çoğaltmalar ekleyin
+ Daha fazla belge için depolamayı büyütmek üzere bölüm ekleyin

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma

Bir veritabanındaki alanlara benzer şekilde, aramak istediğiniz belgelerin yapısını yansıtacak şekilde eşlenecek bir dizin şeması tanımlayın. Arama dizini, hızlı sorgu yürütme için iyileştirilmiş özel bir veri yapısıdır.

[Dizin şemasını Azure Portal](search-what-is-an-index.md)veya [.net SDK](search-howto-dotnet-sdk.md) veya [REST API](/rest/api/searchservice/)kullanarak programlı bir şekilde oluşturmak yaygındır.

### <a name="step-3-load-data"></a>3. Adım: Verileri yükleme

Bir dizin tanımladıktan sonra içeriği karşıya yüklemeye hazır olursunuz. Bir itme veya çekme modeli kullanabilirsiniz.

Push modeli, bir [SDK](search-howto-dotnet-sdk.md) veya [rest](/rest/api/searchservice/addupdate-or-delete-documents)'TEN API 'ler kullanarak JSON belgelerini bir dizine gönderir. Dış veri kümesi, belgeler JSON olduğu sürece neredeyse herhangi bir veri kaynağı olabilir.

Çekme modeli, Azure 'daki kaynaklardan alınan verileri "çeker" ve bir arama dizinine gönderir. Çekme modeli, verileri bağlama, okuma ve seri hale getirme gibi veri alımı özelliklerini kolaylaştıran ve otomatikleştiren [*Dizin oluşturucular*](/rest/api/searchservice/Indexer-operations) aracılığıyla uygulanır. Desteklenen veri kaynakları Azure Cosmos DB, Azure SQL ve Azure depolama içerir.

### <a name="step-4-send-queries-and-handle-responses"></a>4. Adım: sorguları gönderme ve yanıtları işleme

Bir dizini doldurduktan sonra, [REST API](/rest/api/searchservice/Search-Documents) veya [.NET SDK](/dotnet/api/microsoft.azure.search.idocumentsoperations)ile basit http isteklerini kullanarak hizmet uç noktanıza [arama sorguları](search-query-overview.md) verebilirsiniz.

Kullanıcı girişi toplayan ve sonuçları işleyen bir Web sayfasını derlemek ve genişletmek için [ilk arama uygulamanızı oluşturun](tutorial-csharp-create-first-app.md) . Ayrıca, var olan bir dizini sorgulamak için Azure portal ' de etkileşimli REST çağrıları veya yerleşik [Arama Gezgini](search-explorer.md) [için Postman](search-get-started-postman.md) kullanabilirsiniz.

## <a name="how-it-compares"></a>Karşılaştırma

Müşteriler genellikle Azure Bilişsel Arama diğer aramayla ilgili diğer çözümlerle nasıl karşılaştırdığını sorar. Aşağıdaki tabloda temel farklılıklar özetlenmiştir.

| Karşılaştırılan | Temel farklılıklar |
|-------------|-----------------|
|Bing | [Bing Web Araması API'si](../cognitive-services/bing-web-search/index.yml), Bing.com adresindeki dizinlerde gönderdiğiniz eşleşen terimleri arar. Dizinler, HTML, XML ve genel sitelerdeki diğer web içeriklerinden derlenir. Aynı temel üzerine oluşturulan [Bing Özel Arama](/azure/cognitive-services/bing-custom-search/), tek tek web sitelerine kapsamı belirlenmiş şekilde web içeriği türleri için aynı gezgin teknolojisini sunar.<br/><br/>Azure Bilişsel Arama, tanımladığınız bir dizini ve sahip olduğunuz verileri, genellikle farklı kaynaklardan gelen belgeleri arar. Azure Bilişsel Arama, Dizin [oluşturucular](search-indexer-overview.md)aracılığıyla bazı veri kaynakları için Gezgin yeteneklerine sahiptir, ancak dizin şemanıza uyan tüm JSON belgelerini tek ve birleştirilmiş bir aranabilir kaynağa gönderebilirsiniz. |
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