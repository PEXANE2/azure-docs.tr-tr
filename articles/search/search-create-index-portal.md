---
title: Azure portal-Azure Search Azure Search dizin oluşturma
description: Yerleşik bir portal Dizin Tasarımcısı kullanarak Azure Search için dizin oluşturmayı öğrenin.
manager: nitinme
author: heidisteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: heidist
ms.openlocfilehash: 4abef5a3030643d4c7b91d2911f350190972f1eb
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937274"
---
# <a name="create-an-azure-search-index-in-the-portal"></a>Portalda Azure Search dizin oluşturma

Azure Search, Portal 'da Prototiplerde faydalı olan yerleşik bir dizin tasarımcısını veya Azure Search hizmetinizde barındırılan bir [arama dizini](search-what-is-an-index.md) oluşturmayı içerir. Araç, şema oluşturma için kullanılır. Tanımı kaydettiğinizde boş bir dizin Azure Search ' de tam olarak ifade edilir. Aranabilir içerik ile nasıl yüklenir?

Dizin Tasarımcısı dizin oluşturmak için yalnızca bir yaklaşımdır. Alternatif olarak, [verileri Içeri aktarma sihirbazını](search-get-started-portal.md)kullanarak bir dizin oluşturup yükleyebilirsiniz. Sihirbaz yalnızca kendi oluşturduğu dizinlerde çalışmaktadır. Programlama yoluyla, [.net](search-create-index-dotnet.md) veya [rest](search-create-index-rest-api.md) API 'lerini kullanarak bir dizin oluşturabilirsiniz.

## <a name="start-index-designer"></a>Dizin tasarımcısını Başlat

