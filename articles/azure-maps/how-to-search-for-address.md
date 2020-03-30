---
title: Azure Haritalar Arama hizmetlerini kullanarak konum arama | Microsoft Azure Haritaları
description: Bu makalede, coğrafi kodlama ve ters coğrafi kodlama için Microsoft Azure Haritalar Arama Hizmeti'ni kullanarak bir konum aramayapmayı öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: cf0e5267885df1ace51271c53bb2d68ee5002f00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335424"
---
# <a name="search-for-a-location-using-azure-maps-search-services"></a>Azure Haritalar Arama hizmetlerini kullanarak konum arama

Azure Haritalar [Arama Hizmeti,](https://docs.microsoft.com/rest/api/maps/search) geliştiricilerin adresleri, yerleri, işletme girişlerini ada veya kategoriye göre ve diğer coğrafi bilgileri aramalarına yardımcı olmak üzere tasarlanmış bir leştirilmiş API'dir. Geleneksel coğrafi kodlamayı desteklemenin yanı sıra, hizmetler enlem ve boylamlara göre coğrafi kod adreslerini ve çapraz caddeleri tersine çevirebilir. Arama tarafından döndürülen enlem ve boylam [değerleri, Rota](https://docs.microsoft.com/rest/api/maps/route) ve [Hava Durumu](https://docs.microsoft.com/rest/api/maps/weather) hizmetleri gibi diğer Azure Haritalar hizmetlerinde parametre olarak kullanılabilir.

Bu makalede, nasıl öğreneceksiniz:

* [Arama Adresi API'sini]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) kullanarak bir adres (coğrafi kod adres konumu) için enlem ve boylam koordinatları isteyin
* [Bulanık Arama API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) kullanarak bir adres veya İlgi Alanı (İçN) arama
* Özellikler ve koordinatlarla birlikte bir adres arama
* Koordinat konumunu sokak adresine çevirmek için [Ters Adres Araması](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) yapma
* Arama Adresi Ters [Cross Street API'yi](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet) kullanarak bir çapraz sokak arama

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için öncelikle bir Azure Haritalar hesabı oluşturmanız ve hesap abonelik anahtarını eşlemenizi almanız gerekir. Azure Haritalar hesabı aboneliği oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin ve hesabınızın birincil anahtarını almak için [birincil anahtarı al](quick-demo-map-app.md#get-the-primary-key-for-your-account) adımlarını izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.

Bu makalede, REST aramaları oluşturmak için [Postacı uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.

## <a name="request-latitude-and-longitude-for-an-address-geocoding"></a>Bir adres için enlem ve boylam isteği (coğrafi kodlama)

Bu örnekte, sokak adresini enlem ve boylam koordinatlarına dönüştürmek için Azure Haritalar [Arama Adresi API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) kullanıyoruz. Tam veya kısmi bir sokak adresini API'ye geçirebilir ve sokak, posta kodu ve ülke/bölge gibi ayrıntılı adres özelliklerinin yanı sıra enlem ve boylamdaki konumsal değerleri içeren bir yanıt alabilirsiniz.

Coğrafi kodda bir adres kümesiniz varsa, tek bir API çağrısında bir yığın sorgu göndermek için [Arama Adresi Toplu EkI'yi](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatch) kullanabilirsiniz.

1. Postacı'da **Yeni İstek** | **GET isteğini** tıklatın ve adres **arama**adını alın.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin, API bitiş noktanız için istek URL'sini girin ve varsa bir yetkilendirme protokolü seçin.

![Adres Arama](./media/how-to-search-for-address/address_search_url.png)

| Parametre | Önerilen değer |
|---------------|------------------------------------------------| 
| HTTP yöntemi | GET |
| İstek URL'si | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) | 
| Yetkilendirme | Hayır Auth |

3. **Param'ları**tıklatın ve istek URL'sinde sorgu veya yol parametreleri olarak kullanmak üzere aşağıdaki Anahtar / Değer çiftlerini girin: 

![Adres Arama](./media/how-to-search-for-address/address_search_params.png) 

| Anahtar | Değer | 
|------------------|-------------------------| 
| api-sürümü | 1.0 | 
| abonelik anahtarı | \<Azure Haritalar anahtarınız\> | 
| sorgu | 400 Broad St, Seattle, WA 98109 | 

4. **Gönder'i** tıklatın ve yanıt gövdesini gözden geçirin. 

Bu durumda, tam bir adres sorgusu belirtin ve yanıt gövdesinde tek bir sonuç alırsınız. 

5. Param'larda sorgu dizesini aşağıdaki değere göre edin: 

    ```plaintext 
        400 Broad, Seattle 
    ``` 

6. **Params** bölümüne aşağıdaki Anahtar / Değer çiftini ekleyin ve **Gönder'e**tıklayın: 

| Anahtar | Değer | 
|-----|------------| 
| typeahead | true | 

**İlerideki** bayrak, Adres Arama API'sine sorguyu kısmi bir giriş olarak ele alması ve bir dizi tahmine dayalı değer döndürmesini söyler.

## <a name="using-fuzzy-search-api"></a>Bulanık Arama API'si kullanma

Azure Haritalar[ Bulanık Arama API'si,](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) kullanıcı girişlerinizin bir arama sorgusu için ne olduğunu bilmediğiniz durumlarda kullanılması önerilir. API, İlgi Alanı (POI) aramave coğrafi kodlamayı kanonik bir 'tek satırlı arama' olarak birleştirir. Örneğin, API herhangi bir adres veya POI belirteç kombinasyonu girişleri işleyebilir. Ayrıca bağlamsal bir konum (lat./lon) ile ağırlıklandırılabilir. çifti), koordinat ve yarıçap tarafından tamamen sınırlandırılmış veya herhangi bir coğrafi yanlı bağlantı noktası olmadan daha genel olarak yürütülür.

Çoğu Arama sorgusu, `maxFuzzyLevel=1` performans kazanmak ve olağandışı sonuçları azaltmak için varsayılan olarak sorgular. Bu varsayılan sorgu parametre `maxFuzzyLevel=2` veya `3`geçerek istek başına gerektiği gibi geçersiz kılınabilir.

### <a name="search-for-an-address-using-fuzzy-search"></a>Bulanık Arama'yı kullanarak adres arama

1. Postacı uygulamasını açın, Yeni'yi tıklatın | Yeni oluşturun ve **İstek Al'ı**seçin. **Bulanık aramanın**İstek adını girin, kaydetmek için bir koleksiyon veya klasör seçin ve **Kaydet'i**tıklatın.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin ve API bitiş noktanızın istek URL'sini girin.

    ![Bulanık Arama](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Yetkilendirme | Hayır Auth |

    URL yolundaki **json** özniteliği yanıt biçimini belirler. Bu makalede kullanım kolaylığı ve okunabilirlik için json kullanır. Kullanılabilir yanıt biçimlerini [Haritalar İşlevsel API referansının](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) **Arama Bulanıkını Al** tanımında bulabilirsiniz.

3. **Param'ları**tıklatın ve istek URL'sinde sorgu veya yol parametreleri olarak kullanmak üzere aşağıdaki Anahtar / Değer çiftlerini girin:

    ![Bulanık Arama](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-sürümü | 1.0 |
    | abonelik anahtarı | \<Azure Haritalar anahtarınız\> |
    | sorgu | Pizza |

4. **Gönder'i** tıklatın ve yanıt gövdesini gözden geçirin.

    "Pizza" için belirsiz sorgu dizesi hem "pizza" hem de "restoran" kategorilerinde 10 [noktalık ilgi sonucunu](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (POI) döndürdü. Her sonuç bir sokak adresi, enlem ve boylam değerleri, görünüm bağlantı noktası ve konum için giriş noktaları döndürür.
  
    Sonuçlar, belirli bir başvuru konumuna bağlı değil, bu sorgu için çeşitlidir. Yalnızca uygulamanızın kapsama alanına ihtiyaç duyduğu ülkeleri/bölgeleri belirtmek için **ülkeSet** parametresini kullanabilirsiniz. Varsayılan davranış, tüm dünyayı aramak ve gereksiz sonuçları geri döndürmektir.

5. **Params** bölümüne aşağıdaki Anahtar / Değer çiftini ekleyin ve **Gönder'e**tıklayın:

    | Anahtar | Değer |
    |------------------|-------------------------|
    | ülkeSet | ABD |
  
    Sonuçlar artık ülke koduyla sınırlanır ve sorgu ABD'deki pizza restoranlarını döndürür.
  
    Bir konum için sonuç sağlamak için, ilgi çekici bir noktayı sorgulayabilir ve Bulanık Arama hizmetine yaptığınız çağrıda döndürülen enlem ve boylam değerlerini kullanabilirsiniz. Bu durumda, Seattle Space Needle konumunu döndürmek için Arama hizmetini kullandınız ve lat'ı kullandınız. / lon. aramayı yönlendirmek için değerler.
  
6. Params'ta aşağıdaki Anahtar / Değer çiftlerini girin ve **Gönder'i**tıklatın:

    ![Bulanık Arama](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Anahtar | Değer |
    |-----|------------|
    | Lat | 47.620525 |
    | Lon | -122.349274 |


## <a name="search-for-a-street-address-using-reverse-address-search"></a>Ters Adres Arama'yı kullanarak sokak adresini arama

Azure Haritalar [Arama Adresi Ters API alın,]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) koordinatı (örneğin: 37.786505, -122.3862) insan tarafından anlaşılabilir bir sokak adresine çevirmeye yardımcı olur. Çoğu zaman bu cihaz veya varlık bir GPS beslemesi almak ve koordinat nerede olduğunu bilmek isteyen uygulamaları izleme gereklidir.
Coğrafi kodu tersine çevirmek için bir dizi koordinat konumların varsa, tek bir API çağrısında bir yığın sorgu göndermek için [Arama Adresi Ters Toplu Eki'yi](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatch) kullanabilirsiniz.


1. Postacı'da **Yeni İstek** | **GET isteğini** tıklatın ve ters adres **arama**adını alın.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin ve API bitiş noktanızın istek URL'sini girin.
  
    ![Ters Adres Arama URL'si](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Yetkilendirme | Hayır Auth |
  
3. **Param'ları**tıklatın ve istek URL'sinde sorgu veya yol parametreleri olarak kullanmak üzere aşağıdaki Anahtar / Değer çiftlerini girin:
  
    ![Ters Adres Arama Parametreleri](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-sürümü | 1.0 |
    | abonelik anahtarı | \<Azure Haritalar anahtarınız\> |
    | sorgu | 47.591180,-122.332700 |
  
4. **Gönder'i** tıklatın ve yanıt gövdesini gözden geçirin.

    Yanıt, Safeco Field ile ilgili anahtar adres bilgilerini içerir.
  
5. **Params** bölümüne aşağıdaki Anahtar / Değer çiftini ekleyin ve **Gönder'e**tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | number | true |

    [İstekle](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) birlikte numara sorgu parametresi gönderilirse, yanıt sokağın yan tarafını (Sol veya Sağ) ve bu numara için bir ofset konumunu da içerebilir.
  
6. **Params** bölümüne aşağıdaki Anahtar / Değer çiftini ekleyin ve **Gönder'e**tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | returnSpeedLimit | true |
  
    [ReturnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi ayarlandığında, yanıt deftere nakledilen hız sınırını döndürür.

7. **Params** bölümüne aşağıdaki Anahtar / Değer çiftini ekleyin ve **Gönder'e**tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | returnRoadUse | true |

    [ReturnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi ayarlandığında, yanıt sokak düzeyinde ters coğrafi kodlar için yol kullanım dizisini döndürür.

8. **Params** bölümüne aşağıdaki Anahtar / Değer çiftini ekleyin ve **Gönder'e**tıklayın:

    | Anahtar | Değer |
    |-----|------------|
    | yolKullan | true |

    [RoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresini kullanarak ters coğrafi kod sorgusunu belirli bir yol türüyle sınırlandırabilirsiniz.
  
## <a name="search-for-cross-street-using-reverse-address-cross-street-search"></a>Ters Adres Cross Street Arama'yı kullanarak çapraz sokak arama

1. Postacı'da, **Yeni İstek** | **GET isteğini** tıklatın ve ters adres Cross **Street Arama**adını.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin ve API bitiş noktanızın istek URL'sini girin.
  
    ![Ters Adres Cross Street Arama](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Yetkilendirme | Hayır Auth |
  
3. **Param'ları**tıklatın ve istek URL'sinde sorgu veya yol parametreleri olarak kullanmak üzere aşağıdaki Anahtar / Değer çiftlerini girin:
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | api-sürümü | 1.0 |
    | abonelik anahtarı | \<Azure Haritalar anahtarınız\> |
    | sorgu | 47.591180,-122.332700 |
  
4. **Gönder'i** tıklatın ve yanıt gövdesini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Haritalar Arama Hizmeti REST API belgelerini](https://docs.microsoft.com/rest/api/maps/search)keşfedin.
- Azure [Haritalar Arama Hizmeti en iyi uygulamaları](https://docs.microsoft.com/azure/azure-maps/how-to-use-best-practices-for-search) ve sorgularınızı nasıl optimize edebilirsiniz hakkında bilgi edinin.
