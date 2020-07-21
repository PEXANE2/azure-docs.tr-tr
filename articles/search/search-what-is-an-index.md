---
title: Arama dizini oluşturma
titleSuffix: Azure Cognitive Search
description: ", Şema tanımları ve fiziksel veri yapısı dahil olmak üzere Azure Bilişsel Arama 'de dizin oluşturma kavramlarını ve araçları sunar."
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.openlocfilehash: 9e8d1c012ae07fc458a324315e2635f04c3dbd78
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496544"
---
# <a name="create-a-basic-search-index-in-azure-cognitive-search"></a>Azure Bilişsel Arama temel arama dizini oluşturma

Azure Bilişsel Arama 'de, *arama dizini* tam metin ve filtrelenmiş sorgular için kullanılan aranabilir içeriği depolar. Bir dizin, bir şema tarafından tanımlanır ve ikinci bir adım olarak verileri içeri aktarma ile hizmete kaydedilir. 

Dizinler *belgeleri*içerir. Kavramsal olarak, bir belge, dizininizdeki aranabilir verilerin tek bir birimidir. Bir perakendeciden her ürün için bir belge, bir haber kuruluşunda her bir makale için bir belge olabilir ve bu şekilde devam eder. Bu kavramları daha tanıdık veritabanı eşdeğerlerine eşleme: bir *arama dizini* bir *tabloya*karşılık gelir ve *Belgeler* bir tablodaki *satırlara* kabaca eşdeğerdir.

Bir dizinin fiziksel yapısı, şema tarafından belirlenir ve "aranabilir" olarak işaretlenen alanlar, bu alan için oluşturulan ters bir dizin ile sonuçlanır. 

Aşağıdaki araçlar ve API 'lerle bir dizin oluşturabilirsiniz:

