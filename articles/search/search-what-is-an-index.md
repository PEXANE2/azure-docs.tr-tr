---
title: Dizin tanımı ve kavramları oluşturma
titleSuffix: Azure Cognitive Search
description: Bileşen parçaları ve fiziksel yapı da dahil olmak üzere Azure Bilişsel Arama'da dizin terimlerine ve kavramlarına giriş.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: d2b8b2fecbf85e6590294f1fbd7ff2a4453b9e87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282789"
---
# <a name="create-a-basic-index-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da temel bir dizin oluşturma

Azure Bilişsel Arama'da *dizin,* Azure Bilişsel Arama hizmetinde filtrelenmiş ve tam metin araması için kullanılan kalıcı bir *belge* ve diğer yapıların deposudur. Kavramsal olarak, belge dizininizdeki aranabilir verilerin tek bir birimidir. Örneğin, bir e-ticaret satıcısında sattığı her bir öğe için bir belge, bir haber kuruluşunda her bir makale için bir belge, vb. olabilir. Bu kavramları daha çok bilinen veritabanı eşdeğerlerine eşleyen bir *dizin*, kavramsal olarak bir *tabloya* benzer ve *belgeler* de bir tablodaki *satırlarla* kabaca eşdeğerdir.

Bir dizin eklediğinizde veya yüklediğinizde, Azure Bilişsel Arama sağladığınız şemaya göre fiziksel yapılar oluşturur. Örneğin, dizininizdeki bir alan aranabilir olarak işaretlenmişse, bu alan için ters bir dizin oluşturulur. Daha sonra, belge eklediğinizde veya yüklediğinizde veya Arama sorgularını Azure Bilişsel Arama'ya gönderdiğinizde, arama hizmetinizdeki belirli bir dizine istek gönderirsiniz. Belge değerleriyle yükleme alanlarına *dizin oluşturma* veya veri alma denir.

Portalda, [REST API'de](search-create-index-rest-api.md)veya [.NET SDK'da](search-create-index-dotnet.md)bir dizin oluşturabilirsiniz.

## <a name="recommended-workflow"></a>Önerilen iş akışı

Doğru dizin tasarımına ulaşmak genellikle birden çok yineleme ile elde edilir. Araçlar ve API'lerin bir birleşimini kullanmak, tasarımınızı hızla tamamlamanıza yardımcı olabilir.

