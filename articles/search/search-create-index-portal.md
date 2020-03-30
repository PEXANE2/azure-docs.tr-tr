---
title: Azure portalında arama dizini oluşturma
titleSuffix: Azure Cognitive Search
description: Yerleşik bir portal dizin tasarımcısını kullanarak Azure Bilişsel Arama için nasıl bir dizin oluşturabilirsiniz öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f2e875c625431867e6e83cfd1e0b2c6d7a2781f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112851"
---
# <a name="create-an-azure-cognitive-search-index-in-the-portal"></a>Portalda Azure Bilişsel Arama dizini oluşturma

Azure Bilişsel Arama, portalda prototipler için yararlı olan yerleşik bir dizin tasarımcısı veya Azure Bilişsel Arama hizmetinizde barındırılan bir [arama dizini](search-what-is-an-index.md) oluşturur. Araç şema yapımında kullanılır. Tanımı kaydettiğinizde, boş bir dizin Azure Bilişsel Arama'da tam olarak ifade edilir. Aranabilir içerikle nasıl yüklediğiniz size kalmış.

Dizin tasarımcısı, dizin oluşturmak için yalnızca bir yaklaşımdır. Alternatif olarak, [Alma veri sibunu](search-get-started-portal.md)kullanarak bir dizini oluşturabilir ve yükleyebilirsiniz. Sihirbaz yalnızca kendi oluşturduğu dizinlerle çalışır. Programlı olarak [,.NET](search-create-index-dotnet.md) veya [REST](search-create-index-rest-api.md) API'lerini kullanarak bir dizin oluşturabilirsiniz.

## <a name="start-index-designer"></a>Başlangıç dizin tasarımcısı