* Azure portal, **dizin ekleme** veya **veri alma** Sihirbazı 'nı kullanma
* [Create INDEX (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) kullanma
* [.NET SDK 'yı](search-create-index-dotnet.md) kullanma

Portal aracıyla daha kolay bir şekilde öğrenirsiniz. Portal, sayısal alanlarda tam metin arama özelliklerine izin vermeme gibi belirli veri türleri için gereksinimleri ve şema kurallarını uygular. Bir dizin oluşturma işleminden sonra, [Dizin Al (REST API)](https://docs.microsoft.com/rest/api/searchservice/get-index) kullanarak hizmetten JSON tanımını alarak ve çözümünüze ekleyerek koda geçiş yapabilirsiniz.

## <a name="recommended-workflow"></a>Önerilen iş akışı

Son dizin tasarımına ulaşan yinelemeli bir işlemdir. İlk dizini oluşturmak için Portal ile başlamak ve sonra dizine kaynak denetimi altına yerleştirmek için koda geçmek yaygındır.

1. [**Içeri aktarma verilerini**](search-import-data-portal.md)kullanıp kullanmayacağınızı saptayın. Sihirbaz, kaynak veriler [Azure 'daki desteklenen bir veri kaynağı türünden](search-indexer-overview.md#supported-data-sources)ise, hepsi bir arada Dizin Oluşturucu tabanlı dizin oluşturma işlemi gerçekleştirir.

1. **Verileri Içeri aktar**' ı kullanmanızın ardından şemayı tanımlamak Için **Dizin Ekle** ile başlayın.

   ![Dizin komutu Ekle](media/search-what-is-an-index/add-index.png "Dizin komutu Ekle")

1. Dizindeki her bir arama belgesini benzersiz bir şekilde tanımlamak için kullanılan bir ad ve anahtar belirtin. Anahtar zorunludur ve Edm. String türünde olmalıdır. İçeri aktarma sırasında, kaynak verilerde bulunan benzersiz bir alanı bu alana eşlemeyi planlamanız gerekir. 

   Portal size `id` anahtar için bir alan sağlar. Varsayılanı geçersiz kılmak için `id` Yeni bir alan (örneğin, adlı yeni bir alan tanımı `HotelId` ) oluşturun ve sonra **anahtar**içinde seçin.

   ![Gerekli özellikleri doldur](media/search-what-is-an-index//field-attributes.png "Gerekli özellikleri doldur")

1. Daha fazla alan ekleyin. Portal, farklı veri türleri için hangi [alan özniteliklerinin](#index-attributes) kullanılabilir olduğunu gösterir. Dizin tasarımına yeni başladıysanız, bu yararlı olur.

   Gelen veriler doğası halinde hiyerarşik ise, iç içe yapıları temsil etmek için [karmaşık tür](search-howto-complex-data-types.md) veri türünü atayın. Yerleşik örnek veri kümesi olan oteller, her bir otelle bire bir ilişkiye sahip olan ve her bir otel ile birden çok oda ilişkilendirildiği bir oda karmaşık koleksiyonu olan bir adres (birden çok alt alan içerir) kullanan karmaşık türleri gösterir. 

1. Dizin oluşturulmadan önce dize alanlarına [çözümleyiciler](#analyzers) atayın. Belirli alanlarda otomatik tamamlamayı etkinleştirmek istiyorsanız, [Öneri araçları](#suggesters) için aynısını yapın.

1. Arama hizmetinizde fiziksel yapılar oluşturmak için **Oluştur** ' a tıklayın.

1. Bir dizin oluşturulduktan sonra, tanımları gözden geçirmek veya daha fazla öğe eklemek için ek komutlar kullanın.

   ![Öznitelikleri veri türüne göre gösteren Dizin sayfası ekle](media/search-what-is-an-index//field-definitions.png "Öznitelikleri veri türüne göre gösteren Dizin sayfası ekle")

1. [Get Index (REST API)](https://docs.microsoft.com/rest/api/searchservice/get-index) ve [Postman](search-get-started-postman.md)gibi bir Web testi aracı kullanarak Dizin şemasını indirin. Artık, kod için uyarlayabileceğiniz dizinin bir JSON gösterimine sahipsiniz.

1. [Dizininizi verilerle yükleyin](search-what-is-data-import.md). Azure Bilişsel Arama JSON belgelerini kabul eder. Verilerinizi programlı bir şekilde yüklemek için, istek yükünde JSON belgeleriyle Postman kullanabilirsiniz. Verileriniz JSON olarak kolayca ifade edideğilse, bu adım en yoğun işgücü olacaktır. 

    Bir dizin verilerle yüklendikten sonra, mevcut alanlara yapılan çoğu düzenleme bir dizini bırakıp yeniden oluşturmanızı gerektirir.

1. İstediğiniz sonuçları görene kadar dizininizin dizinini sorgulayın, sonuçları inceleyin ve dizin şemasında daha fazla yineleme yapın. Dizininizi sorgulamak için [**Arama Gezgini**](search-explorer.md) 'Ni veya Postman 'ı kullanabilirsiniz.

Geliştirme sırasında, sık sık yeniden deryi planlayın. Hizmette fiziksel yapılar oluşturulduğundan, var olan bir alan tanımında çoğu değişiklik için [dizinleri bırakma ve](search-howto-reindex.md) yeniden oluşturma gereklidir. Yeniden oluşturma işlemini daha hızlı hale getirmek için verilerinizin bir alt kümesiyle çalışmayı düşünebilirsiniz. 

> [!Tip]
> Portal yaklaşımı yerine kod, dizin tasarımı ve veri içeri aktarma ile aynı anda çalışmak için önerilir. Alternatif olarak, [Postman ve REST API](search-get-started-postman.md) gibi araçlar, geliştirme projeleri hala erken aşamalarındayken kavram kanıtı testi için yararlıdır. Bir istek gövdesinde Dizin tanımında artımlı değişiklikler yapabilir ve sonra güncelleştirilmiş bir şemayı kullanarak bir dizini yeniden oluşturmak için isteği hizmetinize gönderebilirsiniz.

## <a name="index-schema"></a>Dizin şeması

Alanlar koleksiyonunda bir ad ve belirlenmiş anahtar alanı (EDM. String) olması için bir dizin gereklidir. [*Alanlar koleksiyonu*](#fields-collection) genellikle bir dizinin en büyük bölümüdür; burada her bir alan adlandırılmış, yazılır ve nasıl kullanılacağını belirleyecek izin verilen davranışlar ile adlandırılır. 

Diğer öğeler, dil kuralları ve çözümleyici tarafından desteklenen diğer özelliklere göre belirteçlerde dizeleri işlemek için kullanılan [Öneri araçları](#suggesters), [Puanlama profilleri](#scoringprofiles), [çözümleyiciler](#analyzers) ve [çapraz kaynak uzaktan betik (CORS)](#corsoptions) ayarları içerir.

```json
{
  "name": (optional on PUT; required on POST) "name_of_index",
  "fields": [
    {
      "name": "name_of_field",
      "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
      "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
      "filterable": true (default) | false,
      "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
      "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
      "key": true | false (default, only Edm.String fields can be keys),
      "retrievable": true (default) | false,
      "analyzer": "name_of_analyzer_for_search_and_indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
      "searchAnalyzer": "name_of_search_analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
      "indexAnalyzer": "name_of_indexing_analyzer", (only if 'searchAnalyzer' is set and 'analyzer' is not set)
      "synonymMaps": [ "name_of_synonym_map" ] (optional, only one synonym map per field is currently supported)
    }
  ],
  "suggesters": [
    {
      "name": "name of suggester",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["field1", "field2", ...]
    }
  ],
  "scoringProfiles": [
    {
      "name": "name of scoring profile",
      "text": (optional, only applies to searchable fields) {
        "weights": {
          "searchable_field_name": relative_weight_value (positive #'s),
          ...
        }
      },
      "functions": (optional) [
        {
          "type": "magnitude | freshness | distance | tag",
          "boost": # (positive number used as multiplier for raw score != 1),
          "fieldName": "...",
          "interpolation": "constant | linear (default) | quadratic | logarithmic",
          "magnitude": {
            "boostingRangeStart": #,
            "boostingRangeEnd": #,
            "constantBoostBeyondRange": true | false (default)
          },
          "freshness": {
            "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
          },
          "distance": {
            "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
            "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
          },
          "tag": {
            "tagsParameter": "..." (parameter to be passed in queries to specify a list of tags to compare against target fields)
          }
        }
      ],
      "functionAggregation": (optional, applies only when functions are specified) 
        "sum (default) | average | minimum | maximum | firstMatching"
    }
  ],
  "analyzers":(optional)[ ... ],
  "charFilters":(optional)[ ... ],
  "tokenizers":(optional)[ ... ],
  "tokenFilters":(optional)[ ... ],
  "defaultScoringProfile": (optional) "...",
  "corsOptions": (optional) {
    "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
    "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
  },
  "encryptionKey":(optional){
    "keyVaultUri": "azure_key_vault_uri",
    "keyVaultKeyName": "name_of_azure_key_vault_key",
    "keyVaultKeyVersion": "version_of_azure_key_vault_key",
    "accessCredentials":(optional){
      "applicationId": "azure_active_directory_application_id",
      "applicationSecret": "azure_active_directory_application_authentication_key"
    }
  }
}
```

<a name="fields-collection"></a>

## <a name="fields-collection-and-field-attributes"></a>Alanlar koleksiyonu ve alan öznitelikleri

Alanlar bir ada, depolanan verileri sınıflandıran bir türe ve alanın nasıl kullanıldığını belirten özniteliklere sahiptir.

### <a name="data-types"></a>Veri türleri

| Tür | Description |
|------|-------------|
| Edm.String |İsteğe bağlı olarak tam metin araması için simgeleştirilmiş olabilecek metin (sözcük ayırma, sözcük kökü oluşturma vb.). |
| Collection(Edm.String) |Tam metin araması için isteğe bağlı olarak belirteç haline getirilebilen dize listesi. Bir koleksiyondaki öğelerin sayısında teorik bir üst sınır yoktur ancak yük boyutundaki 16 MB'lık üst sınır, koleksiyonlar için geçerlidir. |
| Edm.Boolean |True/false değerlerini içerir. |
| Edm.Int32 |32 bit tamsayı değerleri. |
| Edm.Int64 |64 bit tamsayı değerleri. |
| Edm.Double |Çift duyarlıklı sayısal veriler. |
| Edm.DateTimeOffset |OData v4 biçiminde temsil edilen tarih saat değerleri (örneğin, `yyyy-MM-ddTHH:mm:ss.fffZ` veya `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm` ). |
| Edm.GeographyPoint |Dünya üzerindeki bir coğrafi konumu temsil eden bir nokta. |

Daha fazla bilgi için bkz. [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

<a name="index-attributes"></a>

### <a name="attributes"></a>Öznitelikler

Alan öznitelikleri, bir alanın nasıl kullanıldığını; örneğin, tam metin alanında mı, çok yönlü gezinmede mi, sıralama işlemlerinde vb.’de mi kullanılıp kullanılmayacağı belirler. 

Dize alanları genellikle "aranabilir" ve "alınabilir" olarak işaretlenir. Arama sonuçlarını daraltmak için kullanılan alanlar arasında "sıralanabilir", "filtrelenebilir" ve "çok yönlü tablo" bulunur.

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|aranamaz |Tam metin aranabilir, dizin oluşturma sırasında sözcüklere bölme gibi sözcük temelli analize tabidir. Aranabilir bir alanı, "güneşli gün" gibi bir değere ayarlarsanız, dahili olarak bu "güneşli" ve "gün" belirteçlerine bölünür. Ayrıntılar için bkz. [Tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)|  
|filtrelenebilir |$filter sorgularında başvurulur. `Edm.String` veya `Collection(Edm.String)` türünde filtrelenebilir alanlara sözcüklere bölme işlemi uygulanmaz, bu nedenle karşılaştırmalar yalnızca tam eşleşmeler içindir. Örneğin, böyle bir alanı "güneşli gün" olarak ayarlarsanız `$filter=f eq 'sunny'` herhangi bir eşleşme bulmaz, ancak `$filter=f eq 'sunny day'` bulur. |  
|amayan |Varsayılan olarak sistem sonuçları puana göre sıralar, ancak belgelerdeki alanlara göre sıralamayı yapılandırabilirsiniz. Türündeki alanlar `Collection(Edm.String)` "sıralanabilir" olamaz. |  
|modellenebilir |Genellikle kategoriye göre (örneğin, belirli bir şehirdeki oteller) isabet sayısını içeren bir arama sonuçları sunumunda kullanılır. Bu seçenek, `Edm.GeographyPoint` türünde alanlarla kullanılamaz. `Edm.String`Filtrelenebilir, "sıralanabilir" veya "çok yönlü tablo" türündeki alanlar en fazla 32 kilobayt uzunluğunda olabilir. Ayrıntılar için bkz. [Dizin Oluşturma (REST API’si)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|anahtar |Dizin içindeki belgeler için benzersiz tanımlayıcı. Anahtar alan olarak tam olarak bir alan seçilmeli ve `Edm.String` türünde olmalıdır.|  
|defterinden |Alanın bir arama sonucunda döndürülüp döndürülemeyeceğini belirler. Filtre, sıralama veya puanlama mekanizması olarak bir alanı (örn. *kâr marjı*) kullanmak istediğinizde, ancak alanın son kullanıcıya görünür olmasını istemediğinizde bu faydalıdır. Bu öznitelik, `key` alanları için `true` olmalıdır.|  

İstediğiniz zaman yeni alanlar ekleyebilseniz de, dizinin ömrü boyunca mevcut alan tanımları kilitlenir. Bu nedenle geliştiriciler genellikle basit dizinler oluşturmak, fikirleri test etmek veya portal sayfalarını kullanarak bir ayarı bulmak için portalı kullanır. Dizini kolayca yeniden derleyebilmeniz için kod tabanlı bir yaklaşım izlerseniz, bir dizin tasarımı çerçevesinde sık sık yapılan yineleme daha verimli olur.

> [!NOTE]
> Bir dizin oluşturmak için kullandığınız API 'Ler, farklı varsayılan davranışlara sahiptir. [REST API 'leri](https://docs.microsoft.com/rest/api/searchservice/Create-Index)için çoğu öznitelik varsayılan olarak etkindir (örneğin, "aranabilir" ve "alınabilir" dize alanları için geçerlidir) ve genellikle bunları kapatmak istiyorsanız ayarlamanız gerekir. .NET SDK için, tersi doğrudur. Açıkça ayarlamadığınız herhangi bir özellikte, özel olarak etkinleştirmediğiniz takdirde, karşılık gelen arama davranışının devre dışı bırakılması varsayılan olur.

## `analyzers`

Çözümleyiciler öğesi, alan için kullanılacak dil Çözümleyicisi adını ayarlar. Kullanabileceğiniz çözümleyiciler aralığı hakkında daha fazla bilgi için bkz. [Azure bilişsel arama dizinine çözümleyiciler ekleme](search-analyzers.md). Çözümleyiciler yalnızca aranabilir alanlarla kullanılabilir. Çözümleyici bir alana atandıktan sonra, dizini yeniden oluşturmadığınız takdirde değiştirilemez.

## `suggesters`

Bir öneri aracı, aramalardaki otomatik tamamlamayı veya tür ön sorguları desteklemek için bir dizindeki hangi alanların kullanıldığını tanımlayan şemanın bir bölümüdür. Genellikle kısmi arama dizeleri, Kullanıcı bir arama sorgusu yazarken [(REST API) önerilere](https://docs.microsoft.com/rest/api/searchservice/suggestions) GÖNDERILIR ve API, önerilen bir belge veya tümcecik kümesi döndürür. 

Bir öneri aracı eklenen alanlar, tür öncelikli arama terimleri oluşturmak için kullanılır. Tüm arama terimleri dizin oluşturma sırasında oluşturulur ve ayrı olarak depolanır. Öneri aracı yapısı oluşturma hakkında daha fazla bilgi için bkz. [Add öneri araçları](index-add-suggesters.md).

## `corsOptions`

Tarayıcı tüm çapraz kaynak isteklerini önleyediğinden, istemci tarafı JavaScript varsayılan olarak herhangi bir API 'yi çağıramaz. Dizininizdeki çapraz kaynak sorgularına izin vermek için **Corsoptions** ÖZNITELIĞINI ayarlayarak CORS 'yi etkinleştirin (çıkış noktaları arası kaynak paylaşımı). Güvenlik nedenleriyle, yalnızca sorgu API 'Leri CORS 'yi destekler. 

CORS için aşağıdaki seçenekler ayarlanabilir:

+ **allowederlerin** (gerekli): Bu, dizininiz için erişim verilecek olan kaynakları içeren bir listesidir. Bu, bu kaynaklardan sunulan tüm JavaScript kodunun dizininizi sorgulayabileceği anlamına gelir (doğru api anahtarını sağladığını varsayarak). Her kaynak genellikle form olur, `protocol://<fully-qualified-domain-name>:<port>` ancak `<port>` genellikle atlanacaktır. Daha fazla ayrıntı için bkz. [çıkış noktaları arası kaynak paylaşımı (Vikipedi)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) .

  Tüm kaynaklardan erişime izin vermek istiyorsanız, `*` **allowedkaynakları** dizisine tek bir öğe olarak dahil edin. *Bu, üretim arama hizmetleri için önerilen bir uygulamadır,* ancak geliştirme ve hata ayıklama için genellikle yararlıdır.

+ **Maxageınseconds** (isteğe bağlı): TARAYıCıLAR, CORS ön denetim yanıtlarını önbelleğe almak için süreyi (saniye cinsinden) belirlemede bu değeri kullanır. Bu negatif olmayan bir tamsayı olmalıdır. Bu değer arttıkça, daha iyi performans olacaktır, ancak CORS ilke değişikliklerinin etkili olması için daha uzun sürer. Ayarlanmamışsa, varsayılan 5 dakikalık bir süre kullanılacaktır.

## `scoringProfiles`

[Puanlama profili](index-add-scoring-profiles.md) , arama sonuçlarında hangi öğelerin göründüğünü etkileyen özel Puanlama davranışlarını tanımlayan şemanın bir bölümüdür. Puanlama profilleri alan ağırlıklarının ve işlevlerden oluşur. Bunları kullanmak için sorgu dizesinde ada göre bir profil belirtirsiniz.

Varsayılan bir Puanlama profili, bir sonuç kümesindeki her öğe için bir arama puanı hesaplamak üzere arka planda çalışır. Dahili, adlandırılmamış Puanlama profilini kullanabilirsiniz. Alternatif olarak, sorgu dizesinde özel bir profil belirtilmediğinde çağrılan varsayılan olarak özel bir profil kullanmak için **defaultScoringProfile** ayarlayın.

<a name="index-size"></a>

## <a name="attributes-and-index-size-storage-implications"></a>Öznitelikler ve dizin boyutu (depolama etkileri)

Bir dizinin boyutu, karşıya yüklediğiniz belgelerin boyutuna göre belirlenir ve öneri araçları dahil edilip edilmeyeceğini ve ayrı alanlarda öznitelikleri nasıl ayarlayacağınıza ilişkin Dizin yapılandırması vardır. 

Aşağıdaki ekran görüntüsünde, çeşitli öznitelik birleşimlerinden kaynaklanan dizin depolama desenleri gösterilmektedir. Dizin, verileri Içeri aktarma Sihirbazı 'nı kullanarak kolayca oluşturabileceğiniz **gerçek emlak örnek dizinine**dayalıdır. Dizin şemaları gösterilmese de, dizin adına göre öznitelikleri çıkarsyükleyebilirsiniz. Örneğin, *reatastate ile aranabilir* Dizin "aranabilir" özniteliğine sahiptir ve başka hiçbir şey yoktur, *reatastate-alınabilir* Dizin, "alınabilir" özniteliği seçilir ve başka hiçbir şey yapmaz.

![Öznitelik seçimine dayalı Dizin boyutu](./media/search-what-is-an-index/realestate-index-size.png "Öznitelik seçimine dayalı Dizin boyutu")

Bu dizin çeşitleri yapay olsa da, özniteliklerin depolamayı nasıl etkilediği hakkında geniş karşılaştırmalar için bunlara başvurabiliyoruz. "Alınabilir" olarak ayarlandığında Dizin boyutu artar mi? Hayır. **Öneri aracı** bir dizin boyutunu artırmak için alan ekleme Evet.

Filtre ve sıralamayı destekleyen dizinler, yalnızca tam metin aramasını destekleyen dizinlerden nispeten daha büyüktür. Bunun nedeni, filtre ve sıralama işlemlerinin tam eşleşmeler için taranması, tam metin dizelerinin varlığını gerektirmektir. Buna karşılık, tam metin sorgularını destekleyen aranabilir alanlar, tüm belgelerden daha az alan tüketen simgeleştirilmiş koşullarla doldurulmuş ters dizinler kullanır. 

> [!Note]
> Depolama mimarisi Azure Bilişsel Arama uygulama ayrıntısı olarak değerlendirilir ve bildirimde bulunulmadan değiştirilebilir. Geçerli davranışın gelecekte devam edeceğini garanti vermez.

## <a name="next-steps"></a>Sonraki adımlar

Dizin kompozisyonunun anlaşılmasından sonra, ilk dizininizi oluşturmak için portalda devam edebilirsiniz. *Reatastate-US-Sample* veya *oteller-Sample* barındırılan veri kaynakları ' nı seçerek **veri alma** Sihirbazı ' nı kullanmaya başlamasını öneririz.

> [!div class="nextstepaction"]
> [Veri alma Sihirbazı (portal)](search-get-started-portal.md)

Her iki veri kümesi için de sihirbaz bir dizin şemasını çıkarabilir, verileri içeri aktarabilir ve arama Gezgini 'ni kullanarak sorgulayabilmeniz için aranabilir bir dizinin çıktısını alabilir. Veri **alma** Sihirbazı ' nın **verilerinize bağlanın** sayfasında bu veri kaynaklarını bulun.

   ![Örnek dizin oluşturma](media/search-what-is-an-index//import-wizard-sample-data.png "Örnek dizin oluşturma")