1. [Azure Portal](https://portal.azure.com) oturum açın ve hizmet panosunu açın. Geçerli abonelikte mevcut "arama hizmetleri" ni aramak için, atlama çubuğundaki **tüm hizmetler** ' e tıklayabilirsiniz. 

2. Sayfanın en üstündeki komut çubuğunda **Dizin Ekle** bağlantısına tıklayın.

   Komut çubuğunda Dizin(media/search-create-index-portal/add-index.png "Ekle") ![bağlantı ekle]

3. Azure Search dizininizi adlandırın. Dizin adlarına dizin oluşturma ve sorgu işlemlerinde başvurulur. Dizin adı, Dizin bağlantılarında kullanılan uç nokta URL 'sinin bir parçası olur ve Azure Search REST API HTTP istekleri gönderir.

   * Bir harfle başlayın.
   * Yalnızca küçük harfleri, rakamları veya kısa çizgileri ("-") kullanın.
   * Adı 60 karakterle sınırlandırın.

## <a name="add-fields"></a>Alan Ekle

Dizin oluşturma, dizininizdeki aranabilir verileri tanımlayan bir *alanlar koleksiyonu* içerir. Tamamen, alanlar koleksiyonu ayrı olarak karşıya yüklediğiniz belgelerin yapısını belirler. Alanlar koleksiyonu, alanın nasıl kullanılabileceğini belirleyecek dizin öznitelikleri ile adlandırılmış ve yazılan isteğe bağlı alanları içerir.

1. Karşıya yükleyeceğiniz belgeleri tam olarak belirtmek için alanlar ekleyin, her biri için bir [veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) belirleyin. Örneğin, belgeler bir *otel kimliği*, *otel adı*, *Adres*, *şehir*ve *bölge*içeriyorsa, dizindeki her biri için karşılık gelen bir alan oluşturun. Öznitelikleri ayarlamayla ilgili yardım için [aşağıdaki bölümde yer alarak bulunan tasarım kılavuzunu](#design) gözden geçirin.

1. Gelen veriler doğası halinde hiyerarşik ise, şemanız iç içe yapıları temsil etmek için [karmaşık türler](search-howto-complex-data-types.md) içermelidir. Yerleşik örnek veri kümesi olan oteller, her bir otelle bire bir ilişkiye sahip olan ve her bir otel ile birden çok oda ilişkilendirildiği bir oda karmaşık koleksiyonu olan bir adres (birden çok alt alan içerir) kullanan karmaşık türleri gösterir. 

1. EDM. String türünde bir *anahtar* alanı belirtin. Her Azure Search dizin için bir anahtar alanı zorunludur ve bir dize olmalıdır. Bu alanın değerleri her belgeyi benzersiz şekilde tanımlamalıdır. Varsayılan olarak, alan *id* olarak adlandırılır ancak dize [adlandırma kurallarını](https://docs.microsoft.com/rest/api/searchservice/Naming-rules)karşıladığından, yeniden adlandırabilirsiniz. Örneğin, alanlar koleksiyonunuz *otel kimliği*içeriyorsa, anahtarınız için bunu seçersiniz. 

1. Her bir alanda öznitelikleri ayarlayın. Dizin Tasarımcısı, veri türü için geçersiz olan tüm öznitelikleri dışlar, ancak nelerin ekleneceğini tavsiye etmez. Özniteliklerin ne için olduğunu anlamak için sonraki bölümde yer alarak bulunan Kılavuzu gözden geçirin.

    Azure Search API belgeleri, basit bir *oteller* dizini içeren kod örnekleri içerir. Aşağıdaki ekran görüntüsünde, Dizin tanımı sırasında belirtilen Fransızca dil Çözümleyicisi de dahil olmak üzere dizin tanımını görebilirsiniz. Bu, portalda alıştırma olarak yeniden oluşturabilirsiniz.

    ![Oteller tanıtım dizini](media/search-create-index-portal/field-definitions.png "oteller tanıtım dizini")

1. İşiniz bittiğinde, dizini kaydetmek ve oluşturmak için **Oluştur** ' a tıklayın.

<a name="design"></a>

## <a name="set-attributes"></a>Öznitelikleri ayarla

İstediğiniz zaman yeni alanlar ekleyebilseniz de, dizinin ömrü boyunca mevcut alan tanımları kilitlenir. Bu nedenle, geliştiriciler genellikle basit dizinler oluşturmak, fikirleri test etmek veya Portal sayfalarını kullanarak bir ayar aramak için portalı kullanır. Dizini kolayca yeniden oluşturabilmeniz için, bir dizin tasarımı üzerinde sık yapılan yineleme, kod tabanlı bir yaklaşım izlerseniz daha etkilidir.

Çözümleyiciler ve öneri araçları, Dizin kaydedilmeden önce alanlarla ilişkilendirilir. Siz oluştururken Dizin tanımınıza dil Çözümleyicileri veya öneri araçları eklediğinizden emin olun.

Dize alanları genellikle **aranabilir** ve **alınabilir**olarak işaretlenir. Arama sonuçlarını daraltmak için kullanılan alanlar, **sıralanabilir**, **filtrelenebilir**ve çok **yönlü tablo**içerir.

Alan öznitelikleri, bir alanın nasıl kullanıldığını (örneğin, tam metin aramasında, çok yönlü gezinme, sıralama işlemlerinde vb.) kullandığını belirtir. Aşağıdaki tabloda her bir öznitelik açıklanmaktadır.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|**aranamaz**|Tam metin aranabilir, dizin oluşturma sırasında sözcük bölünmesi gibi sözlü Analize tabidir. Aranabilir bir alanı "Sunny Day" gibi bir değere ayarlarsanız, dahili olarak "Sunny" ve "Day" belirteçlerine bölünecektir. Ayrıntılar için bkz. [tam metin aramasının nasıl çalıştığı](search-lucene-query-architecture.md).|  
|**filtrelenebilir**|**$Filter** sorgularında başvuruluyor. @No__t-0 veya `Collection(Edm.String)` türündeki filtrelenebilir alanlar sözcük kesimini içermez, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler için geçerlidir. Örneğin, böyle bir alanı f 'yi "Sunny günü" olarak ayarlarsanız, `$filter=f eq 'sunny'` hiçbir eşleşme bulmayacak, ancak `$filter=f eq 'sunny day'` olur. |  
|**amayan**|Varsayılan olarak, sistem sonuçları puana göre sıralar, ancak belgelerdeki alanları temel alan sıralamayı yapılandırabilirsiniz. @No__t-0 türündeki alanlar **sıralanabilir**olamaz. |  
|**modellenebilir**|Genellikle kategoriye göre (örneğin, belirli bir şehirdeki oteller) isabet sayısını içeren bir arama sonuçları sunumunda kullanılır. Bu seçenek `Edm.GeographyPoint` türündeki alanlarla kullanılamaz. **Filtrelenebilir**, **sıralanabilir**veya çok **yönlü tablo** olan `Edm.String` türündeki alanlar en fazla 32 kilobayt uzunluğunda olabilir. Ayrıntılar için bkz. [Dizin oluşturma (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**anahtar**|Dizin içindeki belgeler için benzersiz tanımlayıcı. Anahtar alanı olarak yalnızca bir alan seçilmelidir ve `Edm.String` türünde olmalıdır.|  
|**defterinden**|Alanın bir arama sonucu döndürülüp döndürülmeyeceğini belirler. Bu, bir alanı (örneğin, *kar marjı*) filtre, sıralama veya Puanlama mekanizması olarak kullanmak istediğinizde, ancak alanın son kullanıcıya görünür olmasını istemediğiniz durumlarda faydalıdır. Bu öznitelik, `key` alanları için `true` olmalıdır.|  

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure Search dizini oluşturduktan sonra bir sonraki adıma geçebilirsiniz: [aranabilir verileri dizine yükleyin](search-what-is-data-import.md).

Alternatif olarak, [dizinlere daha ayrıntılı bir bakış](search-what-is-an-index.md)da alabilirsiniz. Alanlar koleksiyonuna ek olarak, bir dizin de çözümleyiciler, öneri araçları, Puanlama profilleri ve CORS ayarlarını belirtir. Portal, en yaygın öğeleri tanımlamaya yönelik sekmeli sayfalar sağlar: alanlar, çözümleyiciler ve öneri araçları. Diğer öğeleri oluşturmak veya değiştirmek için REST API veya .NET SDK 'sını kullanabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

 [Tam metin aramasının çalışması](search-lucene-query-architecture.md)  
 [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet) [REST API arama hizmeti](https://docs.microsoft.com/rest/api/searchservice/)