1. [Azure portalında](https://portal.azure.com) oturum açın ve hizmet panosunu açın. Atlama çubuğundaki **Tüm hizmetler**’e tıklayarak geçerli abonelikteki mevcut "arama hizmetleri" için arama yapabilirsiniz. 

2. Sayfanın üst kısmındaki komut çubuğundaki **dizin ekle** bağlantısını tıklatın.

   ![Komut çubuğuna dizin bağlantısı ekleme](media/search-create-index-portal/add-index.png "Komut çubuğuna dizin bağlantısı ekleme")

3. Azure Bilişsel Arama dizininizi adlandırın. Dizin adları dizin oluşturma ve sorgu işlemlerinde başvurur. Dizin adı, dizin bağlantılarında ve Azure Bilişsel Arama REST API'sinde HTTP isteklerigöndermek için kullanılan uç nokta URL'sinin bir parçası olur.

   * En başta bir harf kullanın.
   * Yalnızca küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın.
   * Adı 60 karakterle sınırlayın.

## <a name="add-fields"></a>Alan ekleme

Dizin oluşturma, dizininizdeki aranabilir verileri tanımlayan bir *Alanlar koleksiyonunu* içerir. Alanlar koleksiyonu, ayrı olarak yüklediğiniz belgelerin yapısını belirtir. Alanlar koleksiyonu, alanın nasıl kullanılabileceğini belirleyen dizin öznitelikleriyle adlandırılmış ve yazılan gerekli ve isteğe bağlı alanları içerir.

1. Yükleyeceğiniz belgeleri tam olarak belirtmek için alanlar ekleyin ve her biri için bir [veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) belirleyin. Örneğin, belgeler *bir otel kimliği,* *otel adı,* *adres,* *şehir*ve *bölgeden*oluşuyorsa, dizindeki her biri için karşılık gelen bir alan oluşturur. Öznitelikleri ayarlama konusunda yardım için [aşağıdaki bölümdeki tasarım kılavuzunu](#design) gözden geçirin.

1. Gelen veriler doğada hiyerarşikseyse, şemaiçiç iç içe yapıları temsil etmek için [karmaşık türleri](search-howto-complex-data-types.md) içermelidir. Yerleşik örnek veri kümesi Oteller, her otelle bire bir ilişkisi olan bir Adres (birden çok alt alan içerir) ve her otelle birden fazla odanın ilişkili olduğu Odalar karmaşık koleksiyonu kullanarak karmaşık türleri gösterir. 

1. Edm.String türünün *anahtar* alanını belirtin. Her Azure Bilişsel Arama dizini için anahtar alan zorunludur ve bir dize olmalıdır. Bu alan için değerler benzersiz her belge tanımlamak gerekir. Varsayılan olarak alan, *id* olarak adlandırılır, ancak [adlandırma kurallarına](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) uygun şekilde alanı yeniden adlandırabilirsiniz. Örneğin, alanlar koleksiyonunuz *otel kimliği*içeriyorsa, bunu anahtarınız için seçersiniz. 

1. Her alandaki öznitelikleri ayarlayın. Dizin tasarımcısı, veri türü için geçersiz olan öznitelikleri hariç tutar, ancak ne içermesi gerektiğini önermez. Özniteliklerin ne için olduğunu anlamak için sonraki bölümdeki kılavuzu gözden geçirin.

    Azure Bilişsel Arama API belgeleri, basit bir *otel* dizinine sahip kod örnekleri içerir. Aşağıdaki ekran görüntüsünde, dizin tanımı sırasında belirtilen Fransızca dil çözümleyicisi de dahil olmak üzere, portalda uygulama alıştırması olarak yeniden oluşturabileceğiniz dizin tanımını görebilirsiniz.

    ![Oteller demo endeksi](media/search-create-index-portal/field-definitions.png "Oteller demo endeksi")

1. Tamamlandığında, dizin kaydetmek ve oluşturmak için **Oluştur'u** tıklatın.

<a name="design"></a>

## <a name="set-attributes"></a>Öznitelikleri ayarlama

İstediğiniz zaman yeni alanlar ekleyebilseniz de, dizinin ömrü boyunca mevcut alan tanımları kilitlenir. Bu nedenle geliştiriciler genellikle basit dizinler oluşturmak, fikirleri test etmek veya portal sayfalarını kullanarak bir ayarı bulmak için portalı kullanır. Dizini kolayca yeniden derleyebilmeniz için kod tabanlı bir yaklaşım izlerseniz, bir dizin tasarımı çerçevesinde sık sık yapılan yineleme daha verimli olur.

Dizin kaydedilmeden önce çözümleyiciler ve öneri araçları alanlarla ilişkilendirilir. Oluştururken dizin tanımınıza dil çözümleyicileri veya öneriyiler eklediğinizden emin olun.

Dize alanları genellikle **Aranabilir** ve **Alınabilir** olarak işaretlenir. Arama sonuçlarını daraltmak için kullanılan alanlar arasında **Sıralanabilir**, **Filtrelenebilir** ve **Modellenebilir** alanları yer alır.

Alan öznitelikleri, bir alanın nasıl kullanıldığını; örneğin, tam metin alanında mı, çok yönlü gezinmede mi, sıralama işlemlerinde vb.’de mi kullanılıp kullanılmayacağı belirler. Aşağıdaki tabloda her bir öznitelik açıklanmaktadır.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|**Aranabilir**|Tam metin aranabilir, dizin oluşturma sırasında sözcüklere bölme gibi sözcük temelli analize tabidir. Aranabilir bir alanı, "güneşli gün" gibi bir değere ayarlarsanız, dahili olarak bu "güneşli" ve "gün" belirteçlerine bölünür. Ayrıntılar için bkz. [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)|  
|**Filterable**|**$filter** sorgularında başvurulur. `Edm.String` veya `Collection(Edm.String)` türünde filtrelenebilir alanlara sözcüklere bölme işlemi uygulanmaz, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler içindir. Örneğin, böyle bir alanı "güneşli gün" olarak ayarlarsanız `$filter=f eq 'sunny'` herhangi bir eşleşme bulmaz, ancak `$filter=f eq 'sunny day'` bulur. |  
|**Sıralanabilir**|Varsayılan olarak sistem sonuçları puana göre sıralar, ancak belgelerdeki alanlara göre sıralamayı yapılandırabilirsiniz. `Collection(Edm.String)` türünde alanlar **sıralanabilir** olamaz. |  
|**facetable**|Genellikle kategoriye göre (örneğin, belirli bir şehirdeki oteller) isabet sayısını içeren bir arama sonuçları sunumunda kullanılır. Bu seçenek, `Edm.GeographyPoint` türünde alanlarla kullanılamaz. **Filtrelenebilir**, **sıralanabilir** veya **modellenebilir** olan `Edm.String` türünde alanlar en fazla 32 kilobayt uzunluğunda olabilir. Ayrıntılar için bkz. [Dizin Oluşturma (REST API’si)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**anahtar**|Dizin içindeki belgeler için benzersiz tanımlayıcı. Anahtar alan olarak tam olarak bir alan seçilmeli ve `Edm.String` türünde olmalıdır.|  
|**geri alınabilir**|Alanın bir arama sonucunda döndürülüp döndürülemeyeceğini belirler. Filtre, sıralama veya puanlama mekanizması olarak bir alanı (örn. *kâr marjı*) kullanmak istediğinizde, ancak alanın son kullanıcıya görünür olmasını istemediğinizde bu faydalıdır. Bu öznitelik, `key` alanları için `true` olmalıdır.|  

## <a name="next-steps"></a>Sonraki adımlar

Azure Bilişsel Arama dizini oluşturduktan sonra bir sonraki adıma geçebilirsiniz: [aranabilir verileri dizine yükleyin.](search-what-is-data-import.md)

Alternatif olarak, [dizinlere daha derin denebilir.](search-what-is-an-index.md) Alanlar koleksiyonuna ek olarak bir dizin, çözümleyicileri, öneri araçlarını, puanlama profillerini ve CORS ayarlarını da belirtir. Portal, en yaygın öğeleri tanımlamak için sekmeli sayfalar sağlar: Alanlar, çözümleyiciler ve öneri araçları. Başka öğeler oluşturmak veya değiştirmek için REST API’sini veya .NET SDK’sını kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

 [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)  
 [Arama hizmeti REST API’si](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK’sı](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

