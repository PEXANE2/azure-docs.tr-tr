---
title: Azure haritalar 'ı kullanarak konumlar arayın Arama Hizmeti | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Arama Hizmeti kullanarak bir konumun nasıl aranalınacağını öğreneceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 53856b4157afa5976947c451952fc26eefcdd0ea
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264195"
---
# <a name="find-an-address-using-the-azure-maps-search-service"></a>Azure haritalar arama hizmetini kullanarak bir adres bulma

Haritalar arama hizmeti, geliştiriciler için tasarlanmış bir dizi yeniden API 'dir. Hizmet adresleri, yerleri, ilgi çekici noktaları, iş listelerini ve diğer coğrafi bilgileri arayabilir. Aşağıdakilerin her birinin bir enlem ve boylam değerleri vardır: belirli bir adres, çapraz cadde, coğrafi özellik veya bir ilgi noktası (POı). Bir sorgudaki döndürülen Enlem ve boylam değerlerini diğer harita hizmetlerinde parametre olarak kullanabilirsiniz. Örneğin, döndürülen değerler rota hizmeti veya trafik akışı hizmeti için parametre olabilir. 

Öğrenelim:

* [Benzer arama API 'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) kullanarak adres arayın
* Özellikler ve koordinatlarla birlikte bir adres arayın
* Sokak adresini aramak için [ters adres araması](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) yapın
* [Arama adresi ters açık çapraz API 'yi](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet) kullanarak çapraz cadde arama

## <a name="prerequisites"></a>Ön koşullar

Haritalar hizmeti API 'Lerine herhangi bir çağrı yapmak için bir haritalar hesabı ve bir anahtar gerekir. Azure haritalar için bir hesap [oluşturmak için hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps)konusundaki yönergeleri izleyin. Birincil anahtarınızı almak için yardıma ihtiyacınız varsa, [birincil anahtarı al](quick-demo-map-app.md#get-the-primary-key-for-your-account)bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.

## <a name="using-fuzzy-search"></a>Benzer arama kullanma

Arama hizmeti için varsayılan API, benzer bir [aramadır](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Bu hizmet, bir arama sorgusunda Kullanıcı giriş biçiminden emin değilseniz yararlıdır. API, POı aramasını ve coğrafi kodlamayı kurallı bir ' tek satırlık arama ' olarak birleştirir. Örneğin, API herhangi bir adres veya POı belirteci birleşiminin girişlerini işleyebilir. Ayrıca, bağlamsal bir konum (Enlem./Lonla ağırlıklı olabilir. Çift), bir koordinat ve yarıçap tarafından tam olarak kısıtlanmış veya herhangi bir coğrafi olarak izleme bağlantı noktası olmadan daha genel olarak yürütülürler.

Çoğu arama, performansı elde etmek ve olağandışı sonuçları azaltmak için `maxFuzzyLevel=1` varsayılan olarak sorgular. Bu varsayılan, sorgu parametresi `maxFuzzyLevel=2` veya `3`geçirerek istek başına gerektiğinde geçersiz kılınabilir.

### <a name="search-for-an-address-using-fuzzy-search"></a>Benzer arama kullanarak adres arayın

1. Postman uygulamasını açın, yeni ' ye tıklayın. Yeni oluştur ve **Isteği al**' ı seçin. **Benzer arama**Için bir istek adı girin, kaydedilecek bir koleksiyon veya klasör seçin ve **Kaydet**' e tıklayın.

2. Oluşturucu sekmesinde http **Al** yöntemini SEÇIN ve API uç noktanız IÇIN istek URL 'sini girin.

    ![Benzer arama](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Yetkilendirme | Kimlik Doğrulama Yok |

    URL yolundaki **JSON** özniteliği yanıt biçimini belirler. Bu makalede, kullanım kolaylığı ve okunabilirlik için JSON kullanılır. Kullanılabilir yanıt biçimlerini, [haritalar işlev API 'si başvurusunun](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) **arama için belirsiz** tanımında bulabilirsiniz.

3. **Parametreler**' e tıklayın ve istek URL 'sinde sorgu veya yol parametreleri olarak kullanılacak aşağıdaki anahtar/değer çiftlerini girin:

    ![Benzer arama](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | abonelik-anahtar | Azure haritalar anahtarınızı \<\> |
    | sorgu | pizza |

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

## <a name="search-for-address-properties-and-coordinates"></a>Adres özelliklerini ve koordinatları ara

Arama adresi API 'sine tamamen veya kısmi sokak adresi geçirebilirsiniz. Ayrıntılı adres özelliklerini içeren bir yanıt almaya devam edersiniz. Ayrıntılı adres özellikleri, yükseklik ve boylam, municipsellik veya alt bölüm içindeki konumsal değerler gibi değerlerdir.

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **Adres aramasını**adlandırın.
2. Oluşturucu sekmesinde http **Al** metodunu SEÇIN, API uç noktanız IÇIN istek URL 'sini girin ve varsa bir Yetkilendirme Protokolü seçin.

    ![Adres arama](./media/how-to-search-for-address/address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) |
    | Yetkilendirme | Kimlik Doğrulama Yok |

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

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Ters adres arama kullanarak sokak adresi arama

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **ters adres aramasını**adlandırın.

2. Oluşturucu sekmesinde http **Al** yöntemini SEÇIN ve API uç noktanız IÇIN istek URL 'sini girin.
  
    ![Ters adres arama URL 'SI](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Yetkilendirme | Kimlik Doğrulama Yok |
  
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

    Ters coğrafi kod sorgusunu belirli bir yol türüyle kısıtlayabilirsiniz, [roaduse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresini kullanın.
  
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Ters adres çapraz arama kullanarak çapraz cadde arama

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **çapraz adres çapraz aramasını ters adrese**adlandırın.

2. Oluşturucu sekmesinde http **Al** yöntemini SEÇIN ve API uç noktanız IÇIN istek URL 'sini girin.
  
    ![Ters adres çapraz arama](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Yetkilendirme | Kimlik Doğrulama Yok |
  
3. **Parametreler**' e tıklayın ve istek URL 'sinde sorgu veya yol parametreleri olarak kullanılacak aşağıdaki anahtar/değer çiftlerini girin:
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | abonelik-anahtar | Azure haritalar anahtarınızı \<\> |
    | sorgu | 47.591180,-122,332700 |
  
4. **Gönder** ' e tıklayın ve yanıt gövdesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure haritalar arama hizmeti](https://docs.microsoft.com/rest/api/maps/search) API 'si belgelerini inceleyin.
