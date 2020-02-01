---
title: Azure haritalar arama hizmetlerini kullanarak bir konum arayın | Microsoft Azure haritaları
description: Bu makalede, coğrafi kodlama ve ters coğrafi kodlama için Microsoft Azure haritaları Arama Hizmeti kullanarak bir konumun nasıl aranalınacağını öğreneceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 3b5da7eab9cff5c5e051fc4d5ab7ff582a95c20d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899236"
---
# <a name="search-for-a-location-using-azure-maps-search-services"></a>Azure haritalar arama hizmetlerini kullanarak bir konum arayın

Azure haritalar [Arama hizmeti](https://docs.microsoft.com/rest/api/maps/search) , geliştiricilerin adresleri, yerleri, iş dökümlerini ada veya kategoriye göre ve diğer coğrafi bilgileri aramasına yardımcı olmak üzere tasarlanan bir dizi yeniden API 'dir. Geleneksel kodlamayı 'un desteklenmesinin yanı sıra, hizmetler Ayrıca coğrafi kod adreslerini ve çapraz Streets 'leri de Latitudes ve Longitudes göre ters çevirebilirsiniz. Arama tarafından döndürülen Enlem ve boylam değerleri, [yönlendirme](https://docs.microsoft.com/rest/api/maps/route) ve [Hava durumu](https://docs.microsoft.com/rest/api/maps/weather) hizmetleri gibi diğer Azure haritalar hizmetlerinde parametre olarak kullanılabilir.

Bu makalede şunları öğreneceksiniz:

* [Arama adresi API 'sini]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) kullanarak bir adresin (geocode adres konumu) enlem ve boylam koordinatlarını isteyin
* [Benzer arama API 'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) kullanarak bir adres veya ilgi noktası (POI) arayın
* Özellikler ve koordinatlarla birlikte bir adres arayın
* Koordinat konumunu cadde adresine çevirmek için [ters adres araması](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) yapın
* [Arama adresi ters açık çapraz API 'yi](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet) kullanarak çapraz cadde arama

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için öncelikle bir Azure haritalar hesabı oluşturmanız ve hesap abonelik anahtarını eşleirsiniz. Hesap [oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) ' daki yönergeleri Izleyerek Azure Maps hesap aboneliği oluşturun ve hesabınızın birincil anahtarını almak için [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.

## <a name="request-latitude-and-longitude-for-an-address-geocoding"></a>Bir adres (coğrafi kodlama) için Enlem ve Boylam iste

Bu örnekte, açık adresi Enlem ve Boylam koordinatlarına dönüştürmek için Azure haritalar [arama adresi al API 'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) kullanıyoruz. API 'ye tamamen veya kısmi bir adres geçirebilir ve cadde, posta kodu ve ülke/bölge gibi ayrıntılı adres özelliklerinin yanı sıra Enlem ve boylam değerlerinin konumsal değerlerini içeren bir yanıt alabilirsiniz.

Geocode 'a yönelik bir adres kümesine sahipseniz, tek bir API çağrısında bir toplu sorgu göndermek için [arama adresini gönder Batch API 'sini](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatch) kullanabilirsiniz.

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **Adres aramasını**adlandırın.

2. Oluşturucu sekmesinde http **Al** metodunu SEÇIN, API uç noktanız IÇIN istek URL 'sini girin ve varsa bir Yetkilendirme Protokolü seçin.

![Adres arama](./media/how-to-search-for-address/address_search_url.png)

| Parametre | Önerilen değer |
|---------------|------------------------------------------------| 
| HTTP yöntemi | GET |
| İstek URL'si | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) | 
| Yetkilendirme | Kimlik doğrulama yok |

3. **Parametreler**' e tıklayın ve istek URL 'sinde sorgu veya yol parametreleri olarak kullanılacak aşağıdaki anahtar/değer çiftlerini girin: 

![Adres arama](./media/how-to-search-for-address/address_search_params.png) 

| Anahtar | Değer | 
|------------------|-------------------------| 
| api-version | 1.0 | 
| abonelik-anahtar | Azure haritalar anahtarınızı \<\> | 
| sorgu | 400 geniş St, Seattle, WA 98109 | 

4. **Gönder** ' e tıklayın ve yanıt gövdesini gözden geçirin. 

Bu durumda, tüm adres sorgusu belirttiniz ve yanıt gövdesinde tek bir sonuç alacaksınız. 

5. Params ' de sorgu dizesini aşağıdaki değere düzenleyin: 

    ```plaintext 
        400 Broad, Seattle 
    ``` 

6. Aşağıdaki anahtar/değer çiftini **params** bölümüne ekleyin ve **Gönder**' e tıklayın: 

| Anahtar | Değer | 
|-----|------------| 
| typeahead | doğru | 

**Typeahead** bayrağı, adres araması API 'sine sorguyu kısmi giriş olarak ele almasını ve bir tahmine dayalı değerler dizisi döndürmesini söyler.

## <a name="using-fuzzy-search-api"></a>Benzer arama API 'sini kullanma

Azure Maps[ ,](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) Kullanıcı girdlerinizin bir arama sorgusu için ne olduğunu bilmediğinizde, kullanılması önerilen bir hizmettir. API, Ilgi noktası (POı) arama ve coğrafi kodlama 'yı kurallı bir ' tek satırlı aramada ' olarak birleştirir. Örneğin, API herhangi bir adres veya POı belirteci birleşiminin girişlerini işleyebilir. Ayrıca, bağlamsal bir konum (Lat./Lonla de ağırlıklı olabilir. Çift), bir koordinat ve yarıçap tarafından tam olarak kısıtlanmış veya herhangi bir coğrafi olarak izleme bağlantı noktası olmadan daha genel olarak yürütülürler.

Çoğu arama, performansı elde etmek ve olağandışı sonuçları azaltmak için `maxFuzzyLevel=1` varsayılan olarak sorgular. Bu varsayılan, sorgu parametresi `maxFuzzyLevel=2` veya `3`geçirerek istek başına gerektiğinde geçersiz kılınabilir.

### <a name="search-for-an-address-using-fuzzy-search"></a>Benzer arama kullanarak adres arayın

1. Postman uygulamasını açın, yeni ' ye tıklayın. Yeni oluştur ve **Isteği al**' ı seçin. **Benzer arama**Için bir istek adı girin, kaydedilecek bir koleksiyon veya klasör seçin ve **Kaydet**' e tıklayın.

2. Oluşturucu sekmesinde http **Al** yöntemini SEÇIN ve API uç noktanız IÇIN istek URL 'sini girin.

    ![Benzer arama](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Yetkilendirme | Kimlik doğrulama yok |

    URL yolundaki **JSON** özniteliği yanıt biçimini belirler. Bu makalede, kullanım kolaylığı ve okunabilirlik için JSON kullanılır. Kullanılabilir yanıt biçimlerini, [haritalar işlev API 'si başvurusunun](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) **arama için belirsiz** tanımında bulabilirsiniz.

3. **Parametreler**' e tıklayın ve istek URL 'sinde sorgu veya yol parametreleri olarak kullanılacak aşağıdaki anahtar/değer çiftlerini girin:

    ![Benzer arama](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | abonelik-anahtar | Azure haritalar anahtarınızı \<\> |
    | sorgu | Pizza |

4. **Gönder** ' e tıklayın ve yanıt gövdesini gözden geçirin.

    "Pizza" için belirsiz sorgu dizesi, hem "pizza" hem de "Restoran" kategorilerinde 10 [ilgi çekici sonuç](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (POI) noktası döndürdü. Her sonuç, konum için bir sokak adresi, enlem ve boylam değerleri, görüntüleme bağlantı noktası ve giriş noktaları döndürür.
  
    Sonuçlar, belirli bir başvuru konumuna bağlı değil bu sorgu için farklılaştırmıştır. Yalnızca uygulamanızın kapsama ihtiyacı olan ülkeleri/bölgeleri belirtmek için **ülke kümesi** parametresini kullanabilirsiniz. Varsayılan davranış, büyük olasılıkla gereksiz sonuçlar döndüren dünyanın tamamında arama yapmak için kullanılır.

5. Aşağıdaki anahtar/değer çiftini **params** bölümüne ekleyin ve **Gönder**' e tıklayın:

    | Anahtar | Değer |
    |------------------|-------------------------|
    | countrySet | US |
  
    Sonuçlar artık ülke kodu ile sınırlıdır ve sorgu Birleşik Devletler.
  
    Bir konum için sonuçlar sağlamak üzere bir ilgi alanını sorgulayabilir ve benzer arama hizmeti çağrıdaki döndürülen Enlem ve boylam değerlerini kullanabilirsiniz. Bu durumda, Seattle Space Iğne 'nin konumunu döndürmek ve Lat 'yi kullanbilmeniz için arama hizmetini kullandınız. Lon. aramayı yönlendirmek için değerler.
  
6. Params ' de, aşağıdaki anahtar/değer çiftlerini girin ve **Gönder**' e tıklayın:

    ![Benzer arama](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Anahtar | Değer |
    |-----|------------|
    | lat | 47,620525 |
    | Lon | -122,349274 |


## <a name="search-for-a-street-address-using-reverse-address-search"></a>Ters adres arama kullanarak sokak adresi arama

Azure haritalar [arama adresi al ters API 'si]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) , bir koordinatı (örnek: 37,786505,-122,3862), insan tarafından anlaşılabilir bir sokak adresine çevirmenize yardımcı olur. Genellikle bu, cihazdan veya varlıktan bir GPS akışı aldığınız ve koordinatın bulunduğu adresi belirlemek istediğiniz uygulamaları izlemek için gereklidir.
Geriye doğru coğrafi koda sahip bir koordinat konumları kümesine sahipseniz, tek bir API çağrısında bir toplu sorgu göndermek için [arama sonrası geri çevirme adresi toplu API 'sini](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatch) kullanabilirsiniz.


1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **ters adres aramasını**adlandırın.

2. Oluşturucu sekmesinde http **Al** yöntemini SEÇIN ve API uç noktanız IÇIN istek URL 'sini girin.
  
    ![Ters adres arama URL 'SI](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Yetkilendirme | Kimlik doğrulama yok |
  
3. **Parametreler**' e tıklayın ve istek URL 'sinde sorgu veya yol parametreleri olarak kullanılacak aşağıdaki anahtar/değer çiftlerini girin:
  
    ![Ters adres arama parametreleri](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | abonelik-anahtar | Azure haritalar anahtarınızı \<\> |
    | sorgu | 47.591180,-122,332700 |
  
4. **Gönder** ' e tıklayın ve yanıt gövdesini gözden geçirin.

    Yanıt, Safeco alanı hakkındaki anahtar adres bilgilerini içerir.
  
5. Aşağıdaki anahtar/değer çiftini **params** bölümüne ekleyin ve **Gönder**' e tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | number | doğru |

    [Sayı](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi istekle birlikte gönderilirse, yanıt cadde (sol veya sağ) yanı sıra bu numara için de bir konum konumu içerebilir.
  
6. Aşağıdaki anahtar/değer çiftini **params** bölümüne ekleyin ve **Gönder**' e tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | returnSpeedLimit | doğru |
  
    [Returnspeedlimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi ayarlandığında, yanıt, gönderilen hız sınırını döndürür.

7. Aşağıdaki anahtar/değer çiftini **params** bölümüne ekleyin ve **Gönder**' e tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | returnRoadUse | doğru |

    [Returnroaduse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi ayarlandığında yanıt, ters coğrafi olarak açık coğrafi kodlar için yol kullanım dizisini döndürür.

8. Aşağıdaki anahtar/değer çiftini **params** bölümüne ekleyin ve **Gönder**' e tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | yol kullanımı | doğru |

    Yol [kullanımı](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresini kullanarak ters coğrafi kod sorgusunu belirli bir yol türüyle kısıtlayabilirsiniz.
  
## <a name="search-for-cross-street-using-reverse-address-cross-street-search"></a>Ters adres çapraz arama kullanarak çapraz cadde arama

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **çapraz adres çapraz aramasını ters adrese**adlandırın.

2. Oluşturucu sekmesinde http **Al** yöntemini SEÇIN ve API uç noktanız IÇIN istek URL 'sini girin.
  
    ![Ters adres çapraz arama](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Yetkilendirme | Kimlik doğrulama yok |
  
3. **Parametreler**' e tıklayın ve istek URL 'sinde sorgu veya yol parametreleri olarak kullanılacak aşağıdaki anahtar/değer çiftlerini girin:
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | abonelik-anahtar | Azure haritalar anahtarınızı \<\> |
    | sorgu | 47.591180,-122,332700 |
  
4. **Gönder** ' e tıklayın ve yanıt gövdesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure haritalar arama hizmeti](https://docs.microsoft.com/rest/api/maps/search) API 'si belgelerini inceleyin.
- [En iyi yöntemler](https://docs.microsoft.com/azure/azure-maps/how-to-use-best-practices-for-search)hakkında bilgi edinin.