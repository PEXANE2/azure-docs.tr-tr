---
title: "Öğretici: Google Maps 'tan Web hizmetlerini geçirme | Microsoft Azure haritaları"
description: Google Maps 'tan Microsoft Azure Maps 'a Web Hizmetleri geçirme.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 50bdc0722328f857279b2cbd9a6e4cee740b9df8
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77048939"
---
# <a name="migrate-web-service-from-google-maps"></a>Web hizmetini Google Maps 'tan geçirme

Azure haritalar ve Google Maps, REST Web Hizmetleri aracılığıyla uzamsal API 'lere erişim sağlar. Bu iki platformun API arabirimleri benzer işlevler gerçekleştirir, ancak her biri farklı adlandırma kuralları ve yanıt nesneleri kullanır.

Aşağıdaki tabloda, Google Maps hizmeti API 'sine benzer bir işlevsellik sağlayan Azure Maps hizmeti API 'SI gösterilmektedir.

| Google Maps hizmeti API 'SI | Azure haritalar hizmeti API 'SI                                                                      |
|-------------------------|---------------------------------------------------------------------------------------------|
| Yükleye              | [Yolu](https://docs.microsoft.com/rest/api/maps/route)                               |
| Uzaklık matrisi         | [Yol matrisi](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) |
| Coğrafi kodlama               | [Search](https://docs.microsoft.com/rest/api/maps/search)                             |
| Konum arama           | [Search](https://docs.microsoft.com/rest/api/maps/search)                             |
| Otomatik tamamlamayı yerleştir      | [Search](https://docs.microsoft.com/rest/api/maps/search)                             |
| Statik eşleme              | [İşlenecek](https://docs.microsoft.com/rest/api/maps/render/getmapimage)                 |
| Saat Dilimi               | [Saat dilimi](https://docs.microsoft.com/rest/api/maps/timezone)                        |

Aşağıdaki hizmet API 'Leri Şu anda Azure haritalar 'da kullanılabilir değildir:

- Nedeniyle
- Konumu
- Ayrıntıları ve yer fotoğraflarını yerleştir. Telefon numaraları ve Web sitesi URL 'SI Azure haritalar arama API 'sinde bulunabilir.
- Harita URL 'Leri
- Ağzı. Hız sınırı verileri Azure Maps 'taki Route ve ters coğrafi kodlama API 'Leri aracılığıyla kullanılabilir.
- Statik cadde görünümü

Azure haritalar, ilgi çekici olabilecek birkaç ek REST Web hizmetine sahiptir:

- [Uzamsal işlemler](https://docs.microsoft.com/rest/api/maps/spatial): bir hizmete bölge sınırlaması gibi karmaşık uzamsal hesaplamalar ve işlemler için yük boşaltma.
- [Trafik](https://docs.microsoft.com/rest/api/maps/traffic): gerçek zamanlı trafik akışına ve olay verilerine erişin.

## <a name="geocoding-addresses"></a>Coğrafi kodlama adresleri

Coğrafi kodlama, bir adresi bir koordinat içine dönüştürme işlemidir. Örneğin, "1 Microsoft Way, Redmond, WA", "Boylam:-122,1298, Latitude: 47,64005" olarak dönüştürülür. Koordinatları haritada konumlandırmak veya Haritayı ortalamak için koordinatlar gereklidir.

Azure Maps, coğrafi kodlama adresleri için çeşitli yöntemler sağlar:

- [**Serbest biçimli adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress): tek bir adres dizesi belirtip isteği hemen işleyin. Tek bir dize adresi "1 Microsoft Way, Redmond, WA" dır. Bu yöntem, tek tek adreslere hızla coğrafi kod eklemeniz gerektiğinde önerilir.
- [**Yapılandırılmış adres coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressstructured): tek bir adresin parçalarını belirtin ve isteği gerçek zamanlı olarak işleyin. Bir adresin parçaları cadde adı, şehir, ülke ve posta kodu içerir. Bu yöntem iki ana senaryo için önerilir. Veriler, tek tek adres parçaları olarak zaten ayrıştırılır. Ya da tek tek adreslerin hızla coğrafi olarak kodda olması gerekir.
- [**Toplu iş adresi coğrafi kodlama**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatchpreview): en fazla 10.000 adres içeren bir istek oluşturun ve isteği bir süre boyunca işleyin. Tüm adresler, sunucuda paralel olarak coğrafi olarak kodlanır. Coğrafi kodlama tamamlandığında, tüm sonuç kümesi indirilebilir hale gelir. Bu yöntem, büyük veri kümelerine coğrafi kodlama için önerilir.
- [**Benzer arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): Bu API, adres coğrafi kodlamasını ilgi çekici arama noktasıyla birleştirir. Bu API, serbest biçimli bir dizeyi alır ve isteği gerçek zamanlı olarak işler. Serbest biçimli bir dize bir adres, yer işareti, bir ilgi alanı veya ilgi alanı kategorisi olabilir. Bu API, adresleri ve ilgilendiğiniz noktaları sorgulamak için aynı metin kutusu kullanıldığında önerilir.
- [**Benzer toplu işlem araması**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): en fazla 10.000 adres içeren bir istek oluşturun ve isteği bir süre boyunca işleyin. Konum, yer işareti veya ilgi noktası isteyebilirsiniz. Tüm veriler sunucuda ve paralel olarak işlenir. Tamamlandığında, tüm sonuç kümesi indirilebilir hale gelir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi  |
|---------------------------|--------------------------------------|
| `address`                   | `query`                            |
| `bounds`                    | `topLeft` ve `btmRight`           |
| `components`                | `streetNumber`<br/>`streetName`<br/>`crossStreet`<br/>`postalCode`<br/>`municipality`-şehir/şehir<br/>`municipalitySubdivision` – komşuları, alt/süper şehir<br/>`countrySubdivision` eyalet veya il<br/>`countrySecondarySubdivision` İlçesi<br/>`countryTertiarySubdivision`-District<br/>`countryCode`-iki harfli ülke kodu |
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `region`                    | `countrySet`                       |

Arama hizmetinin nasıl kullanılacağına ilişkin bir örnek [burada](how-to-search-for-address.md)belgelenmiştir. [Arama için en iyi yöntemleri](how-to-use-best-practices-for-search.md)gözden geçirdiğinizden emin olun.

> [!TIP]
> Serbest biçimli adres coğrafi kodlama ve benzer arama API 'Leri, istek URL 'sine `&amp;typeahead=true` eklenerek otomatik tamamlama modunda kullanılabilir. Bu, sunucuya giriş metninin büyük olasılıkla kısmi olduğunu ve aramanın tahmine dayalı moda gitmesini söyler.

## <a name="reverse-geocode-a-coordinate"></a>Bir koordinatı tersine çevrilmiş coğrafi kod

Ters coğrafi kodlama, coğrafi koordinatları yaklaşık bir adrese dönüştürme işlemidir. "Boylam:-122,1298, Latitude: 47,64005" ile koordinatları "1 Microsoft Way, Redmond, WA" olarak dönüştürür.

Azure haritalar, birkaç ters coğrafi kodlama yöntemi sağlar:

- [**Adres ters coğrafi bölge**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse): Bu koordinat için karşılık gelen yaklaşık adresi almak için tek bir coğrafi koordinat belirtin. İsteği gerçek zamanlı olarak işleyin.
- [**Çapraz cadde ters coğrafi bölge**](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet): tek bir coğrafi koordinat belirtip yakındaki çapraz cadde hakkındaki bilgileri alın. İsteği gerçek zamanlı olarak işleyin.
- [**Batch adresi ters coğrafi makinesi**](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatchpreview): en fazla 10.000 koordinat içeren ve isteği bir süre içinde işleyen bir istek oluşturun. Tüm veriler, sunucuda paralel olarak işlenir. Tamamlandığında, tüm sonuç kümesi indirilebilir hale gelir.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi   | Karşılaştırılabilir Azure Maps API parametresi   |
|-----------------------------|---------------------------------------|
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `latlng`                    | `query`  |
| `location_type`             | *Yok*     |
| `result_type`               | `entityType`    |

[Arama için en iyi uygulamaları](how-to-use-best-practices-for-search.md)gözden geçirin.

Azure haritalar ters coğrafi kodlama API 'SI, Google Maps ' de kullanılamayan bazı ek özelliklere sahiptir. Uygulamanızı geçirirken, bu özellikler uygulamanızla tümleştirilecek şekilde yararlı olabilir:

- Hız sınırı verilerini alma
- Yol kullanım bilgilerini alma: yerel yol, arterial, sınırlı erişim, rampa, vb.
- Bir koordinat bulunduğu cadde tarafını alma

## <a name="search-for-points-of-interest"></a>İlgi noktası arama

Yer arama API 'sini kullanarak Google Haritalar 'da ilgi çekici verilerin bulunduğu noktaya erişilebilir. Bu API, ilgilendiğiniz noktaları aramak için üç farklı yol sağlar:

- **Metinden yer bulun:** Adını, adresini veya telefon numarasını temel alarak bir ilgi alanı arar.
- **Yakın arama**: bir konumun belirli bir uzaklıkta yer alan ilgi alanlarını arar.
- **Metin arama:** Bir dizi ilgi ve konum bilgisi içeren serbest biçimli bir metin kullanarak yerleri arar. Örneğin, "New York 'ta pizza" veya "ana St yakınında" Restoranlar ".

Azure Maps, ilgi noktaları için çeşitli arama API 'Leri sağlar:

- [**POI arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi): ada göre ilgi alanları için arama yapın. Örneğin, "Starbuları".
- [**POI kategorisi arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory): kategoriye göre ilgi alanları için arama yapın. Örneğin, "Restoran".
- [**Yakın arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby): bir konumun belirli bir uzaklıkta yer alan ilgi alanlarını arar.
- [**Benzer arama**](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy): Bu API, adres coğrafi kodlamasını ilgi çekici arama noktasıyla birleştirir. Bu API, serbest biçimli bir dize alır. Bir adres, yer, yer işareti, ilgi çekici bir nokta, bir dizi ilgi alanı vb. Bu API, isteği neredeyse gerçek zamanlı işleyebilir. Kullanıcılar aynı metin kutusunu kullanarak adresleri veya ilgi çekici noktaları ararken bu API önerilir.
- [**Geometri dahilinde ara**](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry): belirtilen geometri (çokgen) içinde olan ilgi alanlarını arayın.
- [**Yol boyunca ara**](https://docs.microsoft.com/rest/api/maps/search/postsearchalongroute): belirtilen yol yolu boyunca ilgi alanları için arama yapın.
- [**Benzer toplu işlem araması**](https://docs.microsoft.com/rest/api/maps/search/postsearchfuzzybatchpreview): en fazla 10.000 adres, yer, yer işareti veya ilgi alanı içeren ve bunların bir süre içinde işlenmesini içeren bir istek oluşturun. Tüm veriler, sunucuda paralel olarak işlenir. İstek işlemi tamamlandığında, tüm sonuç kümesini indirin.

Şu anda Azure haritalar 'ın, Google Maps 'ta metin arama API 'sine karşılaştırılabilir bir API 'SI yoktur.

> [!TIP]
> POı Search API 'SI, POı kategorisi arama API 'SI ve benzer arama API 'Leri her biri otomatik tamamlama modunda kullanılabilir. İstek URL 'sine `&amp;typeahead=true` ekleyin. Bu, sunucuya giriş metninin büyük olasılıkla kısmi olduğunu ve aramanın tahmine dayalı modda tamamlanacaktır.

[Arama için en iyi uygulamaları](how-to-use-best-practices-for-search.md)gözden geçirin.

### <a name="find-place-from-text"></a>Metinden yer bulun

Azure haritalar 'ı [POI Search](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) API 'Sini veya [benzer arama](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API 'sini kullanarak, ilgi alanları adına veya adrese göre arama yapın.

Aşağıdaki tabloda, Google Maps API parametreleri ilgili Azure Maps API parametreleri ile gösterilmektedir.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi |
|---------------------------|-------------------------------------|
| `fields`                  | *Yok*                               |
| `input`                   | `query`                             |
| `inputtype`               | *Yok*                               |
| `key`                     | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `locationbias`            | `lat`, `lon` ve `radius`<br/>`topLeft` ve `btmRight`<br/>`countrySet`  |

### <a name="nearby-search"></a>Yakındaki arama

Azure haritalar 'da, [yakındaki arama](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) API 'sini kullanarak yakın ilgi alanları alın.

Aşağıdaki tabloda, Azure Maps API parametreleri ile Google Maps API parametreleri çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi  |
|---------------------------|--------------------------------------|
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `keyword`                   | `categorySet` ve `brandSet`        |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `location`                  | `lat` ve `lon`                     |
| `maxprice`                  | *Yok*                               |
| `minprice`                  | *Yok*                               |
| `name`                      | `categorySet` ve `brandSet`        |
| `opennow`                   | *Yok*                               |
| `pagetoken`                 | `ofs` ve `limit`                   |
| `radius`                    | `radius`                            |
| `rankby`                    | *Yok*                               |
| `type`                      | `categorySet –` bkz. [desteklenen arama kategorileri](supported-search-categories.md) belgeleri.   |

## <a name="calculate-routes-and-directions"></a>Rotaları ve yönleri hesapla

Azure haritalar 'ı kullanarak rotaları ve yönergeleri hesaplayın. Azure haritalar, Google Maps yönlendirme hizmetiyle aynı işlevselliklerin çoğuna sahiptir, örneğin:

- varış ve ayrılma süreleri.
- gerçek zamanlı ve tahmine dayalı trafik rotaları.
- Farklı ulaşım modları. Örneğin, gidiş, yürüyen, Bicycling.

> [!NOTE]
> Azure haritalar tüm waypoints 'in koordinatlarını gerektirir. Önce adreslerin coğrafi kodlanmış olması gerekir.

Azure haritalar yönlendirme hizmeti, yolları hesaplamak için aşağıdaki API 'Leri sağlar:

- [**Rotayı hesapla**](https://docs.microsoft.com/rest/api/maps/route/getroutedirections): bir rotayı hesaplayın ve isteği gerçek zamanlı olarak işleyin. Bu API hem GET hem POST isteklerini destekler. POST istekleri çok sayıda waypoints belirtmek için veya birçok yol seçeneği kullanılarak önerilir. POST kullanımı, URL isteğinin çok uzun sürmemesini ve soruna neden olmamasını sağlar.
- [**Batch rotası**](https://docs.microsoft.com/rest/api/maps/route/postroutedirectionsbatchpreview): en fazla 1.000 yol isteği içeren bir istek oluşturun. İstek bir süre boyunca işlenir. Tüm veri, sunucuda paralel olarak işlenir. İstek tamamlandıktan hemen sonra kümeyi indirin ve sonuçlar kullanıma alınır.
- [**Mobility Hizmetleri**](https://docs.microsoft.com/rest/api/maps/mobility): genel aktarım kullanarak rotaları ve yönergeleri hesaplayın.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi    | Karşılaştırılabilir Azure Maps API parametresi  |
|------------------------------|--------------------------------------|
| `alternatives`                 | `maxAlternatives`                  |
| `arrival_time`                | `arriveAt`                          |
| `avoid`                        | `avoid`                            |
| `departure_time`              | `departAt`                          |
| `destination`                  | `query`: `"lat0,lon0:lat1,lon1…."` biçimde koordinatları  |
| `key`                          | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                     | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.   |
| `mode`                         | `travelMode`                       |
| `optimize`                     | `computeBestOrder`                 |
| `origin`                       | `query`                            |
| `region`                       | *Yok* : Bu özellik, coğrafi kodlama ile ilgilidir. Azure Haritalar Coğrafi Kodlama API 'sini kullanırken *Countryset* parametresini kullanın.  |
| `traffic_model`               | *Yok* – *trafik parametresi yalnızca trafik parametresiyle* birlikte kullanılması gerekiyorsa belirtilebilir. |
| `transit_mode`                | [Mobility Hizmetleri belgelerine](https://docs.microsoft.com/rest/api/maps/mobility) bakın |
| `transit_routing_preference` | [Mobility Hizmetleri belgelerine](https://docs.microsoft.com/rest/api/maps/mobility) bakın |
| `units`                        | *Yok* – Azure Maps yalnızca ölçüm sistemini kullanır.  |
| `waypoints`                    | `query`                            |

> [!TIP]
> Varsayılan olarak, Azure Maps Route API 'SI yalnızca bir Özet (uzaklık ve saatler) ve yol yolunun koordinatlarını döndürür. Geri alma yönergelerini almak için `instructionsType` parametresini kullanın. Özet ve yol yolunu filtrelemek için `routeRepresentation` parametresini kullanın.

Azure haritalar yönlendirme API 'SI, Google Maps ' de kullanılamayan birçok ek özelliğe sahiptir. Uygulamanızı geçirirken bu özellikleri bütünleştirmek yararlı olabilir:

- Rota türü desteği: en kısa, en hızlı, trilini ve en çok yakıt etkin.
- Ek seyahat modları için destek: Bus, motocycle, Taxi, kamyon ve Van.
- 150 waypoints desteği.
- Tek bir istekte birden çok seyahat süresi hesaplama; geçmiş trafik, canlı trafik, trafik yok.
- Ek yol türlerinden kaçının: Carpool yol, geri alınamaz yollar, zaten kullanılan yollar.
- Kaçınmak için özel alan belirtin.
- Yolun Ascend olabileceği yükseltmeyi sınırlayın.
- Altyapı belirtimlerine göre rota. Altyapı belirtimlerini ve kalan yakıt veya ücreti temel alan combustion veya elektrik taşıtlar için yolları hesaplayın.
- Ticari araç yönlendirme parametrelerini destekler. Örneğin, araç boyutları, ağırlık, Axler sayısı ve kargo türü.
- Maksimum araç hızını belirtin.

Bu özelliklere ek olarak, Azure Maps 'taki yönlendirme hizmeti [yönlendirilebilir aralıkları hesaplamayı](https://docs.microsoft.com/rest/api/maps/route/getrouterange)destekler. Yönlendirilebilir aralıkları hesaplama de ikizde olarak bilinir. Çokgen ile kapsanan bir alan oluşturulmasını gerektirir. Ardından, bir kaynak noktasından herhangi bir yönde seyahat hesaplanıyor. Her şey, belirli bir süre ve yakıt veya ücret miktarı göz önünde bulundurularak.

## <a name="retrieve-a-map-image"></a>Harita görüntüsünü al

Azure Maps, çakışan verilerle statik harita görüntülerini işlemek için bir API sağlar. Azure haritalar 'daki [harita görüntü işleme](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile) API 'Si, Google Maps 'taki statik harita API 'siyle karşılaştırılabilir.

> [!NOTE]
> Azure haritalar, "boylam, enlem" biçiminde koordinat olacak şekilde Ortala, tüm işaretleyici ve yol konumlarını gerektirir. Diğer yandan, Google Maps "enlem, Boylam" biçimini kullanır. Önce adreslerin coğrafi kodlanmış olması gerekir.

Aşağıdaki tabloda, karşılaştırılabilir Azure Maps API parametreleri ile Google Maps API parametreleri gösterilmektedir.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi  |
|---------------------------|--------------------------------------|
| `center`                    | `center`                           |
| `format`                    | `format`: URL yolunun bir parçası olarak belirtilir. Şu anda yalnızca PNG destekleniyor. |
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `maptype`                   | `layer` ve `style` – [desteklenen harita stilleri](supported-map-styles.md) belgelerine bakın. |
| `markers`                   | `pins`                             |
| `path`                      | `path`                             |
| `region`                    | *Yok* – bu, coğrafi kodlama ile ilgili bir özelliktir. Azure Maps coğrafi kodlama API 'sini kullanırken `countrySet` parametresini kullanın.  |
| `scale`                     | *Yok*                              |
| `size`                      | `width` ve `height` – boyutu 8192x8192 olabilir. |
| `style`                     | *Yok*                              |
| `visible`                   | *Yok*                              |
| `zoom`                      | `zoom`                             |

> [!NOTE]
> Azure haritalar, Google Maps 'ta kullanılan harita kutucukları boyutunun iki katı olan kutucukları olan bir kutucuk sistemi kullanır. Bu nedenle yakınlaştırma düzeyi değeri, Google Maps ile karşılaştırıldığında Azure Maps 'ta bir yakınlaştırma düzeyi daha yakından görünüyor. Geçirdiğiniz isteklerde yakınlaştırma düzeyini bir tane azaltır. Yakınlaştırma düzeyi değerinin, kutucuk sistemlerindeki çeşitçuna göre dengelenmesini azaltma.

Daha fazla bilgi için [harita görüntü Işleme API 'Sindeki nasıl yapılır kılavuzuna](how-to-render-custom-data.md)bakın.

Azure haritalar işleme hizmeti, statik bir harita görüntüsü oluşturmanın yanı sıra tarama (PNG) ve vektör biçimindeki harita kutucuklarına doğrudan erişme özelliği sağlar:

- [**Harita kutucuğu**](https://docs.microsoft.com/rest/api/maps/render/getmaptile): temel haritalar (yollar, sınırlar, arka plan) için raster (png) ve vektör kutucukları alma.
- [**Harita Imagery kutucuğu**](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile): havadan ve uydu görüntüsü al kutucukları alın.

> [!TIP]
> Birkaç yıl önce, birçok Google Maps uygulaması, bir maliyet tasarrufu yöntemi olarak, etkileşimli eşleme deneyimlerinden statik harita görüntülerine geçiş yaptı. Azure haritalar 'da, Web SDK 'sında etkileşimli harita denetimini kullanmak genellikle çok daha uygun maliyetli. Hizmet ücreti, uygulamanın yüklediği harita kutucuklarının sayısına bağlıdır. Azure haritalar 'daki harita kutucukları büyük. Genellikle, aynı harita görünümünü statik eşleme ile yeniden oluşturmak için yalnızca birkaç kutucuk sürer. Harita kutucukları tarayıcı tarafından otomatik olarak önbelleğe alınır. Bu nedenle, etkileşimli harita denetimi genellikle statik harita görünümü çoğaltma yaparken bir işlemin bir bölümünü oluşturur. Yatay kaydırma ve yakınlaştırma daha fazla kutucuk yükler, ancak bu davranışı devre dışı bırakmak için harita denetiminde seçenekler vardır. Etkileşimli harita denetimi, statik eşleme hizmetlerinden çok daha fazla görselleştirme seçeneği de sağlar.

### <a name="marker-url-parameter-format-comparison"></a>İşaret URL 'SI parametre biçimi karşılaştırması

**Önce: Google Maps**

Google Maps ' de, URL 'deki `markers` parametresini kullanarak statik harita resmine işaretçiler ekleyebilirsiniz. `markers` parametresi, aşağıda gösterildiği gibi, bu stille birlikte haritada oluşturulacak konumların bir stilini ve bir listesini alır:

```
&markers=markerStyles|markerLocation1|markerLocation2|...
```

Ek stiller, URL 'ye farklı bir stille ve konum kümesiyle ek `markers` parametreler eklenerek kullanılabilir.

İşaret konumları "enlem, Boylam" biçimiyle belirtilir.

Google Maps 'daki işaret stilleri, kanal (\|) karakterleriyle ayrılmış birden çok stil ile `optionName:value`biçim ile eklenir. Şöyle: "optionName1: değer1\|optionName2: değer2". Seçenek adları ve değerleri, iki nokta üst üste (:)) ayrılır. Aşağıdaki stil seçenek adları, Google Maps 'daki stil işaretçileri için kullanılabilir:

- `color`: varsayılan işaret simgesinin rengi. 24 bit onaltılı renk (`0xrrggbb`) veya aşağıdaki değerlerden biri olabilir; `black`, `brown`, `green`, `purple`, `yellow`, `blue`, `gray`, `orange`, `red`, `white`.
- `label`: simgenin üstünde görüntülenecek tek bir büyük harf alfasayısal karakter.
- `size`-işaretin boyutu. `tiny`, `mid`veya `small`olabilir.

Aşağıdaki stil seçenek adları kullanılarak, Google Maps 'a özel simgeler eklenebilir:

- `anchor` – simge resminin koordinatya nasıl hizalanacağını belirtir. Bir piksel (x, y) değeri veya aşağıdaki değerlerden biri olabilir; `top`, `bottom`, `left`, `right`, `center`, `topleft`, `topright`, `bottomleft`veya `bottomright`.
- `icon`: simge resmine işaret eden bir URL.

Örneğin, Google Maps 'ta haritaya, koordinatlara (Boylam:-110, Latitude: 45) aşağıdaki URL parametresi ile kırmızı ve orta ölçekli bir işaret eklenebilir:

```
&markers=color:red|size:mid|45,-110
```

<center>

![Google Maps işaretleyici](media/migrate-google-maps-web-services/google-maps-marker.png)</center>

**Sonrasında: Azure Maps**

Azure Maps ' de, URL 'de `pins` parametresini belirterek statik harita resmine işaretçiler ekleyin. Google Maps gibi, bu parametrede bir stil ve konumların bir listesini belirtin. Farklı stillerle işaretçileri desteklemek için `pins` parametresini birden çok kez belirtin.

```
&pins=iconType|pinStyles||pinLocation1|pinLocation2|...
```

Ek stilleri kullanmak için, URL 'ye farklı bir stille ve konum kümesiyle ek `pins` parametreleri ekleyin.

PIN konumu için Azure Maps, koordinatların "Boylam Enlem" biçiminde olmasını gerektirir. Google Maps "enlem, Boylam" biçimini kullanır. Bir boşluk değil, bir virgül değil, boylam ve enlem 'yi Azure Maps biçiminde ayırır.

`iconType` oluşturulacak PIN türünü belirtir. Aşağıdaki değerlere sahip olabilir:

- `default` – varsayılan pin simgesi.
- `none` – hiçbir simge gösterilmez, yalnızca Etiketler işlenir.
- `custom` – kullanılacak özel bir simge belirtir. Simge resmine işaret eden bir URL, PIN konum bilgileri sonrasında `pins` parametresinin sonuna eklenebilir.
- `{udid}`: Azure Maps veri depolama platformunda depolanan bir simgenin benzersiz veri KIMLIĞI (UDıD).

Azure haritalar 'a `optionNameValue` biçimiyle PIN stilleri ekleyin. Birden çok stili kanal (\|) karakterleriyle ayırın. Örneğin: `iconType|optionName1Value1|optionName2Value2`. Seçenek adları ve değerleri ayrılmamış. Azure haritalar 'daki stil işaretçileri için aşağıdaki stil seçenek adlarını kullanın:

- `al` – işaretin opaklığını (Alpha) belirtir. 0 ile 1 arasında bir sayı seçin.
- `an` – pin bağlayıcısını belirtir. "X y" biçiminde belirtilen X ve y piksel değerleri.
- `co` – PIN rengi. 24 bit onaltılık renk olmalıdır: `FFFFFF``000000`.
- `la` – etiket bağlayıcısını belirtir. "X y" biçiminde belirtilen X ve y piksel değerleri.
- `lc`: etiketin rengi. 24 bit onaltılık renk olmalıdır: `FFFFFF``000000`.
- `ls`: etiketin piksel cinsinden boyutu. 0 ' dan büyük bir sayı seçin.
- `ro`: simgeyi döndürmek için derece cinsinden bir değer. -360 ile 360 arasında bir sayı seçin.
- `sc`: pin simgesi için bir ölçek değeri. 0 ' dan büyük bir sayı seçin.

Etiket değerleri her PIN konumu için belirtilir. Bu yaklaşım, konum listesindeki tüm işaretçilere tek bir etiket değeri uygulamaktan daha etkilidir. Etiket değeri birden çok karakterden oluşan bir dize olabilir. Bir stil veya konum değeri olarak yanlış alınana emin olmak için dizeyi tek tırnak işaretleriyle sarın.

Örneğin, Azure Maps 'ta, "Space Iğne" etiketiyle kırmızı (`FF0000`) varsayılan bir simge ekleme (15 50), aşağıdaki URL parametresi ile birlikte (Boylam:-122,349300, Latitude: 47,620180) simgesiyle yapılır:

```
&pins=default|coFF0000|la15 50||'Space Needle' -122.349300 47.620180
```

<center>

![Azure haritaları işaretçisi](media/migrate-google-maps-web-services/azure-maps-marker.png)</center>

Aşağıdaki örnek, ' 1 ', ' 2 ' ve ' 3 ' Etiket değerleriyle üç PIN ekler:

```
&pins=default||'1'-122 45|'2'-119.5 43.2|'3'-121.67 47.12
```

<center>

Azure ![çoklu işaretçileri eşler](media/migrate-google-maps-web-services/azure-maps-multiple-markers.png)</center>

### <a name="path-url-parameter-format-comparison"></a>Yol URL parametre biçimi karşılaştırması

**Önce: Google Maps**

Google Maps 'ta, URL 'deki `path` parametresi kullanılarak bir statik harita görüntüsüne çizgiler ve çokgenler eklenebilir. `path` parametresi, aşağıda gösterildiği gibi bir stil ve haritada işlenecek konumların bir listesini alır:

```
&path=pathStyles|pathLocation1|pathLocation2|...
```

Ek stiller, URL 'ye farklı bir stille ve konum kümesiyle ek `path` parametreler eklenerek kullanılabilir.

Google Maps 'teki yol konumları `latitude1,longitude1|latitude2,longitude2|…`biçimiyle belirtilir. Yollar, noktaların adreslerini kodlanabilir veya içerebilir.

Google Maps 'taki yol stilleri, kanal (\|) karakterleriyle ayrılmış birden çok stil ile `optionName:value`biçim ile eklenir. Şöyle: `optionName1:value1|optionName2:value2`. Seçenek adları ve değerleri, iki nokta üst üste (:)) ayrılır. Aşağıdaki stil seçenek adları, Google Maps içindeki yollara stil eklemek için kullanılabilir:

- `color`: yolun veya Çokgen anahattının rengi. 24 bit onaltılı renk (`0xrrggbb`), 32 bitlik bir onaltılık renk (`0xrrggbbbaa`) veya aşağıdaki değerlerden biri olabilir: siyah, kahverengi, yeşil, mor, sarı, mavi, gri, turuncu, kırmızı, beyaz.
- `fillColor`: yol alanını (çokgen) dolduracak olan renk. 24 bit onaltılı renk (`0xrrggbb`), 32 bitlik bir onaltılık renk (`0xrrggbbbaa`) veya aşağıdaki değerlerden biri olabilir: siyah, kahverengi, yeşil, mor, sarı, mavi, gri, turuncu, kırmızı, beyaz.
- `geodesic` – yolun, dünya eğriliği takip eden bir çizgi olması gerekip gerekmediğini gösterir.
- `weight`: yolun piksel cinsinden kalınlığı.

Google Maps 'ta, URL parametresindeki koordinatlar arasındaki haritaya kırmızı bir çizgi geçirgenliği ve piksel kalınlığı eklenebilir. Aşağıdaki örnekte, satırda %50 opaklık ve dört piksel kalınlığı bulunur. Koordinatlar Boylam:-110, Enlem: 45 ve Boylam:-100, Enlem: 50.

```
&path=color:0xFF000088|weight:4|45,-110|50,-100
```

<center>

![Google Maps çoklu çizgi](media/migrate-google-maps-web-services/google-maps-polyline.png)</center>

**Sonrasında: Azure Maps**

Azure Maps ' de, URL 'de `path` parametresini belirterek statik harita görüntüsüne çizgiler ve çokgenler ekleyin. Google Maps gibi, bu parametrede bir stil ve konumların bir listesini belirtin. Birden çok daire, çizgi ve poligonu farklı stillerle işlemek için `path` parametresini birden çok kez belirtin.

```
&path=pathStyles||pathLocation1|pathLocation2|...
```

Azure haritalar, yol konumlarına geldiğinde, koordinatların "Boylam Enlem" biçiminde olmasını gerektirir. Google Maps "enlem, Boylam" biçimini kullanır. Bir boşluk değil, bir virgül değil, boylam ve enlem 'yi Azure Maps biçiminde ayırır. Azure Maps, noktalara yönelik kodlanmış yollar veya adresler desteklemez. Daha büyük veri kümelerini, [burada](how-to-render-custom-data.md#get-data-from-azure-maps-data-storage)belgelenen şekilde Azure Maps VERI depolama API 'sine doldurma olarak karşıya yükleyin.

Azure Maps ' de `optionNameValue` biçimiyle yol stilleri ekleyin. Birden çok stili, bu `optionName1Value1|optionName2Value2`gibi kanal (\|) karakterleriyle ayırın. Seçenek adları ve değerleri ayrılmamış. Azure haritalar 'daki stil yolları için aşağıdaki stil seçenek adlarını kullanın:

- `fa`-poligonları işlerken kullanılan Fill Color geçirgenliği (Alpha). 0 ile 1 arasında bir sayı seçin.
- `fc`-bir çokgenin alanını işlemek için kullanılan Fill Color.
- `la`: çizgiler ve çokgenler ana hattı işlenirken kullanılan çizgi rengi geçirgenliği (Alpha). 0 ile 1 arasında bir sayı seçin.
- `lc`: çizgiler ve çokgenlerin anahattını işlemek için kullanılan çizgi rengi.
- `lw`: çizginin piksel cinsinden genişliği.
- `ra`: ölçümlerde bir daire yarıçapı belirtir.

Azure haritalar 'da, URL parametresindeki koordinatlar arasına kırmızı bir çizgi geçirgenliği ve piksel kalınlığı ekleyin. Aşağıdaki örnekte, satır %50 opaklık ve dört piksellik bir kalınlığa sahiptir. Koordinatlar şu değerlere sahiptir: Boylam:-110, Enlem: 45 ve Boylam:-100, Enlem: 50.

```
&path=lcFF0000|la.5|lw4||-110 45|-100 50
```

<center>

![Azure Maps çoklu çizgi](media/migrate-google-maps-web-services/azure-maps-polyline.png)</center>

## <a name="calculate-a-distance-matrix"></a>Uzaklık matrisini hesaplama

Azure haritalar mesafe matris API 'sini sağlar. Seyahat sürelerini ve bir konum kümesi arasındaki mesafeyi, uzaklık matrisi ile hesaplamak için bu API 'yi kullanın. Google Maps 'taki uzaklık Matrisi API 'siyle karşılaştırılabilir.

- [**Yol matrisi**](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview): zaman uyumsuz bir çıkış ve hedef kümesi için seyahat sürelerini ve uzaklıkları hesaplar. İstek başına en fazla 700 hücreyi destekler (hedef sayısı ile çarpılan çıkış sayısı. Bu kısıtlama göz önünde bulundurularak, olası matris boyutlarına örnek olarak şunlar verilebilir: 700x1, 50x10, 10x10, 28x25, 10x70.

> [!NOTE]
> Uzaklık Matrisi API 'sine yönelik bir istek yalnızca istek gövdesinde kaynak ve hedef bilgileri olan bir POST isteği kullanılarak yapılabilir. Ayrıca, Azure Maps tüm kaynakları ve hedefleri koordine etmek gerektirir. Önce adreslerin coğrafi kodlanmış olması gerekir.

Aşağıdaki tabloda, Google Maps API parametrelerine benzer Azure Maps API parametreleri ile çapraz başvuru yapılır.

| Google Maps API parametresi      | Karşılaştırılabilir Azure Maps API parametresi  |
|--------------------------------|--------------------------------------|
| `arrivial_time`                | `arriveAt`                           |
| `avoid`                        | `avoid`                              |
| `depature_time`                | `departAt`                           |
| `destinations`                 | `destination` – istek SONRASı gövdesinin coğrafi JSON olarak belirleyin. |
| `key`                          | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri. |
| `language`                     | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.  |
| `mode`                         | `travelMode`                         |
| `origins`                      | `origins` – istek SONRASı gövdesinin coğrafi JSON olarak belirleyin.  |
| `region`                       | *Yok* : Bu özellik, coğrafi kodlama ile ilgilidir. Azure Maps coğrafi kodlama API 'sini kullanırken `countrySet` parametresini kullanın. |
| `traffic_model`                | *Yok* – yalnızca trafik verilerinin `traffic` parametresiyle kullanılması gerekiyorsa belirtilebilir. |
| `transit_mode`                 | *N/A* -aktarım tabanlı uzaklık matrisleri Şu anda desteklenmiyor.  |
| `transit_routing_preference`   | *N/A* -aktarım tabanlı uzaklık matrisleri Şu anda desteklenmiyor.  |
| `units`                        | *Yok* – Azure Maps yalnızca ölçüm sistemini kullanır. |

> [!TIP]
> Azure haritalar yönlendirme API 'sinde bulunan tüm gelişmiş yönlendirme seçenekleri Azure haritalar Mesafe Matrisi API 'sinde desteklenir. Gelişmiş yönlendirme seçenekleri şunlardır: Ruck yönlendirme, altyapı belirtimleri vb.

## <a name="get-a-time-zone"></a>Saat dilimi al

Azure Maps, bir koordinat saat dilimini almak için bir API sağlar. Azure haritalar Saat dilimi API 'SI, Google Maps 'taki saat dilimi API 'siyle karşılaştırılabilir:

- [**Koordine eden saat dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebycoordinates): koordinat belirtin ve koordinat saat dilimi ayrıntılarını alın.

Aşağıdaki tabloda, Google Maps API parametreleri Azure haritalar 'daki karşılaştırılabilir API parametreleriyle birlikte çapraz başvuru yapılır.

| Google Maps API parametresi | Karşılaştırılabilir Azure Maps API parametresi   |
|---------------------------|---------------------------------------|
| `key`                       | `subscription-key` – Ayrıca bkz. [Azure Maps Ile kimlik doğrulama](azure-maps-authentication.md) belgeleri.       |
| `language`                  | `language` – [desteklenen diller](supported-languages.md) belgesine bakın.    |
| `location`                  | `query`             |
| `timestamp`                 | `timeStamp`         |

Bu API 'nin yanı sıra Azure Maps Platformu, bir dizi zaman dilimi API 'si sağlar. Bu API 'Ler saati, saat diliminin adlarına veya kimliklerine göre dönüştürür:

- [**Kimliğe göre saat dilimi**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonebyid): belirtilen IANA saat dilimi kimliği için geçerli, geçmiş ve gelecekteki saat dilimi bilgilerini döndürür.
- [**Bölge enum IANA**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumiana): bir IANA saat dilimi kimliklerinin tam listesini döndürür. IANA hizmetine yapılan güncelleştirmeler sisteme bir gün içinde yansıtılır.
- [**Zaman dilimi numaralandırması pencereleri**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneenumwindows): Windows saat dilimi kimliklerinin tam listesini döndürür.
- [**Saat dılımı IANA sürümü**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezoneianaversion): Azure haritalar tarafından kullanılan geçerli IANA sürüm numarasını döndürür.
- [**IANA Için saat dilimi pencereleri**](https://docs.microsoft.com/rest/api/maps/timezone/gettimezonewindowstoiana): geçerli bir Windows saat dilimi kimliği verilen ılgılı bır IANA kimliğini döndürür. Tek bir Windows KIMLIĞI için birden çok ıANA kimliği döndürülebilir.

## <a name="client-libraries"></a>İstemci kitaplıkları

Azure Maps, istemci kitaplıklarını aşağıdaki programlama dilleri için sağlar:

- JavaScript, TypeScript, Node. js – [belgeler](how-to-use-services-module.md) \| [NPM paketi](https://www.npmjs.com/package/azure-maps-rest)

Diğer programlama dilleri için açık kaynaklı istemci kitaplıkları:

- .NET Standard 2,0 – [GitHub projesi](https://github.com/perfahlen/AzureMapsRestServices) \| [NuGet paketi](https://www.nuget.org/packages/AzureMapsRestToolkit/)

## <a name="additional-resources"></a>Ek kaynaklar

Azure haritalar REST Hizmetleri için bazı ek belgeler ve kaynaklar aşağıda verilmiştir.

- [Arama için en iyi uygulamalar](how-to-use-best-practices-for-search.md)
- [Adres arayın](how-to-search-for-address.md)
- [Azure haritalar REST hizmeti API başvuru belgeleri](https://docs.microsoft.com/rest/api/maps/)

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar REST hizmetleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Arama hizmetini kullanmaya yönelik en iyi uygulamalar](how-to-use-best-practices-for-search.md)

> [!div class="nextstepaction"]
> [Hizmetler modülünü kullanma (Web SDK)](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)