1. [Dizin leyici](search-indexer-overview.md#supported-data-sources)kullanıp kullanamayacağınızı belirleyin. Dış verileriniz desteklenen veri kaynaklarından biriyse, [**İçe Aktar veri**](search-import-data-portal.md) sihirbazını kullanarak bir dizini prototipleyebilir ve yükleyebilirsiniz.

2. **İçe Aktarma verilerini**kullanamıyorsanız, **Dizin Ekle** sayfasındaki denetimleri kullanarak alan, veri türleri ekleme ve öznitelikleri atama özelliği ni portalda yine de bir başlangıç [dizini oluşturabilirsiniz.](search-create-index-portal.md) Portal, farklı veri türleri için hangi özniteliklerin kullanılabildiğinizi gösterir. Dizin tasarımında yeniyseniz, bu yararlıdır.

   ![Veri türüne göre öznitelikleri gösteren dizin sayfası ekleme](media/search-create-index-portal/field-attributes.png "Veri türüne göre öznitelikleri gösteren dizin sayfası ekleme")
  
   **Oluştur'u**tıklattığınızda, dizininizi destekleyen tüm fiziksel yapılar arama hizmetinizde oluşturulur.

3. [Get Index REST API](https://docs.microsoft.com/rest/api/searchservice/get-index) ve [Postman](search-get-started-postman.md)gibi bir web test aracını kullanarak dizin şeasını indirin. Artık portalda oluşturduğunuz dizinin JSON temsiline sahipsiniz. 

   Bu noktada kod tabanlı bir yaklaşıma geçiyorsunuz. Zaten oluşturulmuş bir dizini oluşturamadığınız için portal yineleme için uygun değildir. Ama kalan görevler için Postacı ve REST kullanabilirsiniz.

4. [Dizininizi verilerle yükleyin.](search-what-is-data-import.md) Azure Bilişsel Arama JSON belgelerini kabul eder. Verilerinizi programlı olarak yüklemek için Postacı'yı istek yükünde JSON belgeleriyle birlikte kullanabilirsiniz. Verileriniz json olarak kolayca ifade edilmezse, bu adım en yoğun emek olacaktır.

5. Dizinisorgula, sonuçları inceleyin ve beklediğiniz sonuçları görmeye başlayana kadar dizin şemasıüzerinde daha fazla yineleyin. Dizininizi sorgulamak için [**Arama gezgini**](search-explorer.md) veya Postacı'yı kullanabilirsiniz.

6. Tasarımınızı tekrarlamak için kodu kullanmaya devam edin.  

Fiziksel yapılar hizmette oluşturulduğundan, varolan bir alan tanımında önemli değişiklikler yaptığınızda [dizinlerin düşmesi ve yeniden oluşturulması](search-howto-reindex.md) gerekir. Bu, geliştirme sırasında sık sık yeniden oluşturmayı planlamanız gerektiği anlamına gelir. Yeniden oluşturmaların daha hızlı gitmesini sağlamak için verilerinizin bir alt kümesiyle çalışmayı düşünebilirsiniz. 

Yinelemeli tasarım için bir portal yaklaşımı yerine kod önerilir. Dizin tanımı için portala güveniyorsanız, her yeniden oluşturma da dizin tanımını doldurmanız gerekir. Alternatif olarak, [Postacı ve REST API](search-get-started-postman.md) gibi araçlar, geliştirme projeleri hala erken safhalardayken kavram kanıtı testi için yararlıdır. İstek gövdesinde dizin tanımında artımlı değişiklikler yapabilir ve ardından güncelleştirilmiş bir şema kullanarak bir dizini yeniden oluşturmak için isteği hizmetinize gönderebilirsiniz.

## <a name="components-of-an-index"></a>Bir dizin bileşenleri

Skematik olarak, Azure Bilişsel Arama dizini aşağıdaki öğelerden oluşur. 

[*Alanlar koleksiyonu*](#fields-collection) genellikle, her alanın adlarının verildiği, yazıldığı ve nasıl kullanılacağını belirleyen izin verilen davranışlarla atfedilen bir dizinin en büyük bölümüdür. Diğer unsurlar [arasında önermeler,](#suggesters) [puanlama profilleri,](#scoring-profiles)özelleştirmeyi desteklemek için bileşen parçalarına sahip [analizörler,](#analyzers) [CORS](#cors) ve [şifreleme anahtar](#encryption-key) seçenekleri yer almaktadır.

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

## <a name="fields-collection-and-field-attributes"></a>Alanlar toplama ve alan öznitelikleri

Şemanızı tanımlarken, dizininizdeki her bir alan için ad, tür ve öznitelikler belirtmeniz gerekir. Alan türü, bu alanda depolanan verileri sınıflandırır. Öznitelikler, alanın nasıl kullanıldığını belirtmek için tek tek alanlarda ayarlanır. Aşağıdaki tablolar belirtebileceğiniz türleri ve öznitelikleri numaralandırır.

### <a name="data-types"></a>Veri türleri
| Tür | Açıklama |
| --- | --- |
| *Edm.String* |İsteğe bağlı olarak tam metin araması (sözcük kırma, kökleme vb.) için belirtilebilen metin. |
| *Collection(Edm.String)* |Tam metin araması için isteğe bağlı olarak belirteç haline getirilebilen dize listesi. Bir koleksiyondaki öğelerin sayısında teorik bir üst sınır yoktur ancak yük boyutundaki 16 MB'lık üst sınır, koleksiyonlar için geçerlidir. |
| *Edm.Boolean* |True/false değerlerini içerir. |
| *Edm.Int32* |32 bit tamsayı değerleri. |
| *Edm.Int64* |64 bit tamsayı değerleri. |
| *Edm.Double* |Çift duyarlıklı sayısal veriler. |
| *Edm.DateTimeOffset* |OData V4 biçiminde temsil edilen tarih saati `yyyy-MM-ddTHH:mm:ss.fffZ` değerleri `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`(örneğin, veya). |
| *Edm.GeographyPoint* |Dünya üzerindeki bir coğrafi konumu temsil eden bir nokta. |

Azure Bilişsel Arama'nın desteklenen veri [türleri](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types)hakkında daha ayrıntılı bilgileri burada bulabilirsiniz.

### <a name="index-attributes"></a>Dizin öznitelikleri

Dizininizdeki tam bir alan, her belgeyi benzersiz olarak tanımlayan **anahtar** alan olarak atanmış olmalıdır.

Diğer öznitelikler, bir uygulamada alanın nasıl kullanıldığını belirler. Örneğin, **aranabilir** öznitelik tam metin arama dahil edilmesi gereken her alana atanır. 

Dizin oluşturmak için kullandığınız API'lerin farklı varsayılan davranışları vardır. REST [API'leri](https://docs.microsoft.com/rest/api/searchservice/Create-Index)için, çoğu öznitelik varsayılan olarak etkinleştirilir (örneğin, **aranabilir** ve **alınabilir** dize alanları için doğrudur) ve genellikle yalnızca bunları kapatmak istiyorsanız bunları ayarlamanız gerekir. .NET SDK için tam tersi doğrudur. Açıkça ayarlamadığınız herhangi bir özellikte, varsayılan değer, özellikle etkinleştirmediğiniz sürece ilgili arama davranışını devre dışı bırakmadır.

| Öznitelik | Açıklama |
| --- | --- |
| `key` |Her bir belgenin belge araması için kullanılan benzersiz kimliğini sağlayan bir dize. Tüm dizinlerin bir anahtarı olması gerekir. Yalnızca bir alan anahtar olabilir ve bunun türü Edm.String olarak ayarlanmalıdır. |
| `retrievable` |Bir arama sonucunda bir alanın döndürülüp döndürülemeyeceğini belirtir. |
| `filterable` |Alanın filtre sorgularında kullanılmasını sağlar. |
| `Sortable` |Bu alanı kullanarak bir sorgunun arama sonuçlarını sıralamasını sağlar. |
| `facetable` |Kullanıcının bağımsız filtrelemesi için [modellenmiş bir gezinmede](search-faceted-navigation.md) bir alanın kullanılmasını sağlar. Genellikle birden çok belgeyi bir araya gruplamak için kullanabileceğiniz yinelemeli değerler içeren alanlar (örneğin, tek bir marka veya hizmet kategorisine denk gelen birden çok belge) model olarak en iyi şekilde işler. |
| `searchable` |Alanı tam metin aranabilir şeklinde işaretler. |

## <a name="index-size"></a>Dizin boyutu

Dizin boyutu, yüklediğiniz belgelerin boyutuna ve öneriyi parayıp eklemediğinize ve öznitelikleri tek tek alanlara nasıl ayarladığınıza göre dizin yapılandırmasına göre belirlenir. Aşağıdaki ekran görüntüsü, çeşitli öznitelik kombinasyonlarından kaynaklanan dizin depolama desenleri gösterir.

Dizin, [portalda](search-get-started-portal.md) dizine ekleyip sorgulayabileceğiniz yerleşik gayrimenkul örnek veri kaynağına dayanır. Dizin şemaları gösterilmese de, dizin adına dayalı öznitelikleri çıkarabilirsiniz. Örneğin, *gayrimenkul aranabilir* dizin seçili ve başka bir şey **aranabilir** özniteliği vardır, *gayrimenkul-geri alabilen* dizin seçili ve başka bir şey, **vb. alınabilen** özniteliğe sahiptir.

![Öznitelik seçimine göre dizin boyutu](./media/search-what-is-an-index/realestate-index-size.png "Öznitelik seçimine göre dizin boyutu")

Bu dizin türevleri yapay olsa da, özniteliklerin depolamayı nasıl etkilediğine yönelik geniş karşılaştırmalar için bunlara başvurabiliriz. **Ayarlanabilir** ayar dizin boyutunu artırır mı? Hayır. **Bir Önerici'ye** alan eklemek dizin boyutunu artırır mı? Evet.

Filtreyi ve sıralamayı destekleyen dizinler, yalnızca tam metin aramayı destekleyenlerden orantılı olarak daha büyüktür. Filtreleme ve sıralama işlemleri, bozulmamış belgelerin varlığını gerektiren tam eşleşmeleri tarar. Buna karşılık, tam metin ve bulanık aramayı destekleyen aranabilir alanlar, tüm belgelerden daha az alan tüketen belirteçli terimlerle doldurulan ters dizinler kullanır. 

> [!Note]
> Depolama mimarisi, Azure Bilişsel Arama'nın bir uygulama ayrıntısı olarak kabul edilir ve önceden haber verilmeden değişebilir. Geçerli davranışın gelecekte de devam edeceğinin garantisi yoktur.

## <a name="suggesters"></a>Öneri Araçları
Önerici, şema'nın aramalarda otomatik tamamlama veya ön yazma sorgularını desteklemek için dizindeki hangi alanların kullanıldığını tanımlayan bir bölümüdür. Genellikle, kullanıcı bir arama sorgusu yazarken kısmi arama dizeleri [Öneriler'e (REST API)](https://docs.microsoft.com/rest/api/searchservice/suggestions) gönderilir ve API önerilen belgeler veya tümcecikler kümesini döndürür. 

Bir önericiye eklenen alanlar, ön yazma arama terimleri oluşturmak için kullanılır. Tüm arama terimleri dizin oluşturma sırasında oluşturulur ve ayrı olarak depolanır. Önerici yapısı oluşturma hakkında daha fazla bilgi için [bkz.](index-add-suggesters.md)

## <a name="scoring-profiles"></a>Puanlama modelleri

[Puanlama profili,](index-add-scoring-profiles.md) şema'nın arama sonuçlarında hangi öğelerin daha yüksek görünebilen öğeleri etkilemenize izin veren özel puanlama davranışlarını tanımlayan bir bölümüdür. Puanlama profilleri alan ağırlıkları ve işlevlerinden oluşur. Bunları kullanmak için, sorgu dizesine ada göre bir profil belirtirsiniz.

Varsayılan puanlama profili, bir sonuç kümesindeki her öğe için bir arama puanı hesaplamak için arka planda çalışır. Dahili, isimsiz puanlama profilini kullanabilirsiniz. Alternatif olarak, **varsayılan Puanlama Profili** varsayılan olarak özel bir profil kullanmak için ayarlayın, sorgu dizeüzerinde özel bir profil belirtilmediğinde çağrılan.

## <a name="analyzers"></a>Çözümleyiciler

Çözümleyiciler öğesi alan için kullanılacak dil çözümleyicisinin adını ayarlar. Kullanabileceğiniz çözümleyicilerin aralığı hakkında daha fazla bilgi için [bkz.](search-analyzers.md) Çözümleyiciler yalnızca aranabilir alanlar ile kullanılabilir. Çözümleyici bir alana atandıktan sonra, dizin yeniden oluşturmadığınız sürece değiştirilemez.

## <a name="cors"></a>CORS

İstemci tarafındaki JavaScript varsayılan olarak herhangi bir API'yi arayamaz, çünkü tarayıcı tüm çapraz orijin isteklerini engeller. Dizininizde çapraz orijinsorgularına izin vermek için **corsOptions** özniteliğini ayarlayarak CORS'i (Çapraz Kaynak Paylaşımı) etkinleştirin. Güvenlik nedenleriyle, yalnızca sorgu API'leri CORS'u destekler. 

CORS için aşağıdaki seçenekler ayarlanabilir:

+ **allowedOrigins** (gerekli): Bu, dizininize erişim hakkı verilecek menşelistesidir. Bu, bu kökenlerden sunulan herhangi bir JavaScript kodunun dizinisorgulamasına izin verilecektir (doğru api anahtarı sağladığını varsayarsak). Genellikle atlanır rağmen `protocol://<fully-qualified-domain-name>:<port>` `<port>` her kökeni genellikle formu. Daha fazla bilgi için [başlangıçlı kaynak paylaşımına (Vikipedi)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) bakın.

  Tüm kökenlere erişime izin vermek `*` istiyorsanız, **izin verilen Origins** dizisinde tek bir öğe olarak ekleyin. *Bu, üretim arama hizmetleri için önerilen bir uygulama değildir,* ancak genellikle geliştirme ve hata ayıklama için yararlıdır.

+ **maxAgeInSeconds** (isteğe bağlı): Tarayıcılar, CORS ön kontrol yanıtlarını önbelleğe almak için süreyi (saniye cinsinden) belirlemek için bu değeri kullanır. Bu negatif olmayan bir tamsayı olmalıdır. Bu değer ne kadar büyükse, daha iyi performans olacaktır, ancak CORS ilke değişikliklerinin etkili olması o kadar uzun sürer. Ayarlanmazsa, varsayılan 5 dakikalık bir süre kullanılır.

## <a name="encryption-key"></a>Şifreleme Anahtarı

Tüm Azure Bilişsel Arama dizinleri Varsayılan olarak Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenirken, dizinler Anahtar Kasası'nda **müşteri tarafından yönetilen anahtarlarla** şifrelenecek şekilde yapılandırılabilir. Daha fazla bilgi için Azure [Bilişsel Arama'da şifreleme anahtarlarını yönet'e](search-security-manage-encryption-keys.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Dizin kompozisyonu anlayışıyla, ilk dizin oluşturmak için portalda devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Dizin ekleme (portal)](search-create-index-portal.md)
