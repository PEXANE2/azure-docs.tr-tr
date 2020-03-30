---
title: Azure Haritalar Arama Hizmeti için en iyi uygulamalar | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar'daki Arama Hizmetini kullanırken en iyi uygulamaları nasıl uygulayacağınızı öğrenin.
author: philmea
ms.author: philmea
ms.date: 01/23/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8d62d7d278323baa0ae49b9e12f46468efb067a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335317"
---
# <a name="best-practices-for-azure-maps-search-service"></a>Azure Haritalar Arama Hizmeti için en iyi uygulamalar

Azure Haritalar [Arama Hizmeti,](https://docs.microsoft.com/rest/api/maps/search) çeşitli özellikler sunan API'ler içerir. Örneğin, Arama Adresi API'si ilgi çekici noktaları (İçN) veya belirli bir konum la ilgili verileri bulabilir. 

Bu makalede, Azure Haritalar Arama Hizmeti'nden veri aradığınızda ses uygulamalarının nasıl uygulanacağı açıklanmaktadır. Şunları öğrenirsiniz:

* İlgili eşleşmeleri döndürmek için sorgular oluşturun.
* Arama sonuçlarını sınırlandırın.
* Sonuç türleri arasındaki farkları öğrenin.
* Adres arama yanıt yapısını okuyun.

## <a name="prerequisites"></a>Ön koşullar

Azure Haritalar hizmet API'lerine arama yapmak için bir Azure Haritalar hesabına ve bir anahtara ihtiyacınız var. Daha fazla bilgi için [bkz.](quick-demo-map-app.md#create-an-account-with-azure-maps) [Get a primary key](quick-demo-map-app.md#get-the-primary-key-for-your-account) 

Azure Haritalar'da kimlik doğrulama hakkında bilgi için azure [haritalarda kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.

> [!TIP]
> Arama Hizmeti'ni sorgulamak [için,](https://www.getpostman.com/apps) REST aramaları oluşturmak için Postacı uygulamasını kullanabilirsiniz. Veya tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.

## <a name="best-practices-to-geocode-addresses"></a>Coğrafi kod adreslerine en iyi uygulamalar

Azure Haritalar Arama Hizmeti'ni kullanarak tam veya kısmi bir adres aradığınızda, API arama sorgunuzdaki anahtar kelimeleri okur. Daha sonra adresin boylam ve enlem koordinatlarını döndürür. Bu işleme *coğrafi kodlama*denir. 

Bir ülkede coğrafi kod alabilme yeteneği, yol verilerinin kullanılabilirliğine ve coğrafi kodlama hizmetinin hassasiyetine bağlıdır. Ülkeye veya bölgeye göre Azure Haritalar coğrafi kodlama özellikleri hakkında daha fazla bilgi için [Bkz.](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage)

### <a name="limit-search-results"></a>Arama sonuçlarını sınırlama

 Azure Haritalar Arama API'si, arama sonuçlarını uygun şekilde sınırlamanıza yardımcı olabilir. Kullanıcılarınızın ilgili verilerini görüntüleyebilmeniz için sonuçları sınırlandırın.

> [!NOTE]
> Arama API'leri, bu makalede tartışılan parametrelerden daha fazla parametreyi destekler.

#### <a name="geobiased-search-results"></a>Coğrafi taraflı arama sonuçları

Geobias sonuçları kullanıcınız için ilgili alana eklemek için, her zaman mümkün olduğunca çok sayıda konum bilgisi ekleyin. Bazı giriş türlerini belirterek arama sonuçlarını kısıtlamak isteyebilirsiniz:

* Parametreyi `countrySet` ayarlayın. `US,FR`Örneğin, bunu ayarlayabilirsiniz. Varsayılan olarak, API tüm dünyayı arar, böylece gereksiz sonuçlar döndürebilir. Sorgunuzun parametresi yoksa, `countrySet` arama yanlış sonuçlar döndürebilir. Örneğin, *Bellevue* adlı bir şehir için yapılan arama, her iki ülkede de *Bellevue*adında bir şehir içerdiğinden ABD ve Fransa'dan sonuçları döndürür.

* Sınırlayıcı `btmRight` kutuyu `topleft` ayarlamak için parametreleri ve parametreleri kullanabilirsiniz. Bu parametreler aramayı haritadaki belirli bir alanla sınırlandırın.

* Sonuçlar için alaka alanını etkilemek için `lat` parametreleri tanımlayın ve `lon` koordine edin. Arama `radius` alanının yarıçapını ayarlamak için parametreyi kullanın.


#### <a name="fuzzy-search-parameters"></a>Bulanık arama parametreleri

Bir arama sorgusu için kullanıcı girişlerinizi bilmiyorsanız Azure Haritalar [Arama Bulanık API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) kullanmanızı öneririz. API, POI arama ve coğrafi kodlamayı kanonik *tek satırlı bir aramada*birleştirir: 

* Ve `minFuzzyLevel` `maxFuzzyLevel` parametreler, sorgu parametreleri kullanıcının istediği bilgilerle tam olarak eşleşmese bile ilgili eşleşmeleri döndürmeye yardımcı olur. Performansı en üst düzeye çıkarmak ve olağandışı sonuçları `minFuzzyLevel=1` `maxFuzzyLevel=2`azaltmak için, arama sorgularını varsayılan olarak ve . 

    Örneğin, `maxFuzzyLevel` parametre 2 olarak ayarlandığında, arama terimi *restrant* *restoranla*eşleşir. Gerektiğinde varsayılan bulanık düzeyleri geçersiz kılabilirsiniz. 

* Sonuç `idxSet` türlerinin tam kümesine öncelik vermek için parametreyi kullanın. Tam bir sonuç kümesine öncelik vermek için, virgülle ayrılmış dizinler listesi gönderebilirsiniz. Listenizde, madde sırası önemli değil. Azure Haritalar aşağıdaki dizinleri destekler:

* `Addr` - **Adres aralıkları**: Caddenin başından ve sonundan enterpolasyonyapılan adres noktaları. Bu noktalar adres aralıkları olarak temsil edilir.
* `Geo` - **Coğrafyalar**: Arazinin idari bölümleri. Bir coğrafya bir ülke, devlet veya şehir olabilir, örneğin.
* `PAD` - **Nokta adresleri**: Sokak adı ve numarası içeren adresler. Nokta adresleri bir dizinde bulunabilir. Bir örnek *Soquel Dr 2501*olduğunu. Bir nokta adresi, adresler için kullanılabilen en yüksek doğruluk düzeyini sağlar.  
* `POI` - **İlgi çekici noktalar**: Dikkat edilmesi gereken veya ilginç olabilecek bir haritaüzerindeki noktalar. [Arama Adresi API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) İçN'leri döndürmez.  
* `Str` - **Sokaklar**: Haritadaki sokaklar.
* `XStr` - **Çapraz sokaklar veya kavşaklar**: Kavşaklar veya iki caddenin kesiştiği yerler.


#### <a name="usage-examples"></a>Kullanım örnekleri

* `idxSet=POI`- Yalnızca İçN'leri arayın. 

* `idxSet=PAD,Addr`- Yalnızca arama adresleri. `PAD`nokta adresini gösterir `Addr` ve adres aralığını gösterir.

### <a name="reverse-geocode-and-filter-for-a-geography-entity-type"></a>Bir coğrafya varlık türü için ters geocode ve filtre

[Arama Adresi Ters API'sinde](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)ters coğrafi kod araması yaptığınızda, hizmet yönetim alanları için çokgenleri döndürebilir.Aramayı belirli coğrafya varlık türlerine daraltmak için isteklerinize `entityType` parametre ekleyin. 

Elde edilen yanıt, coğrafya kimliğini ve eşleşen varlık türünü içerir. Birden fazla varlık sağlarsanız, bitiş noktası *kullanılabilir en küçük varlığı*döndürür. Geri dönen geometri kimliğini kullanarak coğrafyanın geometrisini [Arama Çokgen hizmeti](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon)aracılığıyla elde edebilirsiniz.

#### <a name="sample-request"></a>Örnek istek

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

#### <a name="response"></a>Yanıt

```JSON
{
    "summary": {
        "queryTime": 14,
        "numResults": 1
    },
    "addresses": [
        {
            "address": {
                "routeNumbers": [],
                "countryCode": "US",
                "countrySubdivision": "WA",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "municipality": "Redmond",
                "country": "United States",
                "countryCodeISO3": "USA",
                "countrySubdivisionName": "Washington"
            },
            "position": "47.639454,-122.130455",
            "dataSources": {
                "geometry": {
                    "id": "00005557-4100-3c00-0000-0000596ae571"
                }
            },
            "entityType": "Municipality"
        }
    ]
}
```

### <a name="set-the-results-language"></a>Sonuç dilini ayarlama

Döndürülen `language` arama sonuçlarının dilini ayarlamak için parametreyi kullanın. İstek dili ayarlamıyorsa, varsayılan olarak Arama Hizmeti ülke veya bölgedeki en yaygın dili kullanır. Belirtilen dilde veri yoksa varsayılan dil kullanılır. 

Daha fazla bilgi için Azure [Haritalar desteklenen dillere](https://docs.microsoft.com/azure/azure-maps/supported-languages)bakın.


### <a name="use-predictive-mode-automatic-suggestions"></a>Tahmin modunu kullanma (otomatik öneriler)

Kısmi sorgular için daha fazla `typeahead` eşleşme bulmak `true`için parametreyi ' ye ayarlayın Bu sorgu kısmi giriş olarak yorumlanır ve arama tahmin moduna girer. Parametreyi `typeahead` `true`ayarlamazsanız, hizmet ilgili tüm bilgilerin aktarıldığını varsayar.

Aşağıdaki örnek sorguda, Arama Adresi hizmeti *Microso*için sorgulanır. Burada, `typeahead` parametre `true`ayarlanır. Yanıt, arama hizmetinin sorguyu kısmi sorgu olarak yorumladığını gösterir. Yanıt, otomatik olarak önerilen bir sorgunun sonuçlarını içerir.

#### <a name="sample-query"></a>Örnek sorgu

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1.0&typeahead=true&countrySet=US&lat=47.6370891183&lon=-122.123736172&query=Microsoft
```

#### <a name="response"></a>Yanıt

```JSON
{
    "summary": {
        "query": "microsoft",
        "queryType": "NON_NEAR",
        "queryTime": 18,
        "numResults": 7,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/9438784",
            "score": 2.594099998474121,
            "dist": 314.0590106663596,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
            },
            "position": {
                "lat": 47.63988,
                "lon": -122.12438
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64223,
                    "lon": -122.1256,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1756074",
            "score": 2.592679977416992,
            "dist": 876.0272035824189,
            "address": {
                "streetName": "Microsoft Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64032,
                "lon": -122.1344
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64253,
                    "lon": -122.13535,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63816,
                    "lon": -122.13305,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1470668",
            "score": 2.5290400981903076,
            "dist": 2735.4883918101486,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Bellevue, WA 98007"
            },
            "position": {
                "lat": 47.65784,
                "lon": -122.14335
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.65785,
                    "lon": -122.14335,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65784,
                    "lon": -122.14325,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/12812615",
            "score": 2.527509927749634,
            "dist": 2870.9579016916873,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.66034,
                "lon": -122.1404
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.66039,
                    "lon": -122.14325,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65778,
                    "lon": -122.13749,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/197588",
            "score": 2.4630401134490967,
            "dist": 878.1404663812472,
            "address": {
                "streetName": "157th Avenue Northeast",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "extendedPostalCode": "980525344, 980525398, 980525399",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "157th Avenue Northeast, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64351,
                "lon": -122.13056
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64473,
                    "lon": -122.13058,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.6425,
                    "lon": -122.13016,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/3033991",
            "score": 2.0754499435424805,
            "dist": 3655467.8844475765,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
            },
            "position": {
                "lat": 35.14267,
                "lon": -80.91824
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14287,
                    "lon": -80.91839,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8395877",
            "score": 2.0754499435424805,
            "dist": 3655437.0037482483,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "countrySubdivisionName": "North Carolina",
                "postalCode": "28273",
                "extendedPostalCode": "282738105, 282738106, 282738108, 2827382, 282738200",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28273"
            },
            "position": {
                "lat": 35.14134,
                "lon": -80.9198
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14274,
                    "lon": -80.92159,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14002,
                    "lon": -80.91824,
                    "valid": true
                }
            }
        }
    ]
}
```


### <a name="encode-a-uri-to-handle-special-characters"></a>Özel karakterleri işlemek için bir URI kodlama 

Cadde ler arası adresleri bulmak için, adresteki özel karakterleri işlemek için URI'yi kodlamanız gerekir. Bu adres örneğini göz önünde bulundurun: *1st Avenue & Union Street, Seattle.* Burada, isteği göndermeden önce`&`amper ve karakteri kodlayın. 

Karakter verilerini uri'de kodlamanızı öneririz. URI'de, tüm karakterleri bir yüzde işareti (`%`) ve karakterlerin UTF-8 koduna karşılık gelen iki karakterli hexadecimal değeri kullanarak kodlarsınız.

#### <a name="usage-examples"></a>Kullanım örnekleri

Bu adresle başlayın:

```
query=1st Avenue & E 111th St, New York
```

Adresi kodlayın:

```
query=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```

Aşağıdaki yöntemleri kullanabilirsiniz.

JavaScript veya TypeScript:
```Javascript
encodeURIComponent(query)
```

C# veya Visual Basic:
```csharp
Uri.EscapeDataString(query)
```

Java:
```Java
URLEncoder.encode(query, "UTF-8") 
```

Python:
```Python
import urllib.parse 
urllib.parse.quote(query)
```

C++:
```C++
#include <curl/curl.h>
curl_easy_escape(query)
```

Php:
```PHP
urlencode(query)
```

Yakut:
```Ruby
CGI::escape(query) 
```

Swift:
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Devam et:
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-searching"></a>İçN arama için en iyi uygulamalar

İçN aramasında, İçN sonuçlarını ada göre isteyebilirsiniz. Örneğin, bir işletmeyi ada göre arayabilirsiniz. 

Uygulamanızın kapsama ihtiyacı olduğu `countrySet` ülkeleri belirtmek için parametreyi kullanmanızı şiddetle öneririz. Varsayılan davranış tüm dünyayı aramaktır. Bu geniş arama gereksiz sonuçlar döndürebilir ve arama uzun sürebilir.

### <a name="brand-search"></a>Marka arama

Sonuçların ve yanıttaki bilgilerin alaka düzeyini artırmak için, bir İçN arama yanıtı marka bilgilerini içerir. Yanıtı ayrışdırmak için bu bilgileri kullanabilirsiniz.

İstekte, virgülle ayrılmış bir marka adı listesi gönderebilirsiniz. Parametreyi ayarlayarak sonuçları belirli markalarla `brandSet` sınırlamak için listeyi kullanın. Listenizde, madde sırası önemli değildir. Birden çok marka listesi sağladığınızda, döndürülen sonuçlar listelerinizden en az birine ait olmalıdır.

Marka aramasını keşfetmek için bir İçN kategorisi arama isteği [yapalım.](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) Aşağıdaki örnekte, Redmond, Washington'daki Microsoft kampüsünün yakınında benzin istasyonları arıyoruz. Yanıt, döndürülen her İçN'nin marka bilgilerini gösterir.

#### <a name="sample-query"></a>Örnek sorgu

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=gas%20station&limit=3&lat=47.6413362&lon=-122.1327968
```

#### <a name="response"></a>Yanıt

```JSON
{
    "summary": {
        "query": "gas station",
        "queryType": "NON_NEAR",
        "queryTime": 276,
        "numResults": 3,
        "offset": 0,
        "totalResults": 762680,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/8831765",
            "score": 5.6631999015808105,
            "dist": 1037.0280221303253,
            "info": "search:ta:840531000004190-US",
            "poi": {
                "name": "Chevron",
                "phone": "+(1)-(425)-6532200",
                "brands": [
                    {
                        "name": "Chevron"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.chevron.com",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2444",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Northeast Bellevue, Bellevue",
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63222,
                        "lon": -122.13312,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831752",
            "score": 5.662710189819336,
            "dist": 1330.1278248163273,
            "info": "search:ta:840539001100326-US",
            "poi": {
                "name": "76",
                "phone": "+(1)-(425)-7472126",
                "brands": [
                    {
                        "name": "76"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.76.com",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2421",
                "streetName": "148Th Ave Ne",
                "municipalitySubdivision": "Redmond, Bridle Trails, Bellevue",
                "municipality": "Redmond, Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "2421 148Th Ave Ne, Bellevue, WA 98007",
                "localName": "Bellevue"
            },
            "position": {
                "lat": 47.63187,
                "lon": -122.14365
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63277,
                    "lon": -122.14498,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63097,
                    "lon": -122.14232,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "minor",
                    "position": {
                        "lat": 47.63187,
                        "lon": -122.14374,
                        "valid": true
                    }
                },
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63186,
                        "lon": -122.14313,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831764",
            "score": 5.662449836730957,
            "dist": 1458.645407416307,
            "info": "search:ta:840539000488527-US",
            "poi": {
                "name": "BROWN BEAR CAR WASH",
                "phone": "+(1)-(425)-6442868",
                "brands": [
                    {
                        "name": "Texaco"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.texaco.com/",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "15248",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Redmond",
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62827,
                        "lon": -122.13628,
                        "valid": true
                    }
                }
            ]
        }
    ]
}
```


### <a name="airport-search"></a>Havaalanı arama

Arama İçi API'sını kullanarak, havaalanlarını resmi kodlarını kullanarak arayabilirsiniz. Örneğin, Seattle-Tacoma Uluslararası Havaalanı'nı bulmak için *SEA'yi* kullanabilirsiniz: 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>Yakındaki arama

Belirli bir konum etrafında İçN sonuçlarını almak için [Yakındaki Arama API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby)kullanmayı deneyebilirsiniz. Bitiş noktası yalnızca PoI sonuçlarını döndürür. Bir arama sorgusu parametre almaz. 

Sonuçları sınırlamak için yarıçapı ayarlamanızı öneririz.

## <a name="understanding-the-responses"></a>Yanıtları anlama

Azure Haritalar Arama Hizmeti'ne adres arama isteğinde bulunarak Seattle'da bir adres bulalım. Aşağıdaki istek URL'sinde, `countrySet` abd'deki `US` adresi aramak için parametreyi ayarladık.

### <a name="sample-query"></a>Örnek sorgu

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

### <a name="supported-types-of-results"></a>Desteklenen sonuç türleri

* **Nokta Adresi**: Sokak adı ve numarası olan belirli bir adrese sahip haritadaki noktalar. Nokta Adresi, adresler için en yüksek doğruluk düzeyini sağlar. 

* **Adres Aralığı**: Sokağın başından ve sonundan enterpolasyona tabi olan adres noktalarıaralığı.  

* **Coğrafya**: Bir arazinin, örneğin, ülke, eyalet veya şehir gibi idari bölümlerini temsil eden haritadaki alanlar. 

* **POI**: Dikkat edilmesi gereken ve ilginç olabilecek bir haritadaki noktalar.

* **Sokak**: Haritadaki sokaklar. Adresler, adresi içeren sokağın enlem ve boylam koordinatlarına göre çözülür. Ev numarası işlenmemiş olabilir. 

* **Cross Street**: Kavşaklar. Çapraz sokaklar, iki caddenin kesiştiği kavşakları temsil eder.

### <a name="response"></a>Yanıt

Tepki yapısına bakalım. Aşağıdaki yanıtta, sonuç nesnelerinin türleri farklıdır. Dikkatlice bakarsanız, üç tür sonuç nesnesi görürsünüz:

* Nokta Adresi
* Sokak
* Çapraz Sokak

Adres aramasının İçN'leri döndürmediğini unutmayın.  

Her `Score` yanıt nesnesinin parametresi, eşleşen skorun aynı yanıttaki diğer nesnelerin puanlarıyla nasıl ilişkili olduğunu gösterir. Yanıt nesnesi parametreleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress)

```JSON
{
    "summary": {
        "query": "400 broad street seattle wa",
        "queryType": "NON_NEAR",
        "queryTime": 146,
        "numResults": 6,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/28725082",
            "score": 9.893799781799316,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/6700384",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61724,
                "lon": -122.35207
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61825,
                    "lon": -122.35336,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61626,
                    "lon": -122.35078,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/9701953",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61965,
                "lon": -122.349
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62066,
                    "lon": -122.35041,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.34761,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/11721297",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Downtown Seattle, Denny Regrade, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98121",
                "extendedPostalCode": "981211237",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98121"
            },
            "position": {
                "lat": 47.61825,
                "lon": -122.35078
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61857,
                    "lon": -122.35078,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61825,
                    "lon": -122.35041,
                    "valid": true
                }
            }
        },
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/232144",
            "score": 6.754479885101318,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
            },
            "position": {
                "lat": 47.62545,
                "lon": -122.33974
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62635,
                    "lon": -122.34107,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62455,
                    "lon": -122.33841,
                    "valid": true
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>Geometri

*Geometri'nin* yanıt türü, altındaki `dataSources` `geometry` nesnede döndürülen geometri `id`kimliğini ve . Örneğin, GeoJSON biçiminde geometri verilerini istemek için [Arama Çokgen hizmetini](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) kullanabilirsiniz. Bu biçimi kullanarak, bir dizi varlık için bir şehir veya havaalanı anahattı alabilirsiniz. Daha sonra geometri içinde [bir geofence](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) veya [Arama İçN'ler](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)kurmak için bu sınır verilerini kullanabilirsiniz.


[Arama Adresi](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) API'si veya [Arama Bulanıkı](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API'si için yanıtlar, `geometry` aşağıdaki `id` `dataSources` nesnede döndürülen geometri kimliğini içerebilir:


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için lütfen bkz:

> [!div class="nextstepaction"]
> [Azure Haritalar Arama Hizmeti istekleri nasıl oluşturur?](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address)

> [!div class="nextstepaction"]
> [Arama Hizmeti API belgeleri](https://docs.microsoft.com/rest/api/maps/